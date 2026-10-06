[Preparing the OpenBackup Server Environment](openbackup-prerequisites.md)Proceed after the system and network requirements have been met.
Once the following procedure is completed, the OpenBackup server can be integrated with OwlDB.

{% hint style="info" %}
**Note**

The per-database configuration files and Agent processes are created automatically by OwlDB at the time of integration, so they do not need to be written manually.
{% endhint %}

# Install OpenBackup <a href="#install-openbackup" id="install-openbackup"></a>

## 1. Installing OpenBackup <a href="#install-openbackup-package" id="install-openbackup-package"></a>

OpenBackup is `3.1.1` Install with the version. The one installed on the database server `barman-cli` is also the same `3.11.1` must be.

| Installation location | Package | Provided command | Role |
| --- | --- | --- | --- |
| OpenBackup server | `barman` | `barman put-wal`, `barman get-wal` | Receive and retain WAL |
| Database server | `barman-cli` | `barman-wal-archive`, `barman-wal-restore` | Transfer WAL |

{% hint style="warning" %}
**Caution**

`pip install barman` If you install without specifying the version, the latest version is installed, which `3.11.1` does not match.

In this case, the versions of the WAL sender and receiver do not match, causing integration and backup to fail.
{% endhint %}

### 1-1. Distribution Decompression <a href="#id-1-1" id="id-1-1"></a>

Decompress the OpenSQL distribution to the desired path.

```bash
mkdir -p /opt/opensql-src
tar xzf <OpenSQL distribution>.tar.gz -C /opt/opensql-src --strip-components=1
```

### 1-2. Setting the Installation Path Environment Variables <a href="#id-1-2" id="id-1-2"></a>

Since the installation script requires the following environment variables, **set them first before installation**.

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

If it is not set, the following error is displayed in the next step and the installation does not proceed.

```bash
Error: Environment variable 'OPENSQL_HOME' is not set.
```

`PG_HOME` The value specified in must match the `path_prefix`(`<PG_HOME>/bin`) in step 3.

### 1-3. Proceeding with Installation <a href="#id-1-3" id="id-1-3"></a>

```bash
cd /opt/opensql-src/scripts
. ./setenv.sh "$(pwd)"
./install.sh postgresql
./install.sh barman
```

The OpenBackup server does not start the PostgreSQL service. `pg_receivewal`, `pg_basebackup` Only client utilities such as are used.

### 1-4. Version Verification <a href="#id-1-4" id="id-1-4"></a>

`3.11.1` should be printed.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

## 2. Configuring the OpenBackup Execution Account <a href="#openbackup-account" id="openbackup-account"></a>

### 2-1. Creating the Account <a href="#id-2-1" id="id-2-1"></a>

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
| Home Directory | The account's home directory. Backup data is stored under this directory. | `/var/lib/barman` | Required |
| sudo Privileges | NOPASSWD sudo | `barman ALL=(ALL) NOPASSWD: ALL` | Required |

OwlDB controls the Barman Agent via `sudo systemctl restart barman-agent@<database ID>` . Since this command must run without entering a password, NOPASSWD sudo privileges are required.

The OpenBackup server operates with this single account. Performing backups, running the OwlDB Agent, and connecting to the database server are all done with the same account.

### 2-2. Configuring PATH

`barman` In the account, `pg_receivewal`, `pg_basebackup` add to PATH so that PostgreSQL client commands such as these can be run without specifying the full path. Write it according to the `PG_HOME` value from 1-2.

```bash
su - barman
echo 'export PATH=$PATH:/opt/postgresql/bin' >> ~/.bashrc
source ~/.bashrc
exit
```

This setting is used when the operator runs commands directly. Since Barman cron does not use this PATH, you must separately configure the `path_prefix` in step 3.

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
| path_prefix | Path to the PostgreSQL client executables | `/opt/postgresql/bin` | Required |
| barman_user | Barman execution account | `barman` | Required |
| configuration_files_directory | Directory for per-database configuration files. Set it to the same value as the OwlDB Agent's `BARMAN_CONF_DIR` . | `/etc/barman.d` | Required |
| barman_home | Backup data storage path | `/var/lib/barman` | Required |
| log_file | Log file path | `/var/log/barman/barman.log` | Required |
| log_level | Log level | `INFO` | Required |

`path_prefix` must be set. Since Barman cron runs in a minimal PATH environment, the PATH configured in the account's `.bashrc` and the like is not applied. `path_prefix` Without `pg_receivewal` , cron cannot find it, and WAL collection fails permanently.

Create the directories used in the configuration and assign ownership.

