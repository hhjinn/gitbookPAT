Install a database to operate it in OwlDB. Once the database installation is complete, you can use all the features provided by OwlDB.

**Check the following items before installation.**

- The installation feature can only be used by administrator accounts.

# Standard architecture <a href="#standard-architecture" id="standard-architecture"></a>

OwlDB provides a new database installation feature based on standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Up to 4-node cluster configuration |
| Single + DR (Tibero) | Standby fixed at 1 node in a Single configuration |
| TAC + DR (Tibero) | Standby fixed at 1 node in a TAC configuration |
| HA (OpenSQL) | Replica fixed at 1 node in an HA configuration |

For TAC and DR configurations, all nodes are configured with the same spec, and for DR configurations the Standby node is fixed at 1.

{% hint style="info" %}
**Note**

**Note**

A Multi-node Cluster cannot be configured as Standby, and up to 1 Standby node is supported.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you change the database configuration directly from outside without going through OwlDB, some OwlDB features may not operate normally. If a configuration change is needed, be sure to perform it through OwlDB.
{% endhint %}

---

# **Installation process** <a href="#installation-process" id="installation-process"></a>

1. **OwlDB console screen** > **Dashboard**Navigate to it.
2. At the top of the dashboard, the **Install** Click the button to go to the installation page.
3. Enter the installation options step by step. Entering installation options consists of a total of 5 steps, and for details on each step, please refer to the [**Installation option steps** ](#installation-option-steps)section below.
4. After verifying the entered information and completing the validation of installation feasibility, **Install** Click the button.
5. Once installation starts, you can check the progress status in the dashboard list. When the status changes to **Running**the installation has been completed normally.

{% hint style="info" %}
**Note**

The database installation feature can only be used by the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database install button

Installation in progress **Cancel** When you click the button, a confirmation modal appears, and when you click confirm in the modal, the entered information is reset and you are moved to the dashboard.

Once the installation request is received, you can check the installation start, completion, and failure status through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

**Install** Clicking the button simultaneously verifies the presence of the license file and the core count.

- If there is no license file on the target installation host, the installation will not proceed, and you must place the license file and try again.
- If the requested core count exceeds the maximum core count of your license, the installation will not proceed.
- If processing is delayed due to many installation requests being received simultaneously, you must try again after a while.
{% endhint %}

---

# **Installation option steps** <a href="#installation-option-steps" id="installation-option-steps"></a>

**Engine options**

This is the step for configuring the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>A name to identify the DB Service<ul><li>Only 6–30 characters of uppercase/lowercase English letters, numbers, and hyphens (-) can be entered</li><li>Cannot be created with a duplicate name within an OwlDB account</li><li>Default value:<code>owldb-001</code>Assigned sequentially starting from</li></ul></td></tr><tr><td>DB Engine Type*</td><td>Database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a multiplexed configuration</li><li><strong>OpenSQL</strong> : Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (select from 2–4)</li><li><strong>OpenSQL</strong> : Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to use in OpenSQL (not applicable to Tibero)<ul><li>Default value:<strong>17.9</strong></li></ul></td></tr></tbody></table>

The * notation indicates a required input item.

---

**DR configuration**

This is the step for setting whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (can be selected directly)</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 1: Automatic failover</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Single: Levels 0, 1, 3 supported</li><li>TAC: Levels 0, 1 supported</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when transactions occur</li><li><strong>ARCH ASYNC</strong> : A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li></ul></td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically set to not use DR, and HA to use DR, and cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Levels 0, 3 supported (default is Level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to the ASYNC method and cannot be modified</td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When DR use is selected in the Enable DR item **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** items are displayed.
- When using ARCH ASYNC mode, synchronization to Standby may be delayed depending on the cycle at which archive logs are generated (up to approximately 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If you set the Failover Automation Level to Level 3 (fully automated), the entire process from failover through recovery and resource cleanup is handled automatically. Because recovery speed is prioritized above all, some recent data as of the time of failure may be lost.
{% endhint %}

---

**Instance Configuration**

Input items for each node are automatically configured according to the topology. The * notation indicates a required input item.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

**Each Path**can only be entered as a file system path. Entering the same path or different paths redundantly is also permitted.

However, the following conditions must be met.

- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandBy DB to the Primary DB | If using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, you must place the SSH Key File at the specified path in advance. For detailed setup instructions, [Configuring the SSH public key between nodes](#4VCx1BdX0fpROq6CwGnH)refer to.
{% endhint %}

**All Paths**can only be entered as a file system path. Entering the same path or different paths redundantly is also permitted.

However, the following conditions must be met.

- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to use for communication between nodes inside the Cluster | Enter for both the Primary and Standby clusters |
| Data Path* | Enter the Data Path | Enter the raw device or partition path, using a shared volume |
| Redo Path* | Enter the Redo Path | Enter the raw device or partition path, using a shared volume |
| Archive Path* | Enter the Archive Path | Enter the raw device or partition path, using a shared volume |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

**Data Path, Redo Path, Archive Path**Enter the raw device path (`/dev/sdb`) or partition path (`/dev/sdb1`). **Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to use for communication between nodes inside the Cluster | Enter for both the Primary and Standby clusters |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandBy DB to the Primary DB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Enter the raw device or partition path, using a shared volume |
| Redo Path* | Enter the Redo Path | Enter the raw device or partition path, using a shared volume |
| Archive Path* | Enter the Archive Path | Enter the raw device or partition path, using a shared volume |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, you must place the SSH Key File at the specified path in advance. For detailed setup instructions, [Configuring the SSH public key between nodes](#4VCx1BdX0fpROq6CwGnH)refer to.
{% endhint %}

**Data Path, Redo Path, Archive Path**Enter the raw device path (`/dev/sdb`) or partition path (`/dev/sdb1`). **Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

{% hint style="info" %}
**Note**

- The above input items are automatically configured for as many nodes as needed according to the selected topology.
- For a stable operating environment, all instances within the cluster are automatically configured with the same specifications.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
The node role is displayed as **Leader/Replica**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

**Each Path**can only be entered as a file system path. Entering the same path or different paths redundantly is also permitted.

However, the following conditions must be met.

- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to use for the replication connection | - |
| Network interface* | Select a network interface | - |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, you must place the SSH Key File at the specified path in advance. For detailed setup instructions, [Configuring the SSH public key between nodes](#4VCx1BdX0fpROq6CwGnH)refer to.
{% endhint %}

**All Paths**can only be entered as a file system path. Entering the same path or different paths redundantly is also permitted.

However, the following conditions must be met.

- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.
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
| SYS User Password* | The password of the database highest-privilege administrator account (SYS) |
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
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | Size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>The name of the database to be used</td></tr><tr><td>User Id*</td><td>ID of the database's highest-privilege administrator account</td></tr><tr><td>User Password*</td><td>The password of the database highest-privilege administrator account</td></tr><tr><td>Character Set*</td><td>The character encoding to be used for the database</td></tr><tr><td>Timezone*</td><td>The OS time zone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>Database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (not modifiable)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified</td></tr><tr><td>Connection Pooler Port</td><td>The port on which the connection pool receives client connections in OpenSQL<ul><li>Default value: 6432</li><li>Input range: 1024–65535</li></ul></td></tr><tr><td>Extension</td><td>Select Extensions to install together when creating an OpenSQL database (multiple selections possible)</td></tr></tbody></table>

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

You can enter this step after validation has been completed for all options entered in the previous steps.

Check the entered configuration information at a glance, and if there are no issues, **Install** Click the button to start the installation. To modify the content, **Previous** Please click the button.

{% hint style="info" %}
**Note**

**4. Database Configuration** at the step, **Next** When you click the button, validation of the entered information is performed automatically, and you must pass the validation **5. Verify Configuration Information** to proceed to the step.

**[Tibero]** Additionally verify the following items.

- Whether the entered Data/Redo/Archive/Backup Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the OS Timezone of all nodes matches
- Whether the total size of the entered Data Files does not exceed the available storage space

**[OpenSQL]** Additionally verify the following items.

- Whether the entered Data Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the SSH Key File Path actually exists and whether it is a Private key
- Whether the OS Timezone of all nodes matches
- Whether the total size of the entered Data Files does not exceed the available storage space

If the validation fails, an error message is displayed for the item where the error occurred, and you cannot proceed to the next step.
{% endhint %}

---

# Installation Progress Status <a href="#installation-progress-status" id="installation-progress-status"></a>

You can check the database installation progress in real time on the dashboard. The installation proceeds divided into major steps and detailed steps as shown in the table below, and the steps actually performed may differ depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major Step | Detailed Step |
| --- | --- |
| Verify Installation Prerequisites | 1. Verify sudo privileges<br>2. Verify required files<br>3. Verify parameter config |
| Infrastructure Configuration | 1. Configure Kernel environment<br>2. Install required packages<br>3. Configure Disk udev rule<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero Configuration | 1. Create Tibero instance Tip file & DSN file<br>2. Change volume configuration due to cm sequential installation<br>3. Wait for volume due to cm sequential installation<br>4. Tip convert (primary <→ standby)<br>5. Change Standby installation completion tag setting<br>6. Wait for Standby installation completion tag<br>7. RMGR backup and transfer<br>8. Wait for RMGR backup |
| DB Installation | 1. CM gen (register and run resource)<br>2. DB create (TAS also included depending on topology)<br>3. Create Disk snapshot<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Verify Tibero Status | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Major Step | Detailed Step |
| --- | --- |
| Infrastructure Configuration | 1. Configure Kernel environment<br>2. Install required packages<br>3. Install PgAgent<br>4. mount volume<br>5. Prepare data directory |
| OpenSQL Configuration | 1. Configure module |
| OpenSQL Installation | 1. Configuration after bootstrap |
| OpenSQL Status Check | 1. Apply settings after initialization |
| PGAgent Installation | 1. Configure PgAgent |
{% endtab %}
{% endtabs %}
