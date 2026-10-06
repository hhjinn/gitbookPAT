# OpenBackup Server Installation Guide

[Preparing the OpenBackup server environment](openbackup.md)Proceed after meeting the system and network requirements. Completing the procedure below brings OwlDB to a state where it can integrate with the OpenBackup server.

{% hint style="info" %}
**Note**

The per-database configuration files and Agent processes are automatically created by OwlDB at integration time, so you do not write them manually.
{% endhint %}

## OpenBackup installation

### 1. Install OpenBackup

OpenBackup is `3.1.1` Install this version. The version installed on the database server `barman-cli` must also be identical `3.11.1` .

| Installation location | Package      | Provided command                           | Role                  |
| --------------------- | ------------ | ------------------------------------------ | --------------------- |
| OpenBackup server     | `barman`     | `barman put-wal`, `barman get-wal`         | Receive and store WAL |
| Database server       | `barman-cli` | `barman-wal-archive`, `barman-wal-restore` | Transfer WAL          |

{% hint style="warning" %}
**Caution**

`pip install barman` If you install without specifying a version like this, the latest version will be installed and `3.11.1` will not match.

In this case, the versions on the WAL-sending side and the WAL-receiving side do not match, so integration and backup will fail.
{% endhint %}

#### 1-1. Extract the distribution

Extract the OpenSQL distribution to the desired path.

```bash
mkdir -p /opt/opensql-src
tar xzf <OpenSQL distribution>.tar.gz -C /opt/opensql-src --strip-components=1
```

#### 1-2. Set the installation path environment variables

Since the installation script requires the following environment variables, **set them first before installation**.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

| Item          | Description                                                                 | Example                | Required or Not |
| ------------- | --------------------------------------------------------------------------- | ---------------------- | --------------- |
| OPENSQL\_HOME | OpenSQL installation path                                                   | `/opt/postgresql`      | Required        |
| PG\_HOME      | PostgreSQL installation path. The executables are `<PG_HOME>/bin` placed in | `/opt/postgresql`      | Required        |
| PG\_DATA\_DIR | PostgreSQL data path                                                        | `/opt/postgresql/data` | Required        |

If not set, the following error is output in the next step, and the installation does not proceed.

```bash
Error: Environment variable 'OPENSQL_HOME' is not set.
```

`PG_HOME` The value specified in must equal the `path_prefix`(`<PG_HOME>/bin`) in step 3.

#### 1-3. Proceed with installation

```bash
cd /opt/opensql-src/scripts
. ./setenv.sh "$(pwd)"
./install.sh postgresql
./install.sh barman
```

The OpenBackup server does not start the PostgreSQL service. `pg_receivewal`, `pg_basebackup` It uses only client utilities such as

#### 1-4. Verify the version

`3.11.1` should be output.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

### 2. Configure the OpenBackup execution account

#### 2-1. Create the account

Create a dedicated OS account for running OpenBackup and grant it NOPASSWD sudo privileges.

```bash
groupadd -g 1001 barman
useradd -u 1001 -g 1001 -m -d /var/lib/barman -s /bin/bash barman

echo "barman ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/barman
chmod 440 /etc/sudoers.d/barman
```

| Item            | Description                                                        | Example                          | Required or Not |
| --------------- | ------------------------------------------------------------------ | -------------------------------- | --------------- |
| Account         | Dedicated OS account for running OpenBackup                        | `barman`                         | Required        |
| Home directory  | Account home directory. Backup data is stored under this directory | `/var/lib/barman`                | Required        |
| sudo privileges | NOPASSWD sudo                                                      | `barman ALL=(ALL) NOPASSWD: ALL` | Required        |

OwlDB runs the Barman Agent `sudo systemctl restart barman-agent@<데이터베이스 ID>` controls it. Since this command must run without a password prompt, NOPASSWD sudo privileges are required.

The OpenBackup server runs under this single account. Backup execution, OwlDB Agent execution, and database server access are all performed under the same account.

#### 2-2. PATH Configuration

`barman` From the account, `pg_receivewal`, `pg_basebackup` add to PATH so that PostgreSQL client commands such as these can be run without a path. Write it according to the `PG_HOME` value from 1-2.

```bash
su - barman
echo 'export PATH=$PATH:/opt/postgresql/bin' >> ~/.bashrc
source ~/.bashrc
exit
```

This setting is used when the operator runs commands directly. Barman cron does not use this PATH, so you must separately configure the `path_prefix` in step 3.

### 3. OpenBackup Global Configuration

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

