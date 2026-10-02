OwlDB performs backup and recovery of the OpenSQL database using Barman (Backup and Recovery Manager) as OpenBackup. The OpenBackup server is a server that stores backup data and WAL (Write-Ahead Log), and is installed on a separate server from OwlDB.

# System Requirements

### 1. Hardware Requirements

| Item | Requirements |
| --- | --- |
| Processor | Calculated according to the scale of the target database |
| Memory | Calculated according to the scale of the target database |
| Disk | Calculated by summing the backup data and WAL storage capacity (see below) |

Disk capacity is calculated by summing the following items.

| Purpose | Calculation criteria |
| --- | --- |
| Backup data | Target database size × number of backups to retain |
| WAL | Total amount of WAL generated during the backup retention period |
| Logs and binaries | Barman logs and installation binaries |

When linking multiple databases to a single OpenBackup server, sum the capacity for each database.

### 2. Software Requirements

| Item | Requirements |
| --- | --- |
| OS | Rocky Linux 9.5 or later |
| OpenBackup | `3.11.1` (Included in the OpenSQL distribution) |

{% hint style="info" %}
**Note**

OpenBackup is **installed from the OpenSQL distribution**and its version must be the same as the database server's `barman-cli` For details, see [OpenBackup installation](openbackup-1.md)respectively.
{% endhint %}

### 3. OS packages

Install the following packages on the OpenBackup server.

```bash
dnf install -y systemd sudo cronie rsync openssh-server openssh-clients \
  which procps-ng tar gzip hostname iproute \
  python3 python3-pip python3-setuptools python3-psycopg2 python3-dateutil python3-six \
  glibc-langpack-en libicu ncurses-libs lz4-libs readline zlib libgcc libstdc++
```

| Package | Purpose |
| --- | --- |
| `sudo` | OwlDB `sudo systemctl` controls the Barman Agent with this. If it is not installed `/etc/sudoers.d` the directory does not exist and the installation process fails |
| `cronie` | Running Barman cron. If it is not installed, WAL will not be collected |
| `rsync` | Transferring data files during recovery |
| `openssh-server` | Receiving WAL archives from the database server |
| `python3-psycopg2` | OpenBackup's PostgreSQL connection |
| `tar`, `gzip` | Extracting the distribution |
| `which`, `procps-ng` | Checking the installation status |
| `glibc-langpack-en` or lower | Runtime libraries required for running PostgreSQL and OpenBackup |

{% hint style="info" %}
**Note**

On servers where it is already installed, proceed without any changes.
{% endhint %}

### 4. Distribution files

| Item | Description | Example |
| --- | --- | --- |
| OpenSQL distribution | OpenBackup, the PostgreSQL client, and the Barman Agent are all included | `Tmax_OpenSQL_*.tar.gz` |
| OwlDB Agent | Agent that communicates with the OwlDB server | `owlagent_dist_latest.tar.gz` |
| SSH key | Key used for bidirectional connection. The Barman server requires both the **public key**(allows connections from the database server) and the **private key**(connects to the database server) | - |

# Network Requirements

Configure the firewall to allow the following communication.

| Direction | Port | Purpose |
| --- | --- | --- |
| OpenBackup server → OwlDB server | OwlDB server port | Agent receives commands from OwlDB |
| OwlDB server → OpenBackup server | OpenBackup Agent Port (default `8080`) | Barman Agent control |
| OpenBackup server → database server | `5432` | Perform backup and receive WAL |
| OpenBackup server → database server | `22` | Transfer data files during recovery (rsync) |
| OpenBackup server → database server | `8008` | Query Patroni cluster status |
| Database server → OpenBackup server | `22` | Transfer WAL archives |

- The OpenBackup Agent Port is the value entered on the OwlDB screen during integration, and `1024`~`65535` can be specified within the range. It has the same default value as the OwlDB server port, but they are ports on different servers.
- `22` This port must be **open in both** directions. From the OpenBackup server to the database server, data files are transferred during recovery, and in the opposite direction, WAL archives are transferred. If only one direction is open, backup or recovery will fail.
- Connection settings are **performed directly in both directions**. The only part OwlDB handles automatically is specifying which key the database server *uses to* connects to the OpenBackup server; registering the OpenBackup server to *allow that key*must be done manually.

{% hint style="info" %}
**Note**

It is displayed on the OwlDB console screen as follows.

| Documentation term | OwlDB console screen label |
| --- | --- |
| OpenBackup server | Backup server |
| Barman Agent port | OpenBackup Agent Port |
| Integration status | Health (Connected / Disconnected) |
| WAL retention method | **WAL retention method** (`archiver` / `streaming` / `archiver+streaming`) |
{% endhint %}
