Proceed once the system and network requirements in [Preparing the OpenBackup server environment](openbackup-prerequisites.md) have been met.
Completing the procedure below brings the OpenBackup server to a state where it can be integrated with OwlDB.

{% hint style="info" %}
**Note**

The per-database configuration files and the Agent process are created automatically by OwlDB at the time of integration, so you do not need to write them yourself.
{% endhint %}

# OpenBackup installation <a href="#install-openbackup" id="install-openbackup"></a>

## 1. OpenBackup installation <a href="#install-openbackup-package" id="install-openbackup-package"></a>

Install OpenBackup as version `3.1.1`. The `barman-cli` installed on the database server must likewise be `3.11.1`.

| Installation location | Package | Provided commands | Role |
| --- | --- | --- | --- |
| OpenBackup server | `barman` | `barman put-wal`, `barman get-wal` | Receives and stores WAL |
| Database server | `barman-cli` | `barman-wal-archive`, `barman-wal-restore` | Transfers WAL |

{% hint style="warning" %}
**Caution**

If you install without specifying a version, as in `pip install barman`, the latest version is installed, which conflicts with `3.11.1`.

In this case, the versions on the WAL-sending side and the WAL-receiving side do not match, causing integration and backup to fail.
{% endhint %}

### 1-1. Decompress the distribution package <a href="#id-1-1" id="id-1-1"></a>

Decompress the OpenSQL distribution to the desired path.

```bash
mkdir -p /opt/opensql-src
tar xzf <OpenSQL distribution>.tar.gz -C /opt/opensql-src --strip-components=1
```

### 1-2. Set installation path environment variables <a href="#id-1-2" id="id-1-2"></a>

The installation script requires the following environment variables, so **set them beforehand**.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

| Item | Description | Example | Required or Not |
| --- | --- | --- | --- |
| OPENSQL_HOME | OpenSQL installation path | `/opt/postgresql` | Required |
| PG_HOME | PostgreSQL installation path. The executables are placed in `<PG_HOME>/bin` | `/opt/postgresql` | Required |
| PG_DATA_DIR | PostgreSQL data path | `/opt/postgresql/data` | Required |

If not set, the following error is output in the next step and the installation does not proceed.

```bash
Error: Environment variable 'OPENSQL_HOME' is not set.
```

The value specified for `PG_HOME` must be the same as the `path_prefix` (`<PG_HOME>/bin`) in step 3.

### 1-3. Proceed with installation <a href="#id-1-3" id="id-1-3"></a>

```bash
cd /opt/opensql-src/scripts
. ./setenv.sh "$(pwd)"
./install.sh postgresql
./install.sh barman
```

On the OpenBackup server, the PostgreSQL service is not started. Only client utilities such as `pg_receivewal` and `pg_basebackup` are used.

### 1-4. Verify version <a href="#id-1-4" id="id-1-4"></a>

`3.11.1` should be output.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

## 2. Configure the OpenBackup execution account <a href="#openbackup-account" id="openbackup-account"></a>

### 2-1. Create an account <a href="#id-2-1" id="id-2-1"></a>

Create a dedicated OS account to run OpenBackup and grant it NOPASSWD sudo privileges.

```bash
groupadd -g 1001 barman
useradd -u 1001 -g 1001 -m -d /var/lib/barman -s /bin/bash barman

echo "barman ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/barman
chmod 440 /etc/sudoers.d/barman
```

| Item | Description | Example | Required or Not |
| --- | --- | --- | --- |
| Account | Dedicated OS account for running OpenBackup | `barman` | Required |
| Home directory | Account home directory. Backup data is stored under this | `/var/lib/barman` | Required |
| sudo privileges | NOPASSWD sudo | `barman ALL=(ALL) NOPASSWD: ALL` | Required |

OwlDB controls the Barman Agent with `sudo systemctl restart barman-agent@<database ID>`. Since this command must run without a password prompt, NOPASSWD sudo privileges are required.

The OpenBackup server operates with this single account. Performing backups, running the OwlDB Agent, and connecting to the database server are all done with the same account.

### 2-2. Set PATH

