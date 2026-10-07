OwlDB performs backup and recovery of the OpenSQL database using Barman (Backup and Recovery Manager) as OpenBackup. The OpenBackup server is a server that stores backup data and WAL (Write-Ahead Log), and is installed on a separate server from OwlDB.

# System Requirements <a href="#system-requirements" id="system-requirements"></a>

### 1. Hardware Requirements <a href="#id-1" id="id-1"></a>

| Item | Requirement |
| --- | --- |
| Processor | Calculated according to the scale of the target database |
| Memory | Calculated according to the scale of the target database |
| Disk | Calculated by summing the capacity for storing backup data and WAL (see below) |

Disk capacity is calculated by summing the following items.

| Purpose | Calculation criteria |
| --- | --- |
| Backup data | Target database size × number of backups to retain |
| WAL | Total amount of WAL generated during the backup retention period |
| Logs and binaries | Barman logs and installation binaries |

When linking multiple databases to a single OpenBackup server, sum the capacity of each database.

### 2. Software Requirements <a href="#id-2" id="id-2"></a>

| Item | Requirement |
| --- | --- |
| OS | Rocky Linux 9.5 or later |
| OpenBackup | `3.11.1` (Included in the OpenSQL distribution) |

{% hint style="info" %}
**Note**

OpenBackup is **Installed with the OpenSQL distribution**and its version must match the database server's `barman-cli` . For details, refer to [OpenBackup Installation](openbackup-installation.md)respectively.
{% endhint %}

### 3. OS Packages

Install the following packages on the OpenBackup server.

```bash
dnf install -y systemd sudo cronie rsync openssh-server openssh-clients \
  which procps-ng tar gzip hostname iproute \
  python3 python3-pip python3-setuptools python3-psycopg2 python3-dateutil python3-six \
  glibc-langpack-en libicu ncurses-libs lz4-libs readline zlib libgcc libstdc++
```

| Package | Purpose |
| --- | --- |
| `sudo` | OwlDB controls `sudo systemctl` the Barman Agent with this. If not installed, `/etc/sudoers.d` the directory does not exist and installation fails |
| `cronie` | Barman cron execution. If not installed, WAL is not collected |
| `rsync` | Transfer data files during recovery |
| `openssh-server` | Receive WAL archive from the database server |
| `python3-psycopg2` | OpenBackup's PostgreSQL connection |
| `tar`, `gzip` | Decompress the distribution |
| `which`, `procps-ng` | Verify installation status |
| `glibc-langpack-en` and below | Runtime libraries required for running PostgreSQL and OpenBackup |

{% hint style="info" %}
**Note**

On servers where it is already installed, this step is skipped with no changes.
{% endhint %}

### 4. Distribution files <a href="#id-4" id="id-4"></a>

| Item | Description | Example |
| --- | --- | --- |
| OpenSQL distribution | Includes OpenBackup, the PostgreSQL client, and the Barman Agent | `Tmax_OpenSQL_*.tar.gz` |
| OwlDB Agent | The Agent that communicates with the OwlDB server | `owlagent_dist_latest.tar.gz` |
| SSH key | The key used for bidirectional connections. The Barman server requires both a **public key**(permits connections from the database server) and a **private key**(connects to the database server) | - |

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

Configure the firewall to allow the communication below.

| Direction | Port | Purpose |
| --- | --- | --- |
| OpenBackup server → OwlDB server | OwlDB server port | The Agent receives commands from OwlDB |
| OwlDB server → OpenBackup server | OpenBackup Agent Port (default `8080`) | Barman Agent control |
| OpenBackup server → database server | `5432` | Perform backups and receive WAL |
| OpenBackup server → database server | `22` | Transfer data files during recovery (rsync) |
| OpenBackup server → database server | `8008` | Query Patroni cluster status |
| Database server → OpenBackup server | `22` | Transfer WAL archives |

- The OpenBackup Agent Port is the value entered on the OwlDB screen during integration, and `1024`~`65535` can be specified within the range. It has the same default value as the OwlDB server port, but it is a port on a different server.
- `22` The port number **in both directions** must be open. From the OpenBackup server to the database server, data files are transferred during recovery, and in the opposite direction, WAL archives are transferred. If only one direction is open, the backup or recovery fails.
- The connection setup **must be performed manually in both directions**. The only part OwlDB handles automatically is specifying which key the database server *with which key* uses to connect to the OpenBackup server; registering the OpenBackup server so that it *permits that key*must be done manually.

{% hint style="info" %}
**Note**

It is displayed as follows on the OwlDB console screen.

| Documentation term | OwlDB console screen label |
| --- | --- |
| OpenBackup server | Backup server |
| Barman Agent port | OpenBackup Agent Port |
| Integration status | Health (Connected / Disconnected) |
| WAL retention method | **WAL retention method** (`archiver` / `streaming` / `archiver+streaming`) |
{% endhint %}
