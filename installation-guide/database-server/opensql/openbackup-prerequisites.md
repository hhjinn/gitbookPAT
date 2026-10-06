OwlDB performs backup and recovery of the OpenSQL database using Barman (Backup and Recovery Manager), which is the OpenBackup. The OpenBackup server is a server that stores backup data and WAL (Write-Ahead Log), and is installed on a server separate from OwlDB.

# System Requirements <a href="#system-requirements" id="system-requirements"></a>

### 1. Hardware Requirements <a href="#id-1" id="id-1"></a>

| Item | Requirement |
| --- | --- |
| Processor | Calculated according to the target database size |
| Memory | Calculated according to the target database size |
| Disk | Calculated by summing the storage capacity for backup data and WAL (see below) |

The disk capacity is calculated by summing the following items.

| Purpose | Calculation criteria |
| --- | --- |
| Backup data | Target database size × number of backups to retain |
| WAL | Total WAL volume generated during the backup retention period |
| Logs and binaries | Barman logs and installation binaries |

When linking multiple databases to a single OpenBackup server, sum the capacity for each database.

### 2. Software Requirements <a href="#id-2" id="id-2"></a>

| Item | Requirement |
| --- | --- |
| OS | Rocky Linux 9.5 or higher |
| OpenBackup | `3.11.1` (Included in the OpenSQL distribution) |

{% hint style="info" %}
**Note**

OpenBackup is **Installed with the OpenSQL distribution**and the version must be identical to that of the database server's `barman-cli` . For details, see [Install OpenBackup](openbackup-installation.md).
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
| `sudo` | OwlDB `sudo systemctl` controls the Barman Agent with this. If not installed, `/etc/sudoers.d` the directory is missing and the installation process fails |
| `cronie` | Barman cron execution. If not installed, WAL is not collected |
| `rsync` | Transfers data files during recovery |
| `openssh-server` | Receives WAL archives from the database server |
| `python3-psycopg2` | OpenBackup's PostgreSQL connection |
| `tar`, `gzip` | Distribution Decompression |
| `which`, `procps-ng` | Checking Installation Status |
| `glibc-langpack-en` Below | Runtime libraries required to run PostgreSQL and OpenBackup |

{% hint style="info" %}
**Note**

Servers where it is already installed are skipped without changes.
{% endhint %}

### 4. Distribution Files <a href="#id-4" id="id-4"></a>

| Item | Description | Example |
| --- | --- | --- |
| OpenSQL Distribution | Includes OpenBackup, the PostgreSQL client, and the Barman Agent | `Tmax_OpenSQL_*.tar.gz` |
| OwlDB Agent | Agent that communicates with the OwlDB server | `owlagent_dist_latest.tar.gz` |
| SSH Key | Key used for bidirectional connections. The Barman server requires **public key**(allows connections from the database server) and **private key**(connects to the database server), both are required | - |

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

Configure the firewall to allow the following communications.

| Direction | Port | Purpose |
| --- | --- | --- |
| OpenBackup server → OwlDB server | OwlDB server port | Agent receives commands from OwlDB |
| OwlDB server → OpenBackup server | OpenBackup Agent Port (default `8080`) | Barman Agent control |
| OpenBackup server → database server | `5432` | Perform backups and receive WAL |
| OpenBackup server → database server | `22` | Transfer data files during recovery (rsync) |
| OpenBackup server → database server | `8008` | Query Patroni cluster status |
| Database server → OpenBackup server | `22` | WAL archive transfer |

- The OpenBackup Agent Port is the value entered on the OwlDB screen during integration, and `1024`~`65535` can be specified within the range. It has the same default value as the OwlDB server port, but they are ports on different servers.
- `22` port must be **both directions** open. From the OpenBackup server to the database server, data files are transferred during recovery, and in the opposite direction, WAL archives are transferred. If only one direction is open, the backup or recovery will fail.
- The connection setup is **performed directly in both directions**. What OwlDB handles automatically is only the part that specifies which key the database server uses to *which key* connect to the OpenBackup server, and registering the OpenBackup server to *allow that key*must be done manually.

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
