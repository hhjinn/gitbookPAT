OwlDB performs backup and recovery of the OpenSQL database using Barman (Backup and Recovery Manager), the OpenBackup. The OpenBackup server is the server that stores backup data and WAL (Write-Ahead Log), and it is installed on a separate server from OwlDB.

# System requirements <a href="#system-requirements" id="system-requirements"></a>

### 1. Hardware requirements <a href="#id-1" id="id-1"></a>

| Item | Requirements |
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

When multiple databases are integrated into a single OpenBackup server, sum the capacity of each database.

### 2. Software requirements <a href="#id-2" id="id-2"></a>

| Item | Requirements |
| --- | --- |
| OS | Rocky Linux 9.5 or later |
| OpenBackup | `3.11.1` (included in the OpenSQL distribution) |

{% hint style="info" %}
**Note**

OpenBackup is **installed with the OpenSQL distribution**, and its version must be the same as the `barman-cli` of the database server. For details, refer to [Install OpenBackup](openbackup-installation.md).
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
| `sudo` | OwlDB controls the Barman Agent with `sudo systemctl`. If it is not installed, the `/etc/sudoers.d` directory will not exist and the installation process will fail. |
| `cronie` | Runs the Barman cron. If it is not installed, WAL will not be collected. |
| `rsync` | Transfers data files during recovery |
| `openssh-server` | Receives WAL archives from the database server |
| `python3-psycopg2` | OpenBackup's PostgreSQL connection |
| `tar`, `gzip` | Decompress the distribution package |
| `which`, `procps-ng` | Verify installation status |
| `glibc-langpack-en` or lower | Runtime libraries required to run PostgreSQL and OpenBackup |

{% hint style="info" %}
**Note**

Servers where it is already installed are skipped without any changes.
{% endhint %}

### 4. Distribution files <a href="#id-4" id="id-4"></a>

| Item | Description | Example |
| --- | --- | --- |
| OpenSQL distribution | OpenBackup, the PostgreSQL client, and the Barman Agent are all included | `Tmax_OpenSQL_*.tar.gz` |
| OwlDB Agent | Agent that communicates with the OwlDB server | `owlagent_dist_latest.tar.gz` |
| SSH key | Key used for bidirectional connections. The Barman server requires both a **public key** (to allow connections from the database server) and a **private key** (to connect to the database server) | - |

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

Configure the firewall to allow the following communications.

| Direction | Port | Purpose |
| --- | --- | --- |
| OpenBackup server → OwlDB server | OwlDB server port | The Agent receives commands from OwlDB |
| OwlDB server → OpenBackup server | OpenBackup Agent Port (default `8080`) | Barman Agent control |
| OpenBackup server → database server | `5432` | Performs backups and receives WAL |
| OpenBackup server → database server | `22` | Transfers data files during recovery (rsync) |
| OpenBackup server → database server | `8008` | Queries Patroni cluster status |
| Database server → OpenBackup server | `22` | Transfers WAL archives |

- The OpenBackup Agent Port is the value entered on the OwlDB screen during integration, and can be specified in the range `1024`–`65535`. It has the same default as the OwlDB server port, but they are ports on different servers.
- Port `22` must be open in **both directions**. From the OpenBackup server to the database server, data files are transferred during recovery, and in the reverse direction, WAL archives are transferred. If only one direction is open, the backup or recovery will fail.
- Connection setup must be **performed manually in both directions**. The only part OwlDB handles automatically is specifying *which key* the database server uses to connect to the OpenBackup server; registering the OpenBackup server to *allow that key* must be done manually.

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