```bash
mkdir -p /var/log/barman /etc/barman.d /var/lib/barman/agent

chown -R barman:barman /var/lib/barman /var/log/barman /etc/barman.d
chmod 700 /var/lib/barman
```

| directory | Purpose |
| --- | --- |
| `/etc/barman.d` | Per-database OpenBackup configuration file. Generated by OwlDB during integration. |
| `/var/lib/barman/agent` | Per-database Agent configuration file. Generated by OwlDB during integration. |
| `/var/log/barman` | Barman log |

Backup data is stored in `<barman_home>/<database ID>` . However, the Agent configuration file path in step 4 is fixed regardless of `barman_home` .

## 4. Registering the Barman Agent <a href="#register-barman-agent" id="register-barman-agent"></a>

The Barman Agent is a process that detects leader changes in the Patroni cluster and updates the Barman configuration. It uses the `barman_agent/server.py` from the OpenSQL distribution.

### 4-1. Placing the Executable <a href="#id-4-1" id="id-4-1"></a>

```bash
cp /opt/opensql-src/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r /opt/opensql-src/barman_agent/requirements.txt
```

### 4-2. Registering the systemd Template Unit

Since a single OpenBackup server can handle multiple databases, register it as a systemd **template unit**so that an Agent is started per database. The OwlDB database ID is passed as the instance name (`%i`).

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

Since a new unit file was created, apply it to systemd.

```bash
systemctl daemon-reload
```

The two values below are fixed in OwlDB, so use them as-is.

| Item | Value | Whether it can be changed |
| --- | --- | --- |
| Unit name | `barman-agent@.service` | Not allowed |
| Agent configuration file path | `/var/lib/barman/agent/<database ID>.config.yml` | Not allowed |

`barman_home` Even if `/var/lib/barman` is set to a path other than `/var/lib/barman/agent/` , the Agent configuration file path is fixed to `ExecStart` . If the path is written `barman_home` based, the configuration file generated by OwlDB cannot be read and the Agent will not start.

{% hint style="info" %}
**Note**

The contents of the configuration file are generated by OwlDB at the time of integration. In this step, only create the unit file; do not start it.
{% endhint %}

## 5. Registering Barman cron <a href="#register-barman-cron" id="register-barman-cron"></a>

Register cron so that WAL collection and archiving are performed at one-minute intervals.

`/etc/cron.d/barman-cron` Write the file as shown below.

```bash
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now sshd crond
```

`sshd` is also started. When the database server sends WAL archives, it connects to the OpenBackup server's `sshd` . On a server that is already running, it is left as is.

## 6. Installing the OwlDB Agent <a href="#install-owldb-agent" id="install-owldb-agent"></a>

The OwlDB Agent receives commands from the OwlDB server and performs tasks on the OpenBackup server. Since all integration tasks on the OpenBackup server are done through this Agent, be sure to install it.

### 6-1. Extracting the Distribution File <a href="#id-6-1" id="id-6-1"></a>

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

### 6-2. Configuring owlagent.env

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
| AGENT_TYPE | Agent type. For a Barman server, enter `barman` . | `barman` | Required |
| IP | OwlDB server IP | `192.168.0.10` | Required |
| PORT | OwlDB server port | `8080` | Required |
| USERNAME | Agent execution account | `barman` | Required |
| OPENSQL_HOME | PostgreSQL and OpenBackup installation path | `/opt/postgresql` | Required |
| BARMAN_NAME | OpenBackup server name displayed in OwlDB | `barman01` | Required |
| BARMAN_SSH_USER | OpenBackup server SSH connection account | `barman` | Required |
| BARMAN_SSH_KEYPATH | Path to the private key used when the OpenBackup server connects to the database server. In this step, only specify the path; the actual key file is placed in 7-2. | `/var/lib/barman/.ssh/id_rsa` | Required |
| BARMAN_SSH_IP | OpenBackup server IP | `192.168.0.20` | Required |
| BARMAN_SSH_PORT | OpenBackup server SSH port | `22` | Required |
| BARMAN_CONF_DIR | Per-database configuration file directory. `barman.conf` of `configuration_files_directory` Enter a value such as | `/etc/barman.d` | Required |

{% hint style="warning" %}
**Caution**

`AGENT_TYPE` If `barman` is not set to this, OwlDB will not recognize it as an OpenBackup server, so **Backup server** it will not appear in the list.
{% endhint %}

## 7. SSH Configuration <a href="#ssh-settings" id="ssh-settings"></a>

The OpenBackup server and the database server require **bidirectional**SSH connectivity. The preparations differ depending on the direction.