| Item                            | Description                                                                                                                  | Example                      | Required or Not |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | --------------- |
| path\_prefix                    | PostgreSQL client executable path                                                                                            | `/opt/postgresql/bin`        | Required        |
| barman\_user                    | Barman execution account                                                                                                     | `barman`                     | Required        |
| configuration\_files\_directory | Per-database configuration file directory. Set it to the same value as OwlDB Agent's `BARMAN_CONF_DIR` set to the same value | `/etc/barman.d`              | Required        |
| barman\_home                    | Backup data storage path                                                                                                     | `/var/lib/barman`            | Required        |
| log\_file                       | Log file path                                                                                                                | `/var/log/barman/barman.log` | Required        |
| log\_level                      | Log level                                                                                                                    | `INFO`                       | Required        |

`path_prefix` be sure to configure it. Since Barman cron runs in a minimal PATH environment, the PATH configured in the account's `.bashrc` and so on is not applied. `path_prefix` Without it, cron `pg_receivewal` cannot find it, causing WAL collection to fail permanently.

Create the directories used in the configuration and assign ownership.

```bash
mkdir -p /var/log/barman /etc/barman.d /var/lib/barman/agent

chown -R barman:barman /var/lib/barman /var/log/barman /etc/barman.d
chmod 700 /var/lib/barman
```

| directory               | Purpose                                                                         |
| ----------------------- | ------------------------------------------------------------------------------- |
| `/etc/barman.d`         | Per-database OpenBackup configuration file. Created by OwlDB during integration |
| `/var/lib/barman/agent` | Per-database Agent configuration file. Created by OwlDB during integration      |
| `/var/log/barman`       | Barman log                                                                      |

Backup data is stored in `<barman_home>/<데이터베이스 ID>` . However, the Agent configuration file path in step 4 is fixed `barman_home` regardless of it.

### 4. Barman Agent Registration

The Barman Agent is a process that detects leader changes in the Patroni cluster and updates the Barman configuration. It uses the OpenSQL distribution's `barman_agent/server.py` is used.

#### 4-1. Executable Placement

```bash
cp /opt/opensql-src/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r /opt/opensql-src/barman_agent/requirements.txt
```

#### 4-2. systemd Template Unit Registration

Since a single OpenBackup server can handle multiple databases, register it as a systemd **template unit**so that an Agent is started per database. The database ID of OwlDB is passed as the instance name (`%i`).

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

The two values below are fixed in OwlDB, so use them as is.

| R2Bejjj0hVbK                  | dqMQWeb0doX9                                   | sLmi17pasQvN |
| ----------------------------- | ---------------------------------------------- | ------------ |
| Item                          | Value                                          | Changeable   |
| Unit name                     | `barman-agent@.service`                        | Not allowed  |
| Agent configuration file path | `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | Not allowed  |

`barman_home` to `/var/lib/barman` Even when configured to a path other than `/var/lib/barman/agent/` , the Agent configuration file path is fixed. `ExecStart` the path `barman_home` If written based on it, OwlDB cannot read the configuration file it created, and the Agent will not start.

{% hint style="info" %}
**Note**

The contents of the configuration file are created by OwlDB at the time of integration. In this step, only write the unit file and do not start it.
{% endhint %}

### 5. Barman cron Registration

Register cron so that WAL collection and archiving are performed at one-minute intervals.

`/etc/cron.d/barman-cron` Write the file as shown below.

```bash
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now sshd crond
```

`sshd` is also started. When the database server transmits the WAL archive, it connects to the OpenBackup server's  `sshd` . On an already running server, it is kept as is.

### 6. OwlDB Agent Installation

The OwlDB Agent receives commands from the OwlDB server and performs tasks on the OpenBackup server. Since all integration tasks of the OpenBackup server are performed through this Agent, be sure to install it.

#### 6-1. Extracting the Distribution File

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

#### 6-2. owlagent.env Configuration

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

| Item                 | Description                                                                                                                                                  | Example                       | Required or Not |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------- | --------------- |
| AGENT\_TYPE          | Agent type. For the Barman server, enter `barman` enter it                                                                                                   | `barman`                      | Required        |
| IP                   | OwlDB server IP                                                                                                                                              | `192.168.0.10`                | Required        |
| PORT                 | OwlDB server port                                                                                                                                            | `8080`                        | Required        |
| USERNAME             | Agent execution account                                                                                                                                      | `barman`                      | Required        |
| OPENSQL\_HOME        | PostgreSQL and OpenBackup installation path                                                                                                                  | `/opt/postgresql`             | Required        |
| BARMAN\_NAME         | OpenBackup server name displayed in OwlDB                                                                                                                    | `barman01`                    | Required        |
| BARMAN\_SSH\_USER    | OpenBackup server SSH access account                                                                                                                         | `barman`                      | Required        |
| BARMAN\_SSH\_KEYPATH | Private key path used when the OpenBackup server connects to the database server. In this step, only specify the path; the actual key file is placed in 7-2. | `/var/lib/barman/.ssh/id_rsa` | Required        |
| BARMAN\_SSH\_IP      | OpenBackup server IP                                                                                                                                         | `192.168.0.20`                | Required        |
| BARMAN\_SSH\_PORT    | OpenBackup server SSH port                                                                                                                                   | `22`                          | Required        |
| BARMAN\_CONF\_DIR    | Per-database configuration file directory. `barman.conf` of `configuration_files_directory` Enter the same value as                                          | `/etc/barman.d`               | Required        |

{% hint style="warning" %}
**Caution**

`AGENT_TYPE` If `barman` is not it, **Backup server** OwlDB does not recognize it as an OpenBackup server, so it is not displayed in the list.
{% endhint %}

### 7. SSH Configuration

The OpenBackup server and the database server require **bidirectional**SSH access. What needs to be prepared differs by direction.

| Direction                           | Purpose                                                   | Preparation                                                       |
| ----------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------- |
| Database server → OpenBackup server | WAL archive transmission (`barman-wal-archive`)           | The OpenBackup server allows connection with that key (7-1)       |
| OpenBackup server → database server | Data file transfer during recovery, `rsync` Backup method | Placing authentication credentials on the OpenBackup server (7-2) |

Whether the database server **uses to** connects to the OpenBackup server is automatically configured by OwlDB during integration. The database server's `~/.ssh/config` is recorded to use the SSH key entered when registering the node in. However, **OpenBackup** **having the server accept that key is not automatic, so** you must register it directly in 7-1.

#### 7-1. Allowing access from the database server to OpenBackup\*\* \*\*access to the server

Add the public key of the key used when registering the database node to the OpenBackup execution account's `authorized_keys` .

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <public key of the database node registration key> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

Without this registration, when the database server transfers WAL, it `Permission denied (publickey)` fails with. Then, during integration, the `barman check` of **WAL archive**and **continuous archiving** item fails, the integration is rolled back, and **Health**is **Not connected** is displayed as.

#### 7-2. Configuring access from the OpenBackup server to the database server

From the OpenBackup execution account to the OpenSQL account on the database server, **without entering a password,** configure so that access is established.

Place the private key file in a location that the OpenBackup execution account can read.

```bash
cp <private key file> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

