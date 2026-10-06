{% hint style="info" %}
**Note**

- This guide is intended for the customer's infrastructure administrators.
- To register and manage an existing Tibero in production with OwlDB **Tibero 7 or later**must be met
{% endhint %}

---

This page explains the system, network, and database settings that must be prepared before registering an existing Tibero in production with OwlDB, as well as how to place the deployment files.

# System Requirements

### Disk (volume) requirements

Since the existing database is already configured, no separate preparation is required for the Data/Archive/Redo disks. Only the Backup disk needs to be checked.

The Backup disk must have an xfs filesystem created and be mounted.

{% hint style="info" %}
**Note**

For a TAC configuration, the Backup disk must be a shared volume accessible from all nodes.
{% endhint %}

# Network Requirements

### **Required port configuration for the database server**

The following ports are required to register an existing database with OwlDB.

<table><thead><tr><th>Port Type</th><th>Port Number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener Port</strong></td><td>Existing DB configuration value</td><td><ul><li>Database connection port</li><li>Tibero: e.g., 8629/tcp</li></ul></td><td><ul><li>Allow inbound DB Listener port from the OwlDB server</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Do not change the ports currently used by the existing database. Additionally open port 40001 for Agent communication and the Listener port for DB connection.
{% endhint %}

### Firewall Configuration

Firewall configuration is required for communication between the OwlDB server and the database server.

---

# Database configuration requirements

### Required settings for TAC and DR configurations

For databases operating in a TAC (Tibero Active Cluster) or DR configuration, please set the following parameters in advance.

**DB parameter**

| DB parameter | Value | Description |
| --- | --- | --- |
| LOG_ARCHIVE_FORMAT | - | Set to the same value on all nodes |
| STANDBY_USE_OBSERVER | Y | Required for DR configuration |

**CM parameter**

| CM parameter | Value | Description |
| --- | --- | --- |
| _CM_REDIRECT_STDOUT_TO_OUTFILE | Y | FS07PS_341175b patch required |

---

# Preparing and Placing Deployment Files

### 1. List of Required Files

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- license file (`license.xml`)

{% hint style="info" %}
**Note**

Since Tibero is already installed on the DB being registered, the Tibero binary is not required.
{% endhint %}

### 2. File placement

Move to the path where the existing database is installed (hereinafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

Of the existing database `TB_HOME`Extract the DP binary at the same level as.

```bash
# Extract the DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place the license file
mv {license file} $TB_HOME/license.xml
```

Once preparation is complete, `$TB_HOME` the structure is as follows.

```bash
$TB_HOME/
 ├── license.xml                      # License file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # Infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

Afterwards [Database server Agent installation document](agent-installation.md)and proceed.
