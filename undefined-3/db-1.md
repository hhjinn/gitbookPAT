To operate a database in OwlDB, install the database. Once database installation is complete, you can use all features provided by OwlDB.

**Check the following items before installation.**

- The installation feature is available only to administrator accounts.

### Standard Architecture

OwlDB provides a new database installation feature based on standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Cluster configuration of up to 4 nodes |
| Single + DR (Tibero) | Single configuration with 1 fixed Standby node |
| TAC + DR (Tibero) | TAC configuration with 1 fixed Standby node |
| HA (OpenSQL) | HA configuration with 1 fixed Replica node |

For TAC and DR configurations, all nodes are configured with the same specifications, and for DR configurations the Standby node is fixed at 1.

{% hint style="info" %}
**Note**

**Notice**

A Multi-node Cluster cannot be configured as a Standby, and up to 1 Standby node is supported.
{% endhint %}

{% hint style="warning" %}
**Caution**

If the database configuration is changed directly from outside without going through OwlDB, some OwlDB features may not work properly. If a configuration change is needed, be sure to perform it through OwlDB.
{% endhint %}

---

### **Installation Process**

1. **OwlDB console screen** > **Dashboard**Navigate to.
2. At the top of the dashboard, **Install** Click the button to navigate to the installation page.
3. Enter the installation options step by step. Entering installation options consists of a total of 5 steps. For details on each step, refer to the [**Installation Option Steps** ](#undefined-2)section below.
4. Once you have verified the entered information and the installation feasibility validation is complete, **Install** Click the button.
5. Once installation begins, you can check the progress status in the dashboard list. When the status changes to **Running**, the installation has completed successfully.

{% hint style="info" %}
**Note**

The database installation feature is available only from the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database installation button

During installation, **Cancel** When you click the button, a confirmation modal appears, and when you click Confirm in the modal, the entered information is reset and you are moved to the dashboard.

Once the installation request is received, you can check whether installation has started, completed, or failed through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

**Install** When you click the button, verification of the license file presence and core count is also performed.

- If there is no license file on the target host for installation, the installation will not proceed, and you must place the license file and try again.
- If the requested core count exceeds the maximum core count of the license you hold, the installation will not proceed.
- If processing is delayed due to many installation requests being received at the same time, try again after a while.
{% endhint %}

---

### **Installation Option Steps**

**Engine Options**

This step configures the database name, engine, and topology information.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name to identify the DB Service<ul><li>Only 6 to 30 characters of uppercase and lowercase English letters, numbers, and hyphens (-) can be entered</li><li>Duplicate creation is not allowed within an OwlDB account</li><li>Default value:<code>owldb-001</code>Assigned sequentially starting from</li></ul></td></tr><tr><td>DB Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a redundancy configuration</li><li><strong>OpenSQL</strong> : An Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (select from 2 to 4)</li><li><strong>OpenSQL</strong> : Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to use in OpenSQL (not applicable to Tibero)<ul><li>Default:<strong>17.9</strong></li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.

---

**DR Configuration**

This is the step for configuring whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (can be selected directly)</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 1: Automatic failover</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Single: Supports levels 0, 1, and 3</li><li>TAC: Supports levels 0 and 1</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode options<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.
{% endtab %}
{% tab title="OpenSQL" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically set to not use DR and HA to use DR; cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Supports levels 0 and 3 (default is level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to ASYNC mode; cannot be modified</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When DR usage is selected in the Enable DR item **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** items are displayed.
- When using ARCH ASYNC mode, synchronization to the Standby may be delayed depending on the cycle in which archive logs are generated (up to approximately 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If Failover Automation Level is set to Level 3 (full automation), all processes from recovery to resource cleanup are handled automatically after failover. Because recovery speed is prioritized above all else, some recent data as of the time of failure may be lost.
{% endhint %}

---

**Instance Configuration**

The input fields for each node are automatically configured according to the topology. The * mark indicates a required field.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, according to the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and nodes | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only file system paths can be entered |
| Redo Path* | Enter the Redo Path | Only file system paths can be entered |
| Archive Path* | Enter Archive Path | Only file system paths can be entered |
| Backup Path* | Enter Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Each Path**only accepts file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, according to the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | If NAT is used, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If port forwarding is used, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If NAT is used, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If port forwarding is used, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If NAT is used, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If port forwarding is used, enter the external Port |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| Redo Path* | Enter Redo Path | Only file system paths can be entered |
| Archive Path* | Enter Archive Path | Only file system paths can be entered |
| Backup Path* | Enter Backup Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in advance at the specified path. For detailed configuration instructions, refer to [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**All Paths**only accept file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, according to the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to be used for communication between nodes within the Cluster | Enter for both the Primary and StandBy clusters |
| Data Path* | Enter the data Path | Enter a raw device or partition path; use a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; use a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; use a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Data Path, Redo Path, Archive Path**enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must **read, write, and execute permissions**hold.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, according to the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to be used for communication between nodes within the Cluster | Enter for both the Primary and StandBy clusters |
| Primary Destination IP* (enter for Primary instances only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instances only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instances only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instances only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the data Path | Enter a raw device or partition path; use a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; use a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; use a shared volume |
| Backup Path* | Enter Backup Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

An asterisk (*) indicates a required field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in advance at the specified path. For detailed configuration instructions, see [Configuring SSH public keys between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**Data Path, Redo Path, Archive Path**Enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**Only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths **shared volume**must be configured as.
- The entered path must **actually exist**on the target installation host.
- For the file system path, the account running the DP Agent must have **read, write, and execute permissions**.

{% hint style="info" %}
**Note**

- Depending on the selected topology, the above input fields are automatically configured for the required number of nodes.
- For a stable operating environment, all instances in the cluster are automatically configured with the same specifications.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
The node role is displayed as **Leader/Replica**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

An asterisk (*) indicates a required field.

**Each Path**Only file system paths can be entered. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the target installation host.
- For the file system path, the account running the DP Agent must have **read, write, and execute permissions**.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and nodes | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to be used for the replication connection | - |
| Network Interface* | Select the network interface | - |
| Data Path* | Enter the Data Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the designated path in advance. For detailed configuration instructions, refer to [Configuring the SSH public key between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**All Paths**can only accept file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the target installation host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.
{% endtab %}
{% endtabs %}

---

**Database Configuration**

This is the step for entering database configuration information.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name* | The name of the database to be used |
| SYS User Password* | The password of the database's highest-privilege administrator account (SYS) |
| Target Memory Size* | Target memory size |
| Shared Memory Size* | Shared memory size |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS timezone where the database will be installed |
| VIP* | Select whether to use VIP |
| Primary Node #N Vip | Database virtual IP<br>(Activated when VIP usage is selected) |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | The size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | The size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | The size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (MB) | The size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>Name of the database to use</td></tr><tr><td>User Id*</td><td>Database super-privilege administrator account ID</td></tr><tr><td>User Password*</td><td>Password of the database super-privilege administrator account</td></tr><tr><td>Character Set*</td><td>Character encoding to use for the database</td></tr><tr><td>Timezone*</td><td>OS timezone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>Database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (not editable)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>The value cannot be determined during the discovery process, so it is displayed as empty and cannot be edited</td></tr><tr><td>Connection Pooler Port</td><td>The port on which the connection pool listens for client connections in OpenSQL<ul><li>Default value: 6432</li><li>Input range: 1024–65535</li></ul></td></tr><tr><td>Extension</td><td>Select Extensions to install together when creating the OpenSQL database (multiple selections allowed)</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- The Connection Pooler Port and Extension items are only shown when the OpenSQL engine is selected.
- Even if an Extension fails to install, it does not affect database creation, and Extensions that failed to install can be checked in the system notifications.
{% endhint %}
{% endtab %}
{% endtabs %}

---

**Reviewing Configuration Information**

You can enter this step only after all options entered in the previous steps have been validated.

Review all the configuration information you entered at a glance, and if there are no issues, click the **Install** button to start the installation. To modify the content, click the **Previous** button.

{% hint style="info" %}
**Note**

**4. Database Configuration** In the step, **Next** when you click the button, validation of the entered information is performed automatically, and only after passing the validation can you **5. Reviewing Configuration Information** enter the step.

**[Tibero]** The following items are additionally checked.

- Whether the entered Data/Redo/Archive/Backup Path actually exists on the target installation host
- Whether SSH connection between nodes is possible
- Whether the OS Timezone matches across all nodes
- Whether the total of the entered Data File sizes does not exceed the available storage space

**[OpenSQL]** The following items are additionally checked.

- Whether the entered Data Path actually exists on the target installation host
- Whether SSH connection between nodes is possible
- Whether the SSH Key File Path actually exists and whether it is a Private key
- Whether the OS Timezone matches across all nodes
- Whether the total of the entered Data File sizes does not exceed the available storage space

If the check fails, an error message is displayed for the item where the error occurred, and you cannot proceed to the next step.
{% endhint %}

---

### Installation Progress Status

You can check the installation progress of the database in real time on the dashboard. The installation proceeds divided into major steps and detailed steps as shown in the table below, and the steps actually performed may differ depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major step | Detailed step |
| --- | --- |
| Installation prerequisite validation | 1. sudo privilege validation<br>2. Required file validation<br>3. parameter config validation |
| Infrastructure setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. Disk udev rule configuration<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero configuration | 1. Tibero instance Tip file & DSN file creation<br>2. Volume configuration change due to cm sequential installation<br>3. Volume wait due to cm sequential installation<br>4. Tip convert (primary ↔ standby)<br>5. Standby installation completion tag configuration change<br>6. Standby installation completion tag wait<br>7. RMGR backup and transfer<br>8. RMGR backup wait |
| DB installation | 1. CM gen (resource registration and execution)<br>2. DB create (also includes TAS depending on topology)<br>3. Disk snapshot creation<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero status check | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Main Steps | Detailed Steps |
| --- | --- |
| Infrastructure setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. PgAgent installation<br>4. mount volume<br>5. Data directory preparation |
| OpenSQL configuration | 1. Module configuration |
| OpenSQL Installation | 1. Post-Bootstrap Configuration |
| OpenSQL Health Check | 1. Applying Settings After Initialization |
| PGAgent Installation | 1. PgAgent Configuration |
{% endtab %}
{% endtabs %}