Add to the PATH so that PostgreSQL client commands such as `pg_receivewal` and `pg_basebackup` can be run from the `barman` account without a path. Write it to match the `PG_HOME` value from 1-2.

```bash
su - barman
echo 'export PATH=$PATH:/opt/postgresql/bin' >> ~/.bashrc
source ~/.bashrc
exit
```

This setting is used when an operator runs commands directly. Since the Barman cron does not use this PATH, the `path_prefix` in step 3 must be set separately.

## 3. OpenBackup global configuration <a href="#openbackup-global-settings" id="openbackup-global-settings"></a>

Write the `/etc/barman.conf` file as follows.

```bash
[barman]
path_prefix = /opt/postgresql/bin
barman_user = barman
configuration_files_directory = /etc/barman.d
barman_home = /var/lib/barman
log_file = /var/log/barman/barman.log
log_level = INFOb
```

| Item | Description | Example | Required or Not |
| --- | --- | --- | --- |
| path_prefix | PostgreSQL client executable path | `/opt/postgresql/bin` | Required |
| barman_user | Barman execution account | `barman` | Required |
| configuration_files_directory | Per-database configuration file directory. Set it to the same value as `BARMAN_CONF_DIR` of the OwlDB Agent | `/etc/barman.d` | Required |
| barman_home | Backup data storage path | `/var/lib/barman` | Required |
| log_file | Log file path | `/var/log/barman/barman.log` | Required |
| log_level | Log level | `INFO` | Required |

`path_prefix` must be set. Since Barman cron runs in a minimal PATH environment, the PATH configured in the account's `.bashrc` or similar is not applied. Without `path_prefix`, cron cannot find `pg_receivewal`, causing WAL collection to fail permanently.

Create the directories used in the configuration and assign ownership.

```bash
mkdir -p /var/log/barman /etc/barman.d /var/lib/barman/agent

chown -R barman:barman /var/lib/barman /var/log/barman /etc/barman.d
chmod 700 /var/lib/barman
```

| Directory | Purpose |
| --- | --- |
| `/etc/barman.d` | Per-database OpenBackup configuration file. Created by OwlDB during integration |
| `/var/lib/barman/agent` | Per-database Agent configuration file. Created by OwlDB during integration |
| `/var/log/barman` | Barman log |

Backup data is stored in `<barman_home>/<database ID>`. However, the Agent configuration file path in step 4 is fixed regardless of `barman_home`.

## 4. Registering the Barman Agent <a href="#register-barman-agent" id="register-barman-agent"></a>

The Barman Agent is a process that detects leader changes in the Patroni cluster and updates the Barman configuration. It uses `barman_agent/server.py` from the OpenSQL distribution.

### 4-1. Placing the executable <a href="#id-4-1" id="id-4-1"></a>

```bash
cp /opt/opensql-src/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r /opt/opensql-src/barman_agent/requirements.txt
```

### 4-2. Registering the systemd template unit

Since a single OpenBackup server can handle multiple databases, register it as a systemd **template unit** so that an Agent is started per database. The OwlDB database ID is passed to the instance name (`%i`).

Write the `/usr/lib/systemd/system/barman-agent@.service` file as follows.