**You can freely choose the file name and path.** Just enter the placed path exactly in 6-2's `BARMAN_SSH_KEYPATH` .

```bash
# Example of using a different name
cp <key file> /var/lib/barman/.ssh/<key name>
chmod 600 /var/lib/barman/.ssh/<key name>
chown barman:barman /var/lib/barman/.ssh/<key name>

# Enter the same path in owlagent.env
# BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/<key name>
```

The key file's **format and extension have no restrictions.** Whether it is OpenSSH format (`-----BEGIN OPENSSH PRIVATE KEY-----`) or PEM format, and whether the name is `id_rsa` or `.pem` , it can be used.

The database server must be configured to allow access with that key.

However, **the private key file itself must exist.** During integration, OwlDB generates an access command in the Barman configuration file in the form of `ssh -i <BARMAN_SSH_KEYPATH>` , and this value **absolute path**must be an. Therefore, methods that do not use a key file (ssh-agent, password authentication, etc.) cannot be used.

#### 7-3. Registering the host key

Connect to the database server for the first time using the OpenBackup execution account to register the host key. If there are multiple database servers, **for all nodes** perform this.

```bash
su - barman
ssh -o StrictHostKeyChecking=accept-new <OpenSQL account>@<database server IP> hostname
```

It is normal if the hostname of the database server is output.

This registration is needed for the verification command in 7-4 and when an operator connects directly. When OpenBackup connects during backup/recovery operations, it is not affected because the access command in the configuration file generated by OwlDB includes an option to skip the host key check.

#### 7-4. Verifying access