| Direction | Purpose | Preparation |
| --- | --- | --- |
| Database server → OpenBackup server | WAL archive transfer (`barman-wal-archive`) | OpenBackup server allows connections using that key (7-1) |
| OpenBackup server → database server | Data file transfer during recovery, `rsync` Backup method | Place authentication information on the OpenBackup server (7-2) |

Whether the database server **which key** connects to the OpenBackup server is configured automatically by OwlDB during integration. In the database server's `~/.ssh/config` it records that the SSH key entered when registering the node should be used. However, **OpenBackup** **making the server allow that key is not automatic, so** you must register it directly in 7-1.

### 7-1. Allowing connections from the database server to the OpenBackup server

Register the public key of the key used when registering the database node into the OpenBackup execution account's `authorized_keys` .

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <public key of the database node registration key> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

Without this registration, when the database server transfers WAL, it `Permission denied (publickey)` fails with. Then, during integration, `barman check` of **WAL archive**and **continuous archiving** the item fails, causing the integration to roll back, and **Health**is **Not connected** displayed as.

### 7-2. Configuring connections from the OpenBackup server to the database server

From the OpenBackup execution account to the OpenSQL account on the database server, **without entering a password** configure it so the connection is established.

Place the private key file in a location that the OpenBackup execution account can read.

```bash
cp <private key file> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

**You are free to choose the file name and path.** The path where you placed it should be entered into 6-2's `BARMAN_SSH_KEYPATH` exactly.

```bash
# Example of using a different name
cp <key file> /var/lib/barman/.ssh/<key name>
chmod 600 /var/lib/barman/.ssh/<key name>
chown barman:barman /var/lib/barman/.ssh/<key name>

# Enter the same path in owlagent.env
# BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/<key name>
```

The key file's **format and extension have no restrictions.** Whether OpenSSH format (`-----BEGIN OPENSSH PRIVATE KEY-----`) or PEM format, and whether the name is `id_rsa` or `.pem` either can be used.

The database server must allow connections using that key.

However, **the private key file itself must exist.** During integration, OwlDB generates a connection command in the Barman configuration file in the form of `ssh -i <BARMAN_SSH_KEYPATH>` and this value must be an **absolute path**. Therefore, methods that do not use a key file (ssh-agent, password authentication, etc.) cannot be used.

### 7-3. Registering the host key <a href="#id-7-3" id="id-7-3"></a>

Connect to the database server for the first time using the OpenBackup execution account to register the host key. If there are multiple database servers, **for all nodes** perform this.

```bash
su - barman
ssh -o StrictHostKeyChecking=accept-new <OpenSQL account>@<database server IP> hostname
```

If the database server's hostname is printed, it is working correctly.

This registration is needed for the verification command in 7-4 and when an operator connects directly. When OpenBackup connects for backup/recovery operations, it is unaffected because the connection command in the configuration file generated by OwlDB includes an option to skip host key checking.

### 7-4. Verifying the connection <a href="#id-7-4" id="id-7-4"></a>

The connection from the OpenBackup server to the database server must be established without a password or confirmation prompt. If there are multiple database servers, perform this for all nodes.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

| Output | Meaning |
| --- | --- |
| The database server's hostname | Normal |
| `Host key verification failed` | The host key registration in 7-3 has not been done |
| `Permission denied (publickey)` | The database server does not allow the key placed in 7-2 |

The opposite direction (database server → OpenBackup server) **This direction is handled automatically by OwlDB during integration, so prior verification is not necessary.** At the time of integration, OwlDB writes the connection configuration into the database server's `~/.ssh/config` and this configuration includes an option to automatically accept the host key of the OpenBackup server being connected to for the first time. Therefore, if you attempt a direct connection in this direction before integration, the host key is not registered, so `Host key verification failed` is printed, and this does not mean the installation is faulty.

What you must prepare directly in this direction is 7-1's `authorized_keys` registration only. If this registration is omitted, at the time of integration `barman check` of **WAL archive** and **continuous archiving** it manifests as an item failure.

---

# Verifying OpenBackup Server Integration <a href="#check-openbackup-connection" id="check-openbackup-connection"></a>

Proceed with the OpenBackup server installation already completed.

## 1. Database Server Configuration <a href="#configure-database-server" id="configure-database-server"></a>

In addition to the OpenBackup server, the following items must also be prepared on the database server.

| Item | Description | Required or Not |
| --- | --- | --- |
| `barman-cli` | Same as the OpenBackup server `3.11.1` Install with this version | WAL retention method `archiver` or `archiver+streaming` Required when used as |
| `rsync` | Used for transferring data files during recovery | Required when using recovery |
| Database account | The account entered during OpenSQL installation must exist as a superuser | Required |

OpenBackup connects to the database using the account entered during OpenSQL installation. If this account is a superuser, it already holds replication and backup privileges, so there is no need to create a separate dedicated account.

### 1-1. Verifying barman-cli Installation

```bash
rpm -q barman-cli
barman-wal-archive --version
```

If it is installed, the output appears as follows.

```bash
$ barman-wal-archive --version
barman-wal-archive 3.11.1
```

If it is not installed, the output appears as follows.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

`barman-cli` is a separate package from OpenSQL, so it may not be present even on servers where OpenSQL is installed. Be sure to verify this before integration.

WAL retention method `archiver` or `archiver+streaming` When integrating with `barman-wal-archive` is missing on the database server, OwlDB rejects the integration. When the WAL retention method is `streaming` in the case of `barman-cli` is not required.

### 1-2. Installing barman-cli

`barman-cli` is provided by the official PostgreSQL repository (PGDG).

In environments with internet connectivity, register the repository and then specify the version to install.

```bash
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

