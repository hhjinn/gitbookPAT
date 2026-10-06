# If `Error: Environment variable 'OPENSQL_HOME' is not set.` Is Displayed During Installation <a href="#opensql-home-not-set" id="opensql-home-not-set"></a>

The environment variables required by the installation script are not set. `OPENSQL_HOME`, `PG_HOME`, `PG_DATA_DIR` Set all three and run again.

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

`AGENT_TYPE` If `barman` whether it is, `IP` and `PORT` Check whether it matches the OwlDB server address. If modified, `owlagent_stop.sh` stop it with, then `owlagent_start.sh` run again.

# If Integration Fails at the Barman Agent Startup Step <a href="#barman-agent-fails-during-integration" id="barman-agent-fails-during-integration"></a>

`barman` There is no NOPASSWD sudo configuration for the account.

```bash
su - barman -c 'sudo -n true' && echo OK
```

`OK` If is not displayed `sudo` Check whether the package is installed (`rpm -q sudo`) and `/etc/sudoers.d/barman` verify the file exists.

# If Integration Fails with an Error That `barman-wal-archive` Does Not Exist <a href="#barman-wal-archive-missing" id="barman-wal-archive-missing"></a>

WAL retention method `archiver` or `archiver+streaming` You selected, but on the database server `barman-cli` is not installed. Refer to 1-2 to install it.

# If the Barman Agent Does Not Start <a href="#barman-agent-not-starting" id="barman-agent-not-starting"></a>

The name of the systemd template unit or the configuration file path may differ from OwlDB's fixed value.

```bash
systemctl cat barman-agent@
```

whether the unit name is `barman-agent@.service` whether it is, `ExecStart` is `/var/lib/barman/agent/%i.config.yml` Check whether it reads. `barman_home` Even if is set to a different path, this path is fixed.

After writing the unit file, `systemctl daemon-reload` you may not have run. `systemctl cat` reads the file directly, so it is displayed normally even in this case.

```bash
systemctl daemon-reload
```

# If Health Is Displayed as Disconnected <a href="#health-not-connected" id="health-not-connected"></a>

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

# If Health Is Displayed as Disconnected After Rebuilding the OpenBackup Server <a href="#health-not-connected-after-rebuild" id="health-not-connected-after-rebuild"></a>

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

# If WAL Is Not Collected and `pg_receivewal not present in $PATH` Appears in the Log <a href="#pg-receivewal-not-in-path" id="pg-receivewal-not-in-path"></a>

`/etc/barman.conf` in `path_prefix` is not set. Set it to the path of the PostgreSQL client executable. Since cron re-reads the configuration file each time, you do not need to restart the service.

```bash
[barman]
path_prefix = /opt/postgresql/bin
```

# If a Barman Version Error Occurs During Backup <a href="#barman-version-error" id="barman-version-error"></a>

The OpenBackup server's `barman` and the database server's `barman-cli` versions differ. Verify that both are `3.11.1` on both sides.

```bash
# Barman server
barman --version

# Database server
rpm -q barman-cli
```
