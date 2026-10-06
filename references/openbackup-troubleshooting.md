# If `Error: Environment variable 'OPENSQL_HOME' is not set.` Is Displayed During Installation <a href="#opensql-home-not-set" id="opensql-home-not-set"></a>

The environment variables required by the installation script are not set. Set all three of `OPENSQL_HOME`, `PG_HOME`, and `PG_DATA_DIR`, and then run the script again.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

# If the Server Is Not Displayed in the Backup Server List <a href="#server-missing-from-backup-list" id="server-missing-from-backup-list"></a>

The OwlDB Agent is not registered with the OwlDB server.

```bash
systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
cat /var/lib/barman/owlagent_dist/owlagent.env
```

Check that `AGENT_TYPE` is `barman` and that `IP` and `PORT` match the OwlDB server address. If you modify them, stop the agent with `owlagent_stop.sh` and then run `owlagent_start.sh` again.

# If Integration Fails at the Barman Agent Startup Step <a href="#barman-agent-fails-during-integration" id="barman-agent-fails-during-integration"></a>

The `barman` account has no NOPASSWD sudo configuration.

```bash
su - barman -c 'sudo -n true' && echo OK
```

If `OK` is not displayed, check whether the `sudo` package is installed (`rpm -q sudo`) and whether the `/etc/sudoers.d/barman` file exists.

# If Integration Fails with an Error That `barman-wal-archive` Does Not Exist <a href="#barman-wal-archive-missing" id="barman-wal-archive-missing"></a>

The WAL retention method is set to `archiver` or `archiver+streaming`, but `barman-cli` is not installed on the database server. Install it by referring to 1-2.

# If the Barman Agent Does Not Start <a href="#barman-agent-not-starting" id="barman-agent-not-starting"></a>

The name of the systemd template unit or the configuration file path may differ from OwlDB's fixed value.

```bash
systemctl cat barman-agent@
```

Check that the unit name is `barman-agent@.service` and that `ExecStart` reads `/var/lib/barman/agent/%i.config.yml`. This path is fixed even if `barman_home` is set to a different path.

You may not have run `systemctl daemon-reload` after writing the unit file. Because `systemctl cat` reads the file directly, its output looks normal even in this case.

```bash
systemctl daemon-reload
```

# If Health Is Displayed as Disconnected <a href="#health-not-connected" id="health-not-connected"></a>

If the **WAL archive** and **continuous archiving** items of `barman check` fail, Health is displayed as Disconnected. This means WAL is not being sent from the database server to the Barman server.

Check the failed items on the OpenBackup server.

```bash
su - barman
barman check <database ID>
```

If `WAL archive: FAILED` or `continuous archiving: FAILED` is displayed, check whether the database server can connect to the OpenBackup server. Run the following **as the OpenSQL account on the database server**.

```bash
ssh -o BatchMode=yes -i <node registration key> barman@<Barman server IP> hostname
```

| Output | Cause and action |
| --- | --- |
| `Permission denied (publickey)` | The OpenBackup server does not allow this key. Check the `authorized_keys` registration in 7-1 of the **OpenBackup Server Installation** document |
| `REMOTE HOST IDENTIFICATION HAS CHANGED` | The database server still remembers the host key of the previous OpenBackup server. See the section below |
| OpenBackup server hostname | This direction is working. Also check the opposite direction below |

Also check the opposite direction (OpenBackup server → database server). This direction corresponds to the `ssh` item of `barman check`.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

If the connection fails, check whether the private key file path matches `BARMAN_SSH_KEYPATH` in `owlagent.env` and whether the database server allows the key.

# If Health Is Displayed as Disconnected After Rebuilding the OpenBackup Server <a href="#health-not-connected-after-rebuild" id="health-not-connected-after-rebuild"></a>

The database server still remembers the host key of the previous OpenBackup server and refuses the connection. This happens when the OpenBackup server is reinstalled with the same IP address.

When you try to connect from the database server, the following message is displayed.

```bash
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
Host key verification failed.
```

Remove the entry **on all database nodes**, and then integrate again.

```bash
ssh-keygen -R <Barman server IP>
```

The connection settings created by OwlDB automatically accept a server's host key only on the first connection. If the host key has **changed**, you must remove it manually as shown above.

# If WAL Is Not Collected and `pg_receivewal not present in $PATH` Appears in the Log <a href="#pg-receivewal-not-in-path" id="pg-receivewal-not-in-path"></a>

`path_prefix` is not set in `/etc/barman.conf`. Set it to the directory of the PostgreSQL client executables. Because cron rereads the configuration file each time, you do not need to restart the service.

```bash
[barman]
path_prefix = /opt/postgresql/bin
```

# If a Barman Version Error Occurs During Backup <a href="#barman-version-error" id="barman-version-error"></a>

The versions of `barman` on the OpenBackup server and `barman-cli` on the database server differ. Check that both are `3.11.1`.

```bash
# Barman server
barman --version

# Database server
rpm -q barman-cli
```
