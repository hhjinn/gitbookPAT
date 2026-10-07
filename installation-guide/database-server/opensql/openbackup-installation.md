[Preparing the OpenBackup server environment](openbackup-prerequisites.md)Proceed after the system and network requirements have been met.
Once the procedure below is complete, the OpenBackup server will be in a state where it can be integrated with OwlDB.

{% hint style="info" %}
**Note**

The per-database configuration files and Agent processes are created automatically by OwlDB at the time of integration, so you do not write them manually.
{% endhint %}

# OpenBackup Installation <a href="#install-openbackup" id="install-openbackup"></a>

## 1. Installing OpenBackup <a href="#install-openbackup-package" id="install-openbackup-package"></a>

OpenBackup is `3.1.1` Install this version. The one installed on the database server `barman-cli` is likewise `3.11.1` .

| Installation location | Package | Provided command | Role |
| --- | --- | --- | --- |
| OpenBackup server | `barman` | `barman put-wal`, `barman get-wal` | Receives and stores WAL |
| On the database server | `barman-cli` | `barman-wal-archive`, `barman-wal-restore` | Transfers WAL |

{% hint style="warning" %}
**Caution**

`pip install barman` If you install without specifying the version as shown, the latest version is installed, and `3.11.1` it does not match.

In this case, the versions on the sending and receiving sides of the WAL do not match, so integration and backup fail.
{% endhint %}

### 1-1. Decompress the distribution <a href="#id-1-1" id="id-1-1"></a>

Decompress the OpenSQL distribution to the desired path.

```bash
mkdir -p /opt/opensql-src
tar xzf <OpenSQL distribution>.tar.gz -C /opt/opensql-src --strip-components=1
```

### 1-2. Set the installation path environment variables <a href="#id-1-2" id="id-1-2"></a>

Because the installation script requires the environment variables below, **set them before installation**.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

| Item | Description | Example | Required or Not |
| --- | --- | --- | --- |
| OPENSQL_HOME | OpenSQL installation path | `/opt/postgresql` | Required |
| PG_HOME | PostgreSQL installation path. The executables are `<PG_HOME>/bin` placed in | `/opt/postgresql` | Required |
| PG_DATA_DIR | PostgreSQL data path | `/opt/postgresql/data` | Required |

If you do not set them, the following error is printed in the next step and the installation does not proceed.

```bash
Error: Environment variable 'OPENSQL_HOME' is not set.
```

`PG_HOME` The value specified in must match the `path_prefix`(`<PG_HOME>/bin`) in step 3.

### 1-3. Proceed with the installation <a href="#id-1-3" id="id-1-3"></a>

```bash
cd /opt/opensql-src/scripts
. ./setenv.sh "$(pwd)"
./install.sh postgresql
./install.sh barman
```

The OpenBackup server does not start the PostgreSQL service. `pg_receivewal`, `pg_basebackup` It uses only client utilities such as

### 1-4. Version Check <a href="#id-1-4" id="id-1-4"></a>

`3.11.1` should be printed.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

## 2. Configure the OpenBackup Execution Account <a href="#openbackup-account" id="openbackup-account"></a>

### 2-1. Create the Account <a href="#id-2-1" id="id-2-1"></a>

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
| Home Directory | The account's home directory. Backup data is stored under this directory | `/var/lib/barman` | Required |
| sudo Privileges | NOPASSWD sudo | `barman ALL=(ALL) NOPASSWD: ALL` | Required |

OwlDB controls the Barman Agent `sudo systemctl restart barman-agent@<Database ID>` with. Since this command must run without entering a password, NOPASSWD sudo privileges are required.

The OpenBackup server operates under this single account. Performing backups, running the OwlDB Agent, and connecting to the database server are all done with the same account.

### 2-2. Configure PATH

`barman` From the account `pg_receivewal`, `pg_basebackup` and other PostgreSQL client commands can be run without specifying their path by adding to PATH. Write it to match the `PG_HOME` value from 1-2.

```bash
su - barman
echo 'export PATH=$PATH:/opt/postgresql/bin' >> ~/.bashrc
source ~/.bashrc
exit
```

This setting is used when an operator runs commands directly. Because Barman cron does not use this PATH, you must separately configure the `path_prefix` in step 3.

## 3. OpenBackup Global Configuration <a href="#openbackup-global-settings" id="openbackup-global-settings"></a>

`/etc/barman.conf` Write the file as shown below.

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
| configuration_files_directory | Per-database configuration file directory. Set it to the same value as the OwlDB Agent's `BARMAN_CONF_DIR` set it to the same value | `/etc/barman.d` | Required |
| barman_home | Backup data storage path | `/var/lib/barman` | Required |
| log_file | Log file path | `/var/log/barman/barman.log` | Required |
| log_level | Log level | `INFO` | Required |

