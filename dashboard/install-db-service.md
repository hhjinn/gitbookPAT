Install the database to operate it in OwlDB. Once the database installation is complete, you can use all the features provided by OwlDB.

**Check the following items before installation.**

- The installation feature can only be used by the administrator account.

# Standard architecture <a href="#standard-architecture" id="standard-architecture"></a>

OwlDB provides a new database installation feature based on standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Up to 4-node cluster configuration |
| Single + DR (Tibero) | Fixed to 1 Standby node in a Single configuration |
| TAC + DR (Tibero) | Fixed to 1 Standby node in a TAC configuration |
| HA (OpenSQL) | Fixed to 1 Replica node in an HA configuration |

For TAC and DR configurations, all nodes are configured with the same spec, and in a DR configuration, the Standby node is fixed to 1.

{% hint style="info" %}
**Note**

**Notice**

A Multi-node Cluster cannot be configured as Standby, and up to 1 Standby node is supported.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you change the database configuration directly from outside without going through OwlDB, some OwlDB features may not operate properly. If a configuration change is needed, please be sure to perform it through OwlDB.
{% endhint %}

---

# **Installation process** <a href="#installation-process" id="installation-process"></a>

1. Go to **OwlDB Console screen** > **Dashboard**.
2. Click the **Install** button at the top of the dashboard to move to the installation page.
3. Enter the installation options step by step. Entering the installation options consists of a total of 5 steps, and for details on each step, please refer to the [**Installation Option Steps** ](#installation-option-steps)section below.
4. Review the entered information, and once the installation feasibility verification is complete, click the **Install** button.
5. Once installation starts, you can check the progress status in the dashboard list. When the status changes to **Running**, the installation has completed normally.

{% hint style="info" %}
**Note**

The database installation feature can only be used from the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database installation button

If you click the **Cancel** button during installation, a confirmation modal appears, and if you click Confirm in the modal, the entered information is reset and you are moved to the dashboard.

Once the installation request is received, you can check the installation start, completion, and failure status through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

When you click the **Install** button, license file presence and core count verification are performed together.

- If there is no license file on the target host for installation, the installation does not proceed, and you must place the license file and try again.
- If the requested number of cores exceeds the maximum number of cores of the held license, the installation does not proceed.
- If processing is delayed because many installation requests are received simultaneously, you must try again after a while.
{% endhint %}

---

# **Installation Option Steps** <a href="#installation-option-steps" id="installation-option-steps"></a>

**Engine Options**

This is the step for setting the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name to identify the DB Service<ul><li>Only 6–30 characters of English uppercase/lowercase letters, numbers, and hyphens (-) can be entered</li><li>Cannot be created with a duplicate name within the OwlDB account</li><li>Default: assigned sequentially starting from <code>owldb-001</code></li></ul></td></tr><tr><td>DB Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB scaling through a redundancy configuration</li><li><strong>OpenSQL</strong>: Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (choose from 2–4)</li><li><strong>OpenSQL</strong>: Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to use in OpenSQL (not applicable to Tibero)<ul><li>Default: <strong>17.9</strong></li></ul></td></tr></tbody></table>

The * notation indicates a required input item.

---

**DR Configuration**

This is the step for setting whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (can be selected directly)</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 1: Automatic failover</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Single: Levels 0, 1, 3 supported</li><li>TAC: Levels 0, 1 supported</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li></ul></td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically set to not use DR, and HA to use DR; this cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Levels 0, 3 supported (default: Level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to the ASYNC method and cannot be modified</td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When you select to use DR in the Enable DR item, the **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** items are displayed.
- When using ARCH ASYNC mode, synchronization to the Standby may be delayed depending on the interval at which archive logs are generated (up to about 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If you set the Failover Automation Level to Level 3 (full automation), all processes—from failover through recovery and resource cleanup—are handled automatically. Since recovery speed is given top priority, some recent data as of the time of failure may be lost.
{% endhint %}

---

**Instance Configuration**

Input items for each node are configured automatically according to the topology. An asterisk (*) indicates a required input item.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

For **each Path**, only a file system path can be entered. Entering the same path or different paths in duplicate is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Entered only for the Primary instance) | Directly enter the Primary Destination IP to be used in communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP<br>Entered only for the Primary instance |
| Primary Destination Port*<br>(Entered only for the Primary instance) | Directly enter the Primary Destination Port to be used in communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination IP to be used in communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination Port to be used in communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

For **all Paths**, only a file system path can be entered. Entering the same path or different paths in duplicate is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to be used for communication between nodes within the Cluster from the list | Enter both the Primary and Standby clusters |
| Data Path* | Enter the data Path | Enter a raw device or partition path; use a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; use a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; use a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

For **Data Path, Redo Path, Archive Path**, enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). For the **Backup Path**, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must be configured as a **shared volume**.
- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to be used for communication between nodes within the Cluster from the list | Enter both the Primary and Standby clusters |
| Primary Destination IP* (Entered only for the Primary instance) | Directly enter the Primary Destination IP to be used in communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(Entered only for the Primary instance) | Directly enter the Primary Destination Port to be used in communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination IP to be used in communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination Port to be used in communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the data Path | Enter a raw device or partition path; use a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; use a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; use a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

For **Data Path, Redo Path, Archive Path**, enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). For the **Backup Path**, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must be configured as a **shared volume**.
- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