```bash
[Unit]
Description=Barman Agent for %i
After=network.target

[Service]
Type=simple
User=barman
Group=barman
ExecStart=/usr/local/bin/barman-agent /var/lib/barman/agent/%i.config.yml
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Since a new unit file was written, apply it to systemd.

```bash
systemctl daemon-reload
```

The two values below are fixed in OwlDB, so use them as they are.

| Item | Value | Whether changeable |
| --- | --- | --- |
| Unit name | `barman-agent@.service` | Not allowed |
| Agent configuration file path | `/var/lib/barman/agent/<Database ID>.config.yml` | Not allowed |

Even if `barman_home` is set to a path other than `/var/lib/barman`, the Agent configuration file path is fixed at `/var/lib/barman/agent/`. If you write the `ExecStart` path based on `barman_home`, the Agent will not start because it cannot read the configuration file created by OwlDB.

{% hint style="info" %}
**Note**

The contents of the configuration file are created by OwlDB at the time of integration. In this step, only write the unit file and do not start it.
{% endhint %}

## 5. Registering the Barman cron <a href="#register-barman-cron" id="register-barman-cron"></a>

Register a cron job so that WAL collection and archiving are performed at one-minute intervals.

Write the `/etc/cron.d/barman-cron` file as follows.

```bash
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now sshd crond
```

Start `sshd` together. When the database server transmits the WAL archive, it connects to the OpenBackup server's `sshd`. On servers that are already running, it remains as is.

## 6. Installing the OwlDB Agent <a href="#install-owldb-agent" id="install-owldb-agent"></a>

The OwlDB Agent receives commands from the OwlDB server and performs tasks on the OpenBackup server. Since all integration tasks on the OpenBackup server are carried out through this Agent, be sure to install it.

### 6-1. Extracting the distribution file <a href="#id-6-1" id="id-6-1"></a>

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

### 6-2. Configuring owlagent.env

Write the `/var/lib/barman/owlagent_dist/owlagent.env` file as follows.

```bash
AGENT_TYPE=barman
IP=192.168.0.10
PORT=8080
USERNAME=barman
OPENSQL_HOME=/opt/postgresql
BARMAN_NAME=barman01
BARMAN_SSH_USER=barman
BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/id_rsa
BARMAN_SSH_IP=192.168.0.20
BARMAN_SSH_PORT=22
BARMAN_CONF_DIR=/etc/barman.d
```

| Item | Description | Example | Required or Not |
| --- | --- | --- | --- |
| AGENT_TYPE | Agent type. For a Barman server, enter `barman` | `barman` | Required |
| IP | OwlDB server IP | `192.168.0.10` | Required |
| PORT | OwlDB server port | `8080` | Required |
| USERNAME | Agent execution account | `barman` | Required |
| OPENSQL_HOME | PostgreSQL and OpenBackup installation path | `/opt/postgresql` | Required |
| BARMAN_NAME | OpenBackup server name displayed in OwlDB | `barman01` | Required |
| BARMAN_SSH_USER | OpenBackup server SSH connection account | `barman` | Required |
| BARMAN_SSH_KEYPATH | Private key path used when the OpenBackup server connects to the database server. In this step, only specify the path; the actual key file is placed in 7-2 | `/var/lib/barman/.ssh/id_rsa` | Required |
| BARMAN_SSH_IP | OpenBackup server IP | `192.168.0.20` | Required |
| BARMAN_SSH_PORT | OpenBackup server SSH port | `22` | Required |
| BARMAN_CONF_DIR | Per-database configuration file directory. Enter the same value as `configuration_files_directory` in `barman.conf` | `/etc/barman.d` | Required |

{% hint style="warning" %}
**Caution**

If `AGENT_TYPE` is not `barman`, OwlDB will not recognize it as an OpenBackup server, and it will not appear in the **backup server** list.
{% endhint %}

## 7. SSH configuration <a href="#ssh-settings" id="ssh-settings"></a>

The OpenBackup server and the database server require **bidirectional** SSH connections. The preparations differ for each direction.

| Direction | Purpose | Preparation |
| --- | --- | --- |
| Database server → OpenBackup server | WAL archive transmission (`barman-wal-archive`) | The OpenBackup server allows connections with that key (7-1) |
| OpenBackup server → database server | Data file transmission during recovery, `rsync` backup method | Place authentication information on the OpenBackup server (7-2) |

Which **key** the database server uses to connect to the OpenBackup server is configured automatically by OwlDB during integration. It records in the database server's `~/.ssh/config` to use the SSH key entered during node registration. However, since making the **OpenBackup** **server allow that key is not automatic**, you must register it manually in 7-1.

### 7-1. Allowing connection from the database server to the OpenBackup server

Register the public key of the key used during database node registration in the `authorized_keys` of the OpenBackup execution account.

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <public key of the database node registration key> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

Without this registration, transmitting WAL from the database server will fail with `Permission denied (publickey)`. In that case, during integration, the **WAL archive** and **continuous archiving** items of `barman check` will fail, the integration will be rolled back, and **Health** will be displayed as **not connected**.

### 7-2. Configuring connection from the OpenBackup server to the database server

Configure it so that connection from the OpenBackup execution account to the OpenSQL account of the database server is established **without entering a password**.

Place the private key file in a location that the OpenBackup execution account can read.

```bash
cp <private key file> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

