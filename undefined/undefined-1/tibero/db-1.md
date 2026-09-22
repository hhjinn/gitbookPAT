{% hint style="info" %}
**Note**

- This guide is intended for customer infrastructure administrators.
- To register and manage an existing operational Tibero in OwlDB, **Tibero 7 or later**is required
{% endhint %}

---

This page describes the system, network, and database settings you must prepare, and how to place the deployment files, before registering an existing operational Tibero in OwlDB.

## System Requirements

### Disk (Volume) Requirements

Since the existing database is already configured, no separate preparation is required for the Data/Archive/Redo disks. Only the Backup disk needs to be checked.

The Backup disk must have an xfs filesystem created and be mounted.

{% hint style="info" %}
**Note**

For a TAC configuration, the Backup disk must be a shared volume accessible from all nodes.
{% endhint %}

## Network Requirements

### **Required Port Configuration for the Database Server**

The following ports are required to register an existing database in OwlDB.

<table data-full-width="true"><thead><tr><th>Port Type</th><th>Port Number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener Port</strong></td><td>Existing DB Setting Value</td><td><ul><li>Database Connection Port</li><li>Tibero: e.g., 8629/tcp</li></ul></td><td><ul><li>Allow inbound DB Listener port from the OwlDB server</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Do not change the ports currently used by the existing database. Additionally open port 40001 for Agent communication and the Listener port for DB connections.
{% endhint %}

### Firewall Settings

Firewall settings are required for communication between the OwlDB server and the database server.

---

## Database Configuration Requirements

### Required Settings for TAC and DR Configurations

For databases operating in a TAC (Tibero Active Cluster) or DR configuration, please configure the following parameters in advance.

**DB Parameter**

| DB Parameter | Value | Description |
| --- | --- | --- |
| LOG_ARCHIVE_FORMAT | - | Set to the same value on all nodes |
| STANDBY_USE_OBSERVER | Y | Required for DR configuration |

**CM Parameter**

| CM Parameter | Value | Description |
| --- | --- | --- |
| _CM_REDIRECT_STDOUT_TO_OUTFILE | Y | FS07PS_341175b patch required |

---

## Deployment File Preparation and Placement

### 1. List of Required Files

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- License file (`license.xml`)

{% hint style="info" %}
**Note**

Since Tibero is already installed on the registered DB, the Tibero binary is not required.
{% endhint %}

### 2. File Placement

Move to the path where the existing database is installed (hereafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

Extract the DP binary at the same level as the existing database's `TB_HOME`.

```bash
# Extract the DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place the license file
mv {license file} $TB_HOME/license.xml
```

After preparation is complete, the `$TB_HOME` structure is as follows.

```bash
$TB_HOME/
 ├── license.xml                      # License file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # Infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

Afterward, proceed to the [Database Server Agent Installation document](agent.md)and continue.