dnf install -y rsync
dnf --enablerepo=pgdg-common install -y barman-cli-3.11.1
```

In environments without internet connectivity, obtain the rpm file of the same version in advance and install it.

```bash
dnf install -y ./barman-cli-3.11.1-*.rpm
```

{% hint style="warning" %}
**Caution**

Without specifying the version `dnf install barman-cli` or `pip install barman-cli` if you install with this, the latest version is installed, causing a version mismatch with the Barman server. Be sure to `barman-cli-3.11.1` specify the version as shown here.
{% endhint %}

## 2. Verifying OpenBackup Server Installation Results <a href="#check-openbackup-installation" id="check-openbackup-installation"></a>

On the OpenBackup server, check using the command below.

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

| Check item | Normal result |
| --- | --- |
| `barman --version` | `3.11.1` |
| `/etc/barman.conf` | `path_prefix`, `configuration_files_directory` Configured |
| `barman-agent@` Unit | Unit exists, `ExecStart` is `/var/lib/barman/agent/%i.config.yml` Reference |
| `/etc/cron.d/barman-cron` | 1-minute interval `barman cron` Registered |
| `owldb-barman-agent.service` / `.timer` | All `active` |
| `sshd`, `crond` | All `active` |
| NOPASSWD sudo | `OK` Output |

{% hint style="warning" %}
**Caution**

Do not skip the NOPASSWD sudo check. Without this setting, the installation appears complete, but during integration it fails at the stage where OwlDB starts the Barman Agent.
{% endhint %}

## 3. OwlDB Integration <a href="#connect-owldb" id="connect-owldb"></a>

In the OwlDB web UI, perform the integration in the following order.

1. of the database **OpenBackup Settings** Navigate to the screen.
2. **Backup server** Verify that the installed Barman server appears in the list. If it appears, it is ready for integration.
3. **Backup server**, **OpenBackup Agent Port**, **WAL retention method**, **Backup method** Select it to integrate.
4. After integration **Health** is **Connected** Verify that it is displayed as this.

**Health** is **Connected** If it is this, installation and integration are complete.

---

### Note: Tasks OwlDB Performs Automatically During Integration <a href="#owldb" id="owldb"></a>

The following items are automatically created or configured by OwlDB at the time of integration, so do not write them manually.

| Target | Location | Content |
| --- | --- | --- |
| Barman configuration file | Barman server `/etc/barman.d/<database ID>.conf` | Database connection information, WAL retention method, backup method, `backup_directory` |
| Agent configuration file | Barman server `/var/lib/barman/agent/<database ID>.config.yml` | Cluster name, Patroni connection information, listen port |
| Agent service startup | Barman server `barman-agent@<database ID>` | `sudo systemctl restart` Started with |
| SSH connection settings | Database Server `~/.ssh/config` | Specifies the use of the SSH key entered during node registration when connecting to the Barman server |
| Connection permission rule | Database Server `patroni.yml` | Adds a connection permission rule for the Barman server IP |
| Replication slot | database | When the WAL retention method is `streaming` or `archiver+streaming` Automatically created in the case of |
| Leader change integration | Database Server `patroni.yml` | Configures Barman settings to be refreshed when the leader changes |

{% hint style="info" %}
Note

If there is a problem with the OpenBackup server installation or operation, [Reference Materials > OpenBackup Action Guide](../../../references/openbackup-troubleshooting.md)Please refer to it.
{% endhint %}