{% hint style="info" %}
**Note**

- The input items above are configured automatically for as many nodes as required by the selected topology.
- For a stable operating environment, all instances within the cluster are configured automatically with the same specifications.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
The node role is displayed as **Leader/Replica**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

For **each Path**, only a file system path can be entered. Entering the same path or different paths in duplicate is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to be used for the replication connection | - |
| Network interface* | Select a network interface | - |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

For **all Paths**, only a file system path can be entered. Entering the same path or different paths in duplicate is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.
{% endtab %}
{% endtabs %}

---

**Database Configuration**

This is the step for entering the database configuration information.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name* | The name of the database to be used |
| SYS User Password* | The password of the database super administrator account (SYS) |
| Target Memory Size* | Target memory size |
| Shared Memory Size* | Shared memory size |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS time zone where the database will be installed |
| VIP* | Select whether to use VIP |
| Primary Node #N Vip | Database virtual IP<br>(enabled when Use VIP is selected) |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | Size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | Sub data file size for storing system operation-related data |
| User Tablespace Data File Size (MB) | Tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (MB) | Temporary tablespace data file size used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>The name of the database to be used</td></tr><tr><td>User Id*</td><td>Database top-privilege administrator account ID</td></tr><tr><td>User Password*</td><td>The password of the database super administrator account</td></tr><tr><td>Character Set*</td><td>The character encoding to be used for the database</td></tr><tr><td>Timezone*</td><td>The OS time zone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>Database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (not modifiable)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified</td></tr><tr><td>Connection Pooler Port</td><td>Port on which the connection pool in OpenSQL receives client connections<ul><li>Default: 6432</li><li>Input range: 1024–65535</li></ul></td></tr><tr><td>Extension</td><td>Select the Extensions to install together when creating the OpenSQL database (multiple selections possible)</td></tr></tbody></table>

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

- The Connection Pooler Port and Extension items are displayed only when the OpenSQL engine is selected.
- Even if Extension installation fails, it does not affect database creation, and Extensions that failed to install can be checked in the system notifications.
{% endhint %}
{% endtab %}
{% endtabs %}

---

**Configuration Information Confirmation**

You can enter this step only after validation of all the options entered in the previous steps has been completed.

Review the configuration information you entered at a glance, and if there are no issues, click the **Install** button to start the installation. To modify the content, please click the **Previous** button.

{% hint style="info" %}
**Note**

When you click the **Next** button in the **4. Database Configuration** step, validation of the entered information is performed automatically, and you can enter the **5. Configuration Information Confirmation** step only after passing the validation.

**[Tibero]** The items below are additionally checked.

- Whether the entered Data/Redo/Archive/Backup Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the OS Timezone matches across all nodes
- Whether the total of the entered Data File sizes does not exceed the available storage space

**[OpenSQL]** The items below are additionally checked.

- Whether the entered Data Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the SSH Key File Path actually exists and whether it is a Private key
- Whether the OS Timezone matches across all nodes
- Whether the total of the entered Data File sizes does not exceed the available storage space

If the check fails, an error message is displayed on the item where the error occurred, and you cannot move to the next step.
{% endhint %}

---

# Installation progress status <a href="#installation-progress-status" id="installation-progress-status"></a>

You can monitor the database installation progress in real time on the dashboard. As shown in the table below, the installation proceeds in major steps and detailed steps, and the actual steps performed may vary depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major step | Detailed step |
| --- | --- |
| Installation prerequisite validation | 1. sudo privilege validation<br>2. Required file validation<br>3. parameter config validation |
| Infrastructure setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. Disk udev rule configuration<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero setup | 1. Tibero instance Tip file & DSN file creation<br>2. volume configuration change due to cm sequential installation<br>3. volume wait due to cm sequential installation<br>4. Tip convert (primary <→ standby)<br>5. Standby installation completion tag configuration change<br>6. Standby installation completion tag wait<br>7. RMGR backup and transfer<br>8. RMGR backup wait |
| DB installation | 1. CM gen (resource registration and execution)<br>2. DB create (TAS also included depending on topology)<br>3. Disk snapshot creation<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero status check | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Major step | Detailed step |
| --- | --- |
| Infrastructure setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. PgAgent installation<br>4. mount volume<br>5. Data directory preparation |
| OpenSQL setup | 1. Module configuration |
| OpenSQL installation | 1. Post-bootstrap configuration |
| OpenSQL status check | 1. Apply configuration after initialization |
| PGAgent installation | 1. PgAgent configuration |
{% endtab %}
{% endtabs %}