`path_prefix` be sure to configure this. Because Barman cron runs in a minimal PATH environment, the PATH configured in the account's `.bashrc` and similar files is not applied. `path_prefix` Without it, cron `pg_receivewal` cannot find it, and WAL collection fails permanently.

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

Backup data is stored in `<barman_home>/<Database ID>` is stored. However, the Agent configuration file path in step 4 is `barman_home` fixed regardless of it.

## 4. Register the Barman Agent <a href="#register-barman-agent" id="register-barman-agent"></a>

The Barman Agent is a process that detects leader changes in the Patroni cluster and updates the Barman configuration. It uses the OpenSQL distribution's `barman_agent/server.py` is used.

### 4-1. Place the Executable <a href="#id-4-1" id="id-4-1"></a>

```bash
cp /opt/opensql-src/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r /opt/opensql-src/barman_agent/requirements.txt
```

### 4-2. Register the systemd Template Unit

Because a single OpenBackup server can handle multiple databases, register it as a systemd **template unit**so that an Agent is started per database. The OwlDB database ID is passed to the instance name (`%i`).

`/usr/lib/systemd/system/barman-agent@.service` Write the file as shown below.

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

Since the unit file was newly written, apply it to systemd.

```bash
systemctl daemon-reload
```

The two values below are fixed in OwlDB, so use them as-is.

| Item | Value | Whether it can be changed |
| --- | --- | --- |
| Unit name | `barman-agent@.service` | Not allowed |
| Agent configuration file path | `/var/lib/barman/agent/<Database ID>.config.yml` | Not allowed |

`barman_home` the `/var/lib/barman` Even if set to a path other than this, the Agent configuration file path is `/var/lib/barman/agent/` fixed to. `ExecStart` If you write the `barman_home` path based on, OwlDB cannot read the configuration file it created, and the Agent will not start.

{% hint style="info" %}
**Note**

The contents of the configuration file are created by OwlDB at the time of integration. In this step, only write the unit file and do not start it.
{% endhint %}

## 5. Register the Barman cron <a href="#register-barman-cron" id="register-barman-cron"></a>

Register the cron so that WAL collection and archiving run at 1-minute intervals.

`/etc/cron.d/barman-cron` Write the file as shown below.

```bash
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now sshd crond
```

`sshd` is started together. When the database server sends the WAL archive, it connects to the OpenBackup server's `sshd` with. A server that is already running remains as-is.

## 6. Install the OwlDB Agent <a href="#install-owldb-agent" id="install-owldb-agent"></a>

The OwlDB Agent receives commands from the OwlDB server and performs tasks on the OpenBackup server. Since all integration tasks on the OpenBackup server are carried out through this Agent, be sure to install it.

### 6-1. Extract the Distribution File <a href="#id-6-1" id="id-6-1"></a>

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

### 6-2. Configure owlagent.env

`/var/lib/barman/owlagent_dist/owlagent.env` Write the file as shown below.

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
| AGENT_TYPE | Agent type. For the Barman server, enter `barman` enter | `barman` | Required |
| IP | OwlDB server IP | `192.168.0.10` | Required |
| PORT | OwlDB server port | `8080` | Required |
| USERNAME | Agent execution account | `barman` | Required |
| OPENSQL_HOME | PostgreSQL and OpenBackup installation path | `/opt/postgresql` | Required |
| BARMAN_NAME | OpenBackup server name displayed in OwlDB | `barman01` | Required |
| BARMAN_SSH_USER | OpenBackup server SSH connection account | `barman` | Required |
| BARMAN_SSH_KEYPATH | Private key path used when the OpenBackup server connects to the database server. In this step, only specify the path; the actual key file is placed in 7-2 | `/var/lib/barman/.ssh/id_rsa` | Required |
| BARMAN_SSH_IP | OpenBackup server IP | `192.168.0.20` | Required |
| BARMAN_SSH_PORT | OpenBackup server SSH port | `22` | Required |
| BARMAN_CONF_DIR | Database-specific configuration file directory. `barman.conf` 's `configuration_files_directory` Enter a value such as | `/etc/barman.d` | Required |

{% hint style="warning" %}
**Caution**

`AGENT_TYPE` is `barman` If it is not, OwlDB will not recognize it as an OpenBackup server, so **Backup server** it will not appear in the list.
{% endhint %}

## 7. SSH Configuration <a href="#ssh-settings" id="ssh-settings"></a>