**You can freely decide the file name and path.** Just enter the path where you placed it exactly into `BARMAN_SSH_KEYPATH` in 6-2.

```bash
# Example of using a different name
cp <key file> /var/lib/barman/.ssh/<key name>
chmod 600 /var/lib/barman/.ssh/<key name>
chown barman:barman /var/lib/barman/.ssh/<key name>

# Enter the same path in owlagent.env
# BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/<key name>
```

There is **no restriction on the format and extension** of the key file. Whether it is in OpenSSH format (`-----BEGIN OPENSSH PRIVATE KEY-----`) or PEM format, and whether its name is `id_rsa` or `.pem`, it can be used.

The database server must allow connection with that key.

However, **the private key file itself must exist.** During integration, OwlDB generates a connection command of the form `ssh -i <BARMAN_SSH_KEYPATH>` in the Barman configuration file, and this value must be an **absolute path**. Therefore, methods that do not use a key file (ssh-agent, password authentication, etc.) cannot be used.

### 7-3. Registering the host key <a href="#id-7-3" id="id-7-3"></a>

Connect to the database server for the first time using the OpenBackup execution account to register the host key. If there are multiple database servers, perform this **for all nodes**.

```bash
su - barman
ssh -o StrictHostKeyChecking=accept-new <OpenSQL account>@<database server IP> hostname
```

It is normal if the database server's hostname is output.

This registration is needed for the verification command in 7-4 and when an operator connects directly. When OpenBackup connects during backup/recovery tasks, it is unaffected because the connection command in the configuration file created by OwlDB includes an option to skip host key checking.

### 7-4. Verifying the connection <a href="#id-7-4" id="id-7-4"></a>

Connection from the OpenBackup server to the database server must be established without a password or confirmation prompt. If there are multiple database servers, perform this for all nodes.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

| Output | Meaning |
| --- | --- |
| Database server hostname | Normal |
| `Host key verification failed` | The host key for 7-3 has not been registered |
| `Permission denied (publickey)` | The database server does not accept the key deployed in 7-2 |

For the reverse direction (database server → OpenBackup server), **this direction is handled automatically by OwlDB during integration, so no prior verification is needed.** At the time of integration, OwlDB writes the connection settings to the database server's `~/.ssh/config`, and these settings include an option that automatically accepts the host key of the OpenBackup server on first connection. Therefore, if you attempt a direct connection in this direction before integration, the host key will not be registered and `Host key verification failed` will be output, which does not mean the installation is faulty.

The only thing you need to prepare directly in this direction is the `authorized_keys` registration in 7-1. If this registration is missing, it will surface at integration time as failures of the **WAL archive** and **continuous archiving** items in `barman check`.

---

# Verifying OpenBackup Server Integration <a href="#check-openbackup-connection" id="check-openbackup-connection"></a>

Proceed with the OpenBackup server installation already completed.

## 1. Database Server Configuration <a href="#configure-database-server" id="configure-database-server"></a>

In addition to the OpenBackup server, the following items must also be prepared on the database server.

| Item | Description | Required or Not |
| --- | --- | --- |
| `barman-cli` | Install the same `3.11.1` version as the OpenBackup server | Required when using the WAL retention method as `archiver` or `archiver+streaming` |
| `rsync` | Used for transferring data files during recovery | Required when using recovery |
| Database account | The account entered during OpenSQL installation must exist as a superuser | Required |

OpenBackup connects to the database with the account entered during OpenSQL installation. If this account is a superuser, it already holds replication and backup privileges, so there is no need to create a separate dedicated account.

### 1-1. Verifying barman-cli Installation

```bash
rpm -q barman-cli
barman-wal-archive --version
```

If it is installed, the output will be as follows.

```bash
$ barman-wal-archive --version
barman-wal-archive 3.11.1
```

If it is not installed, the output will be as follows.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

`barman-cli` is a separate package from OpenSQL, so it may not be present even on a server where OpenSQL is installed. Be sure to verify this before integration.

