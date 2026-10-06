### During installation `Error: Environment variable 'OPENSQL_HOME' is not set.` if is displayed <a href="#error-environment-variable-opensql_home-is-not-set" id="error-environment-variable-opensql_home-is-not-set"></a>

The environment variables required by the installation script are not set. `OPENSQL_HOME`, `PG_HOME`, `PG_DATA_DIR` Set all three and run again.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

### If the server is not displayed in the backup server list <a href="#undefined" id="undefined"></a>

The OwlDB Agent is not registered with the OwlDB server.

```bash
systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
cat /var/lib/barman/owlagent_dist/owlagent.env
```

`AGENT_TYPE` If `barman` whether it is, `IP` and `PORT` Check whether it matches the OwlDB server address. If modified, `owlagent_stop.sh` stop it with, then `owlagent_start.sh` run again.

### If it fails at the Barman Agent startup step during integration <a href="#barman-agent" id="barman-agent"></a>

`barman` There is no NOPASSWD sudo configuration for the account.

```bash
su - barman -c 'sudo -n true' && echo OK
```

`OK` If is not displayed `sudo` Check whether the package is installed (`rpm -q sudo`) and `/etc/sudoers.d/barman` verify the file exists.

### During integration, `barman-wal-archive` If it fails with an error that does not exist <a href="#barman-wal-archive" id="barman-wal-archive"></a>

WAL retention method `archiver` or `archiver+streaming` You selected, but on the database server `barman-cli` is not installed. Refer to 1-2 to install it.

### If the Barman Agent does not start <a href="#barman-agent-1" id="barman-agent-1"></a>

The name of the systemd template unit or the configuration file path may differ from OwlDB's fixed value.

```bash
systemctl cat barman-agent@
```

whether the unit name is `barman-agent@.service` whether it is, `ExecStart` is `/var/lib/barman/agent/%i.config.yml` Check whether it reads. `barman_home` Even if is set to a different path, this path is fixed.

After writing the unit file, `systemctl daemon-reload` you may not have run. `systemctl cat` reads the file directly, so it is displayed normally even in this case.

```bash
systemctl daemon-reload
```

### If Health is displayed as Not Connected <a href="#health" id="health"></a>

`barman check` Among the items, **WAL archive** and **continuous archiving** If fails, Health is displayed as Not Connected. WAL is not being transmitted from the database server to the Barman server.

Check the failed items on the OpenBackup server.

```bash
su - barman
barman check <database ID>
```

`WAL archive: FAILED` or `continuous archiving: FAILED` If is displayed, check whether the database server can connect to the OpenBackup server. **With the OpenSQL account on the database server,** run.

```bash
ssh -o BatchMode=yes -i <node registration key> barman@<Barman server IP> hostname
```

| Output | Cause and action |
| --- | --- |
| `Permission denied (publickey)` | The OpenBackup server does not allow this key. **OpenBackup** **Server installation** In document 7-1, `authorized_keys` Check the registration |
| `REMOTE HOST IDENTIFICATION HAS CHANGED` | The database server remembers the host key of the previous OpenBackup server. Refer to the items below |
| OpenBackup server hostname | This direction is normal. Check the opposite direction below as well |

Also check the opposite direction (OpenBackup server → database server). This direction `barman check` of `ssh` corresponds to the item.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

If the connection fails, check whether the private key file path is `owlagent.env` of `BARMAN_SSH_KEYPATH` the same as, and whether the database server allows the key.

### When Health shows as Disconnected after rebuilding the OpenBackup server <a href="#openbackup-health" id="openbackup-health"></a>

The database server remembers the host key of the previous OpenBackup server and refuses the connection. This occurs when the OpenBackup server is reinstalled with the same IP.

When you attempt to connect from the database server, the following is displayed.

```bash
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
Host key verification failed.
```

**On all database nodes,** remove the relevant entry and then reconnect.

```bash
ssh-keygen -R <Barman server IP>
```

The connection configuration created by OwlDB only automatically accepts the host key of a server on first connection, so when the host key has been **changed,** you must clean it up manually as shown above.

### WAL is not collected and the log shows `pg_receivewal not present in $PATH` if is displayed <a href="#wal-pg_receivewal-not-present-in-usdpath" id="wal-pg_receivewal-not-present-in-usdpath"></a>

`/etc/barman.conf` in `path_prefix` is not set. Set it to the path of the PostgreSQL client executable. Since cron re-reads the configuration file each time, you do not need to restart the service.

```bash
[barman]
path_prefix = /opt/postgresql/bin
```

### When a Barman version-related error occurs during backup <a href="#barman" id="barman"></a>

The OpenBackup server's `barman` and the database server's `barman-cli` versions differ. Verify that both are `3.11.1` on both sides.

```bash
# Barman server
barman --version

# Database server
rpm -q barman-cli
```