Access from the OpenBackup server to the database server must be established without a password or confirmation prompt. If there are multiple database servers, perform this for all nodes.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL account>@<database server IP> hostname
```

| Output                          | Meaning                                                  |
| ------------------------------- | -------------------------------------------------------- |
| hostname of the database server | Normal                                                   |
| `Host key verification failed`  | The host key registration in 7-3 has not been done       |
| `Permission denied (publickey)` | The database server does not allow the key placed in 7-2 |

The reverse direction (database server → OpenBackup server) is **Since OwlDB handles this direction automatically during integration, prior verification is not required.** At the time of integration, OwlDB writes the access configuration to the database server's `~/.ssh/config` , and this configuration includes an option to automatically accept the host key of the OpenBackup server being connected to for the first time. Therefore, if you attempt direct access in this direction before integration, the host key is not registered, so `Host key verification failed` is output, and this does not mean the installation is incorrect.

The only thing you need to prepare directly in this direction is the `authorized_keys` registration in 7-1. If this registration is missing, it is revealed as a `barman check` of **WAL archive** and **continuous archiving** item failure at the time of integration.

***

## Verifying OpenBackup server integration

Proceed with the OpenBackup server installation completed.

### 1. Database server configuration

In addition to the OpenBackup server, the following items must also be prepared on the database server.

| Item             | Description                                                               | Required or Not                                                                        |
| ---------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `barman-cli`     | Install with the same `3.11.1` version as the OpenBackup server           | Set the WAL retention method `archiver` or `archiver+streaming` Required when using as |
| `rsync`          | Used for data file transfer during recovery                               | Required when using recovery                                                           |
| Database account | The account entered during OpenSQL installation must exist as a superuser | Required                                                                               |

OpenBackup connects to the database using the account entered during OpenSQL installation. If this account is a superuser, it already holds replication and backup privileges, so there is no need to create a separate dedicated account.

#### 1-1. Verifying barman-cli installation

```bash
rpm -q barman-cli
barman-wal-archive --version
```

If it is installed, the output appears as shown below.

```bash
$ barman-wal-archive --version
barman-wal-archive 3.11.1
```

If it is not installed, the output appears as shown below.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

`barman-cli` is a package separate from OpenSQL, so it may not be present even on a server where OpenSQL is installed. Be sure to verify this before integration.

Set the WAL retention method `archiver` or `archiver+streaming` When integrating with `barman-wal-archive` is missing on the database server, OwlDB refuses the integration. When the WAL retention method is `streaming` , then `barman-cli` is not required.

#### 1-2. Installing barman-cli

`barman-cli` is provided by the official PostgreSQL repository (PGDG).

In an environment with internet access, register the repository and then install by specifying the version.

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

If you install without specifying the version, using `dnf install barman-cli` or `pip install barman-cli` , the latest version is installed and its version will be mismatched with the Barman server. Be sure to specify the version `barman-cli-3.11.1` as shown.
{% endhint %}

### 2. Verifying the OpenBackup server installation result

Check using the command below on the OpenBackup server.

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

| Check item                              | Expected result                                                       |
| --------------------------------------- | --------------------------------------------------------------------- |
| `barman --version`                      | `3.11.1`                                                              |
| `/etc/barman.conf`                      | `path_prefix`, `configuration_files_directory` Configured             |
| `barman-agent@` Unit                    | Unit exists, `ExecStart` is `/var/lib/barman/agent/%i.config.yml` See |
| `/etc/cron.d/barman-cron`               | 1-minute interval `barman cron` Registered                            |
| `owldb-barman-agent.service` / `.timer` | All `active`                                                          |
| `sshd`, `crond`                         | All `active`                                                          |
| NOPASSWD sudo                           | `OK` Output                                                           |

{% hint style="warning" %}
**Caution**

Do not omit the NOPASSWD sudo check. Without this setting, the installation appears to be complete, but during integration it fails at the stage where OwlDB starts the Barman Agent.
{% endhint %}

### 3. OwlDB integration

Perform the integration in the OwlDB web UI in the following order.

1. the database's **OpenBackup settings** Navigate to the screen.
2. **Backup server** Check whether the installed Barman server appears in the list. If it appears, it is ready for integration.
3. **Backup server**, **OpenBackup Agent Port**, **WAL retention method**, **Backup method** Select it to integrate.
4. After integration, **Health** is **Connected** Check whether it is displayed as.

**Health** is **Connected** If it is, the installation and integration are complete.

***

#### Note: Tasks automatically performed by OwlDB during integration

The items below are automatically created or configured by OwlDB at the time of integration, so you do not need to write them yourself.

| Target                    | Location                                                     | Content                                                                                            |
| ------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| Barman configuration file | Barman server `/etc/barman.d/<데이터베이스 ID>.conf`               | Database connection information, WAL retention method, backup method, `backup_directory`           |
| Agent configuration file  | Barman server `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | Cluster name, Patroni connection information, listen port                                          |
| Agent service startup     | Barman server `barman-agent@<데이터베이스 ID>`                     | `sudo systemctl restart` Started with                                                              |
| SSH connection settings   | Database Server `~/.ssh/config`                              | Specifies to use the SSH key entered during node registration when connecting to the Barman server |
| Connection allow rule     | Database Server `patroni.yml`                                | Add a connection allow rule for the Barman server IP                                               |
| Replication slot          | Database                                                     | When the WAL retention method is `streaming` or `archiver+streaming` , automatically created       |
| Leader change integration | Database Server `patroni.yml`                                | Configures the Barman settings to be refreshed when the leader changes                             |

{% hint style="info" %}
Note

If there is a problem with the OpenBackup server installation or operation, [Reference Materials > OpenBackup Troubleshooting Guide](../../../undefined-8/openbackup.md)Please refer to.
{% endhint %}
