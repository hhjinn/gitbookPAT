Install the database to operate it in OwlDB. Once the database installation is complete, you can use all the features provided by OwlDB.

**Check the following before installation.**

- The installation feature can only be used by the administrator account.

### Standard Architecture <a href="#undefined" id="undefined"></a>

OwlDB provides a new database installation feature based on standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Up to 4-node cluster configuration |
| Single + DR (Tibero) | 1 Standby node fixed in Single configuration |
| TAC + DR (Tibero) | 1 Standby node fixed in TAC configuration |
| HA (OpenSQL) | 1 Replica node fixed in HA configuration |

For TAC and DR configurations, all nodes are configured with the same spec, and in DR configuration, the Standby node is fixed to 1.

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

### **Installation Process** <a href="#undefined-1" id="undefined-1"></a>

1. **OwlDB Console Screen** > **Dashboard**to navigate.
2. At the top of the dashboard, **Install** Click the button to move to the installation page.
3. Enter the installation options step by step. The installation option entry consists of a total of 5 steps, and for details on each step, please refer to the [**Installation Option Steps** ](#undefined-2)section below.
4. Once you verify the entered information and the installation feasibility validation is complete, **Install** Click the button.
5. When installation starts, you can check the progress status in the dashboard list. When the status changes to **Running**the installation has been completed normally.

{% hint style="info" %}
**Note**

The database installation feature can only be used by the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database installation button

Installation in progress **Cancel** When you click the button, a confirmation modal appears, and when you click Confirm in the modal, the entered information is reset and you are moved to the dashboard.

Once the installation request is received, you can check the installation start, completion, and failure status through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

**Install** Clicking the button triggers verification of both the license file's existence and the core count.

- If there is no license file on the target installation host, the installation will not proceed, and you must place the license file before trying again.
- If the requested core count exceeds the maximum core count of the held license, the installation will not proceed.
- If many installation requests are received simultaneously and processing is delayed, you must try again shortly.
{% endhint %}

---

### **Installation Option Steps** <a href="#undefined-2" id="undefined-2"></a>

**Engine options**

This is the step for setting the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>A name for identifying the DB Service<ul><li>Only 6 to 30 characters consisting of uppercase/lowercase English letters, numbers, and hyphens (-) can be entered</li><li>Cannot be created in duplicate within an OwlDB account</li><li>Default value :<code>owldb-001</code>Assigned sequentially starting from</li></ul></td></tr><tr><td>DB Engine Type*</td><td>The database engine to be used<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB scaling through a multiplexed configuration</li><li><strong>OpenSQL</strong> : Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (select from 2 to 4)</li><li><strong>OpenSQL</strong> : Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>The PostgreSQL version to use in OpenSQL (not applicable to Tibero)<ul><li>Default value :<strong>17.9</strong></li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

---

**DR configuration**

This is the step for setting whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (can be selected directly)</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 1: Automatic failover</li><li>Level 2 : Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Fully automated</li><li>Single : Levels 0, 1, and 3 supported</li><li>TAC : Levels 0 and 1 supported</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed at a maximum of 1 based on the standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>The log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when transactions occur</li><li><strong>ARCH ASYNC</strong> : A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically set to not use DR, and HA to use DR, and cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/dashboard/db-1.md#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 2 : Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Fully automated</li><li>Levels 0 and 3 supported (default: Level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed at a maximum of 1 based on the standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to the ASYNC method and cannot be modified</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When DR usage is selected in the Enable DR item **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** items are displayed.
- When using ARCH ASYNC mode, synchronization to the Standby may be delayed depending on the cycle at which archive logs are generated (up to approximately 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If you set the Failover Automation Level to Level 3 (full automation), the entire process—from failover to recovery and resource cleanup—is handled automatically. Since recovery speed is prioritized above all, some recent data as of the point of failure may be lost.
{% endhint %}

---

**Instance Configuration**

Input items for each node are automatically configured according to the topology. An asterisk (*) indicates a required input item.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Each Path**accepts only file system paths. Entering the same path or different paths in duplicate is also permitted.

However, the following conditions must be met.

- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(Enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| Redo Path* | Enter the Redo Path | Only a file system path can be entered |
| Archive Path* | Enter the Archive Path | Only a file system path can be entered |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in advance at the designated path. For detailed configuration instructions, [Configuring SSH public keys between nodes](#4VCx1BdX0fpROq6CwGnH)see .
{% endhint %}

**All Paths**accepts only file system paths. Entering the same path or different paths in duplicate is also permitted.

However, the following conditions must be met.

- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to use for communication between nodes within the Cluster from the list | Enter for both the Primary and Standby clusters |
| Data Path* | Enter the Data Path | Enter a raw device or partition path, using a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path, using a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path, using a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Data Path, Redo Path, Archive Path**Enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**In this case, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **a shared volume**be configured as.
- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to use for communication between nodes within the Cluster from the list | Enter for both the Primary and Standby clusters |
| Primary Destination IP* (Enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(Enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Enter a raw device or partition path, using a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path, using a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path, using a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in advance at the designated path. For detailed configuration instructions, [Configuring SSH public keys between nodes](#4VCx1BdX0fpROq6CwGnH)see .
{% endhint %}

**Data Path, Redo Path, Archive Path**Enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**In this case, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **a shared volume**be configured as.
- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

{% hint style="info" %}
**Note**

- The above input items are automatically configured for as many nodes as required by the selected topology.
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
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Each Path**accepts only file system paths. Entering the same path or different paths in duplicate is also permitted.

However, the following conditions must be met.

- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to use for the replication connection | - |
| Network Interface* | Select the network interface | - |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in advance at the designated path. For detailed configuration instructions, [Configuring SSH public keys between nodes](#4VCx1BdX0fpROq6CwGnH)see .
{% endhint %}

**All Paths**accepts only file system paths. Entering the same path or different paths in duplicate is also permitted.

However, the following conditions must be met.

- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.
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
| SYS User Password* | The password of the database's highest-privilege administrator account (SYS) |
| Target Memory Size* | Target memory size |
| Shared Memory Size* | Shared memory size |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS time zone where the database will be installed |
| VIP* | Select whether to use VIP |
| Primary Node #N Vip | Database virtual IP<br>(enabled when Use VIP is selected) |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | The size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | The size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | The size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (MB) | The size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>The name of the database to be used</td></tr><tr><td>User Id*</td><td>The ID of the database's highest-privilege administrator account</td></tr><tr><td>User Password*</td><td>The password of the database's highest-privilege administrator account</td></tr><tr><td>Character Set*</td><td>The character encoding to be used for the database</td></tr><tr><td>Timezone*</td><td>The OS time zone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>Database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (not modifiable)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process</td></tr><tr><td>Connection Pooler Port</td><td>The port on which the connection pool receives client connections in OpenSQL<ul><li>Default value : 6432</li><li>Input range : 1024 to 65535</li></ul></td></tr><tr><td>Extension</td><td>Select the Extensions to install together when creating the OpenSQL database (multiple selections possible)</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

- The Connection Pooler Port and Extension items are displayed only when the OpenSQL engine is selected.
- Even if Extension installation fails, it does not affect database creation, and failed Extensions can be checked in the system notifications.
{% endhint %}
{% endtab %}
{% endtabs %}

---

**Configuration Information Review**

You can enter this step after verification of all the options entered in the preceding steps has been completed.

Review the entered configuration information at a glance, and if there are no issues, **Install** Click the button to start the installation. To modify the content, **Previous** Please click the button.

{% hint style="info" %}
**Note**

**4. Database Configuration** at the step **Next** When you click the button, validation of the entered information is performed automatically, and you must pass the validation to **5. Verify Configuration Information** proceed to the step.

**[Tibero]** The following items are additionally checked.

- Whether the entered Data/Redo/Archive/Backup Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the OS Timezone of all nodes matches
- Whether the total size of the entered Data Files does not exceed the available storage space

**[OpenSQL]** The following items are additionally checked.

- Whether the entered Data Path actually exists on the installation target host
- Whether SSH connection between nodes is possible
- Whether the SSH Key File Path actually exists and whether it is a Private key
- Whether the OS Timezone of all nodes matches
- Whether the total size of the entered Data Files does not exceed the available storage space

If the validation fails, an error message is displayed for the item where the error occurred, and you cannot proceed to the next step.
{% endhint %}

---

### Installation Progress Status <a href="#undefined-6" id="undefined-6"></a>

You can check the installation progress of the database in real time from the dashboard. The installation proceeds divided into major steps and detailed steps as shown in the table below, and the steps actually performed may differ depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major Steps | Detailed Steps |
| --- | --- |
| Installation Prerequisite Validation | 1. sudo privilege validation<br>2. Required file validation<br>3. parameter config validation |
| Infrastructure Configuration | 1. Kernel environment configuration<br>2. Required package installation<br>3. Disk udev rule configuration<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero Configuration | 1. Create Tibero instance Tip file & DSN file<br>2. Change volume configuration due to cm sequential installation<br>3. Wait for volume due to cm sequential installation<br>4. Tip convert (primary <→ standby)<br>5. Change Standby installation complete tag setting<br>6. Wait for Standby installation complete tag<br>7. RMGR backup and transfer<br>8. Wait for RMGR backup |
| DB Installation | 1. CM gen (register and run resource)<br>2. DB create (including TAS depending on topology)<br>3. Create Disk snapshot<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero Status Check | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Major Steps | Detailed Steps |
| --- | --- |
| Infrastructure Configuration | 1. Kernel environment configuration<br>2. Required package installation<br>3. PgAgent installation<br>4. mount volume<br>5. Prepare data directory |
| OpenSQL Configuration | 1. Module configuration |
| OpenSQL Installation | 1. Post-bootstrap configuration |
| OpenSQL Status Check | 1. Apply settings after initialization |
| PGAgent Installation | 1. PgAgent configuration |
{% endtab %}
{% endtabs %}
