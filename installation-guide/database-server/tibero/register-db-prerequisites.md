{% hint style="info" %}
**Note**

- This guide is intended for the customer's infrastructure administrators.
- To register and manage an existing operating Tibero in OwlDB **Tibero 7 or later**it must be
{% endhint %}

---

This page explains the system, network, and database settings that must be prepared before registering an existing operating Tibero in OwlDB, and how to place the deployment files.

# System Requirements <a href="#system-requirements" id="system-requirements"></a>

### Disk (volume) requirements <a href="#undefined" id="undefined"></a>

Since the existing database is already configured, no separate preparation is required for the Data/Archive/Redo disks. Only the Backup disk needs to be checked.

The Backup disk must have the xfs filesystem created and mounted.

{% hint style="info" %}
**Note**

In the case of a TAC configuration, the Backup disk must be a shared volume accessible from all nodes.
{% endhint %}

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

### **Required port configuration for the database server** <a href="#undefined-1" id="undefined-1"></a>

The following ports are required to register an existing database in OwlDB.

<table><thead><tr><th>Port type</th><th>Port number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener port</strong></td><td>Existing DB setting value</td><td><ul><li>Database connection port</li><li>Tibero: e.g., 8629/tcp</li></ul></td><td><ul><li>Allow inbound DB Listener port from the OwlDB server</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Do not change the ports currently used by the existing database. Additionally open port 40001 for Agent communication and the Listener port for DB connection.
{% endhint %}

### Firewall settings <a href="#undefined-2" id="undefined-2"></a>

Firewall settings are required for communication between the OwlDB server and the database server.

---

# Database configuration requirements <a href="#database-requirements" id="database-requirements"></a>

### Required settings for TAC and DR configurations <a href="#tac-dr" id="tac-dr"></a>

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

# Preparing and placing deployment files <a href="#prepare-deployment-files" id="prepare-deployment-files"></a>

### 1. List of required files <a href="#id-1" id="id-1"></a>

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- license file (`license.xml`)

{% hint style="info" %}
**Note**

Since the registered DB already has Tibero installed, the Tibero binary is not required.
{% endhint %}

### 2. File Placement <a href="#id-2" id="id-2"></a>

The path where the existing database is installed (hereinafter `installation directory`) and move to it. (Example: `/home/rocky/owldb`)

of the existing database `TB_HOME`Decompress the DP binary at the same level as.

```bash
# Decompress DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place license file
mv {license file} $TB_HOME/license.xml
```

After preparation is complete `$TB_HOME` the structure is as follows.

```bash
$TB_HOME/
 ├── license.xml                      # license file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

Afterwards [Database server Agent installation document](agent-installation.md)go to and proceed.