The OpenBackup server and the database server **bidirectional**require SSH access. The preparations differ by direction.

| Direction | Purpose | Preparation |
| --- | --- | --- |
| Database server → OpenBackup server | WAL archive transfer (`barman-wal-archive`) | OpenBackup server allows access with that key (7-1) |
| OpenBackup server → database server | Data file transfer during recovery, `rsync` Backup method | Place authentication information on the OpenBackup server (7-2) |

Whether the database server **with which key** connects to the OpenBackup server is configured automatically by OwlDB during integration. The database server's `~/.ssh/config` is recorded to use the SSH key entered when registering the node. However, **OpenBackup** **making the server allow that key is not automatic, so** you must register it directly in 7-1.

### 7-1. Allowing access from the database server to the OpenBackup server

The public key of the key used when registering the database node, into the OpenBackup execution account's `authorized_keys` .

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <public key of the database node registration key> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

If this registration is missing, when the database server transfers WAL, it `Permission denied (publickey)` fails with. Then, during integration, `barman check` 's **WAL archive**and **continuous archiving** the item fails, the integration is rolled back, and **Health**is **Not connected** displayed as.

### 7-2. Configuring access from the OpenBackup server to the database server

From the OpenBackup execution account to the database server's OpenSQL account, **without entering a password** configure it so that access is possible.

Place the private key file in a location readable by the OpenBackup execution account.

```bash
cp <private key file> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

**The file name and path can be chosen freely.** Just enter the placed path exactly into 6-2's `BARMAN_SSH_KEYPATH` .

```bash
# Example of using a different name
cp <key file> /var/lib/barman/.ssh/<key name>
chmod 600 /var/lib/barman/.ssh/<key name>
chown barman:barman /var/lib/barman/.ssh/<key name>

# Enter the same path in owlagent.env
# BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/<key name>
```

The key file's **format and extension are not restricted.** Whether OpenSSH format (`-----BEGIN OPENSSH PRIVATE KEY-----`) or PEM format, whether the name is `id_rsa` or `.pem` , it can be used.

The database server must allow access with that key.

However, **the private key file itself must be present.** During integration, OwlDB generates an access command in the Barman configuration file in the form of `ssh -i <BARMAN_SSH_KEYPATH>` , and this value must be **an absolute path**. Therefore, methods that do not use a key file (ssh-agent, password authentication, etc.) cannot be used.

### 7-3. Registering the host key <a href="#id-7-3" id="id-7-3"></a>

Connect to the database server for the first time with the OpenBackup execution account and register the host key. If there are multiple database servers, **for all nodes** perform this.

```bash
su - barman
ssh -o StrictHostKeyChecking=accept-new <OpenSQL account>@<database server IP> hostname
```

If the database server's hostname is printed, it is working correctly.

This registration is needed for the verification command in 7-4 and when an operator connects directly. When OpenBackup connects during backup/recovery operations, it is not affected because the access command in the configuration file generated by OwlDB includes an option to skip the host key check.

### 7-4. Verifying access <a href="#id-7-4" id="id-7-4"></a>

Access from the OpenBackup server to the database server must succeed without a password or confirmation prompt. If there are multiple database servers, perform this for all nodes.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

| Output | Meaning |
| --- | --- |
| The database server's hostname | Normal |
| `Host key verification failed` | The host key registration in 7-3 has not been done |
| `Permission denied (publickey)` | The database server does not allow the key placed in 7-2 |

The reverse direction (database server → OpenBackup server) is **This direction is handled automatically by OwlDB during integration, so advance verification is not needed.** At the time of integration, OwlDB writes the access configuration to the database server's `~/.ssh/config` , and this configuration includes an option to automatically accept the host key of the OpenBackup server being connected to for the first time. Therefore, if you attempt a direct connection in this direction before integration, the host key is not registered, so `Host key verification failed` is printed, and this does not mean the installation is wrong.

What you must prepare directly in this direction is 7-1's `authorized_keys` registration only. If this registration is omitted, it surfaces as an `barman check` 's **WAL archive** and **continuous archiving** item failure at the time of integration.

---

# Verifying OpenBackup Server Integration <a href="#check-openbackup-connection" id="check-openbackup-connection"></a>

Proceed with the OpenBackup server installation already completed.

## 1. Database Server Configuration <a href="#configure-database-server" id="configure-database-server"></a>

In addition to the OpenBackup server, the following items must also be prepared on the database server.

| Item | Description | Required or Not |
| --- | --- | --- |
| `barman-cli` | Same as the OpenBackup server `3.11.1` Install with this version | The WAL retention method `archiver` or `archiver+streaming` Required when used as |
| `rsync` | Used for transferring data files during recovery | Required when using recovery |
| Database account | The account entered during OpenSQL installation must exist as a superuser | Required |

OpenBackup connects to the database using the account entered during OpenSQL installation. If this account is a superuser, it already holds replication and backup privileges, so there is no need to create a separate dedicated account.

### 1-1. Verifying barman-cli Installation

```bash
rpm -q barman-cli
barman-wal-archive --version
```

If installed, the output appears as follows.

```bash
$ barman-wal-archive --version
barman-wal-archive 3.11.1
```

If not installed, the output appears as follows.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

`barman-cli` is a package separate from OpenSQL, so it may not be present even on a server where OpenSQL is installed. Be sure to verify it before integration.

The WAL retention method `archiver` or `archiver+streaming` When integrating with `barman-wal-archive` is missing on the database server, OwlDB refuses the integration. When the WAL retention method is `streaming` in this case, `barman-cli` is not required.

### 1-2. Installing barman-cli

`barman-cli` is provided by the official PostgreSQL repository (PGDG).

In an environment with internet connectivity, register the repository and then install by specifying the version.

```bash
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