When integrating with the WAL retention method as `archiver` or `archiver+streaming`, OwlDB will refuse integration if `barman-wal-archive` is not present on the database server. If the WAL retention method is `streaming`, `barman-cli` is not required.

### 1-2. Installing barman-cli

`barman-cli` is provided by the official PostgreSQL repository (PGDG).

In an environment with internet access, register the repository and then specify the version to install.

```bash
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

dnf install -y rsync
dnf --enablerepo=pgdg-common install -y barman-cli-3.11.1
```

In an environment without internet access, obtain the rpm file of the same version in advance and install it.

```bash
dnf install -y ./barman-cli-3.11.1-*.rpm
```

{% hint style="warning" %}
**Caution**

If you install with `dnf install barman-cli` or `pip install barman-cli` without specifying a version, the latest version will be installed and will not match the Barman server's version. Be sure to specify the version, as in `barman-cli-3.11.1`.
{% endhint %}

## 2. Verifying OpenBackup Server Installation Results <a href="#check-openbackup-installation" id="check-openbackup-installation"></a>

Check on the OpenBackup server with the following commands.

```bash
# Barman version
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)

# Global configuration
$ cat /etc/barman.conf

# Agent template unit
$ systemctl cat barman-agent@

# Barman cron
$ cat /etc/cron.d/barman-cron

# OwlDB Agent
$ systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
active
active

# Service status
$ systemctl is-active sshd crond
active
active

# NOPASSWD sudo for the barman account
$ su - barman -c 'sudo -n true' && echo OK
OK
```

| Item to verify | Normal result |
| --- | --- |
| `barman --version` | `3.11.1` |
| `/etc/barman.conf` | `path_prefix`, `configuration_files_directory` are set |
| `barman-agent@` unit | The unit exists, and `ExecStart` references `/var/lib/barman/agent/%i.config.yml` |
| `/etc/cron.d/barman-cron` | `barman cron` registered on a 1-minute cycle |
| `owldb-barman-agent.service` / `.timer` | All `active` |
| `sshd`, `crond` | All `active` |
| NOPASSWD sudo | `OK` output |

{% hint style="warning" %}
**Caution**

Do not skip verifying the NOPASSWD sudo. Without this setting, the installation may appear complete, but it will fail at the stage where OwlDB starts the Barman Agent during integration.
{% endhint %}

## 3. OwlDB Integration <a href="#connect-owldb" id="connect-owldb"></a>

Integrate in the following order from the OwlDB web UI.

1. Go to the **OpenBackup Settings** screen for the database.
2. Check whether the installed Barman server appears in the **Backup Servers** list. If it appears, it is ready for integration.
3. Select the **Backup Server**, **OpenBackup Agent Port**, **WAL retention method**, and **backup method** to perform the integration.
4. After integration, check whether **Health** is displayed as **Connected**.

If **Health** is **Connected**, the installation and integration are complete.

---

### Note: Tasks OwlDB performs automatically during integration <a href="#owldb" id="owldb"></a>

The following items are automatically created or configured by OwlDB at integration time, so you do not write them yourself.

| Target | Location | Content |
| --- | --- | --- |
| Barman configuration file | Barman server `/etc/barman.d/<database ID>.conf` | Database connection information, WAL retention method, backup method, `backup_directory` |
| Agent configuration file | Barman server `/var/lib/barman/agent/<database ID>.config.yml` | Cluster name, Patroni connection information, listen port |
| Agent service startup | Barman server `barman-agent@<database ID>` | Started with `sudo systemctl restart` |
| SSH connection settings | Database server `~/.ssh/config` | Specify to use the SSH key entered during node registration when connecting to the Barman server |
| Connection allow rules | Database server `patroni.yml` | Add a connection allow rule for the Barman server IP |
| Replication slot | Database | Automatically created when the WAL retention method is `streaming` or `archiver+streaming` |
| Leader change integration | Database server `patroni.yml` | Configure to refresh the Barman settings when the leader changes |

{% hint style="info" %}
Note

If there is a problem with the OpenBackup server installation or operation, please refer to [Reference Materials > OpenBackup Remediation Guide](../../../references/openbackup-troubleshooting.md).
{% endhint %}
