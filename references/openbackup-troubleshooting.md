# When `Error: Environment variable 'OPENSQL_HOME' is not set.` is output during installation <a href="#opensql-home-not-set" id="opensql-home-not-set"></a>

The environment variables required by the installation script are not set. Set all three — `OPENSQL_HOME`, `PG_HOME`, `PG_DATA_DIR` — and then run it again.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

# When the server is not displayed in the backup server list <a href="#server-missing-from-backup-list" id="server-missing-from-backup-list"></a>

The OwlDB Agent is not registered with the OwlDB server.

```bash
systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
cat /var/lib/barman/owlagent_dist/owlagent.env
```

Check whether `AGENT_TYPE` is `barman`, and whether `IP` and `PORT` match the OwlDB server address. If you made changes, stop it with `owlagent_stop.sh` and then run `owlagent_start.sh` again.

# When it fails at the Barman Agent startup step during integration <a href="#barman-agent-fails-during-integration" id="barman-agent-fails-during-integration"></a>

There is no NOPASSWD sudo configuration for the `barman` account.

```bash
su - barman -c 'sudo -n true' && echo OK
```

If `OK` is not output, check whether the `sudo` package is installed (`rpm -q sudo`) and whether the `/etc/sudoers.d/barman` file exists.

# When integration fails with an error that `barman-wal-archive` does not exist <a href="#barman-wal-archive-missing" id="barman-wal-archive-missing"></a>

The WAL retention method was selected as `archiver` or `archiver+streaming`, but `barman-cli` is not installed on the database server. Install it by referring to 1-2.

# When the Barman Agent does not start <a href="#barman-agent-not-starting" id="barman-agent-not-starting"></a>

The name of the systemd template unit or the configuration file path may differ from OwlDB's fixed value.

```bash
systemctl cat barman-agent@
```

Check whether the unit name is `barman-agent@.service` and whether `ExecStart` reads `/var/lib/barman/agent/%i.config.yml`. Even if you set `barman_home` to a different path, this path is fixed.

You may not have run `systemctl daemon-reload` after creating the unit file. `systemctl cat` reads the file directly, so it still produces normal output in this case.

```bash
systemctl daemon-reload
```

# When Health is displayed as not connected <a href="#health-not-connected" id="health-not-connected"></a>

If **WAL archive** and **continuous archiving** among the `barman check` items fail, Health is displayed as not connected. This is a state where WAL is not being transmitted from the database server to the Barman server.

Check the failed items on the OpenBackup server.

```bash
su - barman
barman check <database ID>
```

If `WAL archive: FAILED` or `continuous archiving: FAILED` is output, check whether the database server can connect to the OpenBackup server. Run this **as the OpenSQL account on the database server**.

```bash
ssh -o BatchMode=yes -i <node registration key> barman@<Barman server IP> hostname
```

| Output | Cause and action |
| --- | --- |
| `Permission denied (publickey)` | The OpenBackup server does not allow this key. Check the `authorized_keys` registration in section 7-1 of the **OpenBackup** **server installation** document. |
| `REMOTE HOST IDENTIFICATION HAS CHANGED` | The database server remembers the host key of the previous OpenBackup server. Refer to the item below. |
| OpenBackup server hostname | This direction is normal. Also check the opposite direction below. |

Also check the opposite direction (OpenBackup server → database server). This direction corresponds to the `ssh` item of `barman check`.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

If the connection fails, check whether the private key file path matches `BARMAN_SSH_KEYPATH` in `owlagent.env` and whether the database server allows that key.

# When Health is displayed as not connected after rebuilding the OpenBackup server <a href="#health-not-connected-after-rebuild" id="health-not-connected-after-rebuild"></a>

The database server remembers the host key of the previous OpenBackup server and refuses the connection. This occurs when the OpenBackup server is reinstalled with the same IP.

When you attempt to connect from the database server, the output is as follows.

```bash
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
Host key verification failed.
```

Remove the relevant item **on all database nodes** and then integrate again.

```bash
ssh-keygen -R <Barman server IP>
```

The connection settings created by OwlDB automatically accept only the host key of the server on first connection, so when the host key has **changed**, you must clean it up manually as shown above.

# When WAL is not collected and `pg_receivewal not present in $PATH` is output in the log <a href="#pg-receivewal-not-in-path" id="pg-receivewal-not-in-path"></a>

`path_prefix` is not set in `/etc/barman.conf`. Set it to the PostgreSQL client executable path. Since cron re-reads the configuration file each time, you do not need to restart the service.

```bash
[barman]
path_prefix = /opt/postgresql/bin
```

# When a Barman version-related error occurs during backup <a href="#barman-version-error" id="barman-version-error"></a>

The `barman` version on the OpenBackup server differs from the `barman-cli` version on the database server. Check that both are `3.11.1`.

```bash
# Barman server
barman --version

# Database server
rpm -q barman-cli
```