dnf install -y rsync
dnf --enablerepo=pgdg-common install -y barman-cli-3.11.1
```

In an environment without internet connectivity, obtain the rpm file of the same version in advance and install it.

```bash
dnf install -y ./barman-cli-3.11.1-*.rpm
```

{% hint style="warning" %}
**Caution**

Without specifying the version, `dnf install barman-cli` or `pip install barman-cli` if installed with this, the latest version is installed and the version mismatches the Barman server. Be sure to `barman-cli-3.11.1` specify the version as shown.
{% endhint %}

## 2. Verifying OpenBackup Server Installation Results <a href="#check-openbackup-installation" id="check-openbackup-installation"></a>

Check on the OpenBackup server with the following commands.

```bash
# Barman version
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)

# Global settings
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

| Check item | Normal result |
| --- | --- |
| `barman --version` | `3.11.1` |
| `/etc/barman.conf` | `path_prefix`, `configuration_files_directory` Configured |
| `barman-agent@` unit | unit exists, `ExecStart` is `/var/lib/barman/agent/%i.config.yml` see reference |
| `/etc/cron.d/barman-cron` | 1-minute interval `barman cron` registered |
| `owldb-barman-agent.service` / `.timer` | all `active` |
| `sshd`, `crond` | all `active` |
| NOPASSWD sudo | `OK` Output |

{% hint style="warning" %}
**Caution**

Do not skip the NOPASSWD sudo check. Without this setting, the installation appears complete, but during integration it fails at the stage where OwlDB starts the Barman Agent.
{% endhint %}

## 3. OwlDB Integration <a href="#connect-owldb" id="connect-owldb"></a>

Integrate in the following order in the OwlDB web UI.

1. Of the database **OpenBackup Settings** Move to the screen.
2. **Backup server** Check whether the installed Barman server appears in the list. If it appears, it is ready for integration.
3. **Backup server**, **OpenBackup Agent Port**, **WAL retention method**, **Backup method** Select it to integrate.
4. After integration, **Health** is **Connected** Check whether it is displayed as such.

**Health** is **Connected** If so, the installation and integration are complete.

---

### Note: Tasks OwlDB performs automatically during integration <a href="#owldb" id="owldb"></a>

The following items are automatically created or configured by OwlDB at the time of integration, so do not write them yourself.

| Target | Location | Content |
| --- | --- | --- |
| Barman configuration file | Barman server `/etc/barman.d/<Database ID>.conf` | Database connection information, WAL retention method, backup method, `backup_directory` |
| Agent configuration file | Barman server `/var/lib/barman/agent/<Database ID>.config.yml` | Cluster name, Patroni connection information, listen port |
| Agent service startup | Barman server `barman-agent@<Database ID>` | `sudo systemctl restart` Started with |
| SSH connection settings | Database Server `~/.ssh/config` | Specifies to use the SSH key entered during node registration when connecting to the Barman server |
| Connection permission rule | Database Server `patroni.yml` | Adds a connection permission rule for the Barman server IP |
| Replication slot | Database | The WAL retention method `streaming` or `archiver+streaming` Automatically created when it is |
| Leader change integration | Database Server `patroni.yml` | Configures the Barman settings to be updated when the leader changes |

{% hint style="info" %}
Note

If there is a problem with the OpenBackup server installation and operation, [Reference Material > OpenBackup Action Guide](../../../references/openbackup-troubleshooting.md)Please refer to it.
{% endhint %}
