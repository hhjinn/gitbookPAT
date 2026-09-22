Install the database to operate it in OwlDB. Once the database installation is complete, you can use all features provided by OwlDB.

**Check the following items before installation.**

- The installation feature is available only to administrator accounts.

### Standard Architecture

OwlDB provides a new database installation feature based on a standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Up to 4-node cluster configuration |
| Single + DR (Tibero) | Single configuration with 1 Standby node fixed |
| TAC + DR (Tibero) | TAC configuration with 1 Standby node fixed |
| HA (OpenSQL) | HA configuration with 1 Replica node fixed |

For TAC and DR configurations, all nodes are configured with the same specifications, and in a DR configuration the Standby node is fixed to one.

{% hint style="info" %}
**Note**

**Notice**

A Multi-node Cluster cannot be configured as Standby, and the number of Standby nodes is supported up to a maximum of one.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you change the database configuration directly from outside without going through OwlDB, some OwlDB features may not work properly. If a configuration change is needed, be sure to perform it through OwlDB.
{% endhint %}

---

### **Installation Process**

1. **OwlDB console screen** > **Dashboard**Navigate to it.
2. At the top of the dashboard, **Install** Click the button to navigate to the installation page.
3. Enter the installation options step by step. Entering the installation options consists of a total of 5 steps, and for details on each step, please refer to the [**Installation Option Steps** ](#undefined-2)section below.
4. Review the entered information, and once validation of installation feasibility is complete, **Install** Click the button.
5. Once installation starts, you can check the progress status in the dashboard list. When the status changes to **Running**the installation has completed successfully.

{% hint style="info" %}
**Note**

The database installation feature is available only from the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database Install button

During installation **Cancel** When you click the button, a confirmation modal appears, and when you click confirm in the modal, the entered information is reset and you are moved to the dashboard.

Once the installation request is received, you can check the installation start, completion, and failure status through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

**Install** When you click the button, verification of the license file existence and the core count is performed together.

- If there is no license file on the target installation host, the installation will not proceed, and you must place the license file and try again.
- If the requested core count exceeds the maximum core count of the held license, the installation will not proceed.
- If many installation requests are received simultaneously and processing is delayed, you must try again after a short while.
{% endhint %}

---

### **Installation Option Steps**

**Engine Options**

This step configures the database name, engine, and topology information.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name for identifying the DB Service<ul><li>Only 6–30 characters of English uppercase/lowercase letters, numbers, and hyphens (-) can be entered</li><li>Cannot be created as a duplicate within the OwlDB account</li><li>Default:<code>owldb-001</code>Assigned sequentially starting from</li></ul></td></tr><tr><td>DB Engine Type*</td><td>Database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB scaling through a redundancy configuration</li><li><strong>OpenSQL</strong> : An Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (select from 2–4)</li><li><strong>OpenSQL</strong> : Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to be used in OpenSQL (not applicable to Tibero)<ul><li>Default:<strong>17.9</strong></li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.

---

**DR Configuration**

This step configures whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (can be selected directly)</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 1: Automatic failover</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Single: Supports Levels 0, 1, and 3</li><li>TAC: Supports Levels 0 and 1</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode options<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits archive log files once they are generated after a log switch</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.
{% endtab %}
{% tab title="OpenSQL" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically set to not use DR, and HA to use DR; this cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover level</a><ul><li>Level 0: Manual</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Level 3: Full automation</li><li>Supports Levels 0 and 3 (default is Level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed to a maximum of 1 based on the standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to the ASYNC method; cannot be modified</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When DR usage is selected in the Enable DR item **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** these items are displayed.
- When using ARCH ASYNC mode, synchronization to the Standby may be delayed depending on the cycle at which archive logs are generated (up to approximately 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If Failover Automation Level is set to Level 3 (full automation), the entire process—from recovery to resource cleanup after failover—is handled automatically. Since recovery speed is prioritized above all, some recent data as of the failure point may be lost.
{% endhint %}

---

**Instance Configuration**

Input fields for each node are automatically configured according to the topology. The * mark indicates a required field.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Data Path* | Enter the Data Path | Only file system paths can be entered |
| Redo Path* | Enter the Redo Path | Only file system paths can be entered |
| Archive Path* | Enter Archive Path | Only file system paths can be entered |
| Backup Path* | Enter Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Each Path**can only accept file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, according to the selected topology configuration | - |
| Directly enter or select the IP to be used for communication between OwlDB and the node | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If using port forwarding, enter the external Port |
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

**All Paths**can only accept file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to be used for communication between nodes within the Cluster | Enter for both the Primary and StandBy clusters |
| Data Path* | Enter the data Path | Enter a raw device or partition path; uses a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; uses a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; uses a shared volume |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

**Data Path, Redo Path, Archive Path**enter a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths **shared volume**must be configured as.
- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Interconnect IP* | Select from the list the Interconnect IP to be used for communication between nodes within the Cluster | Enter for both the Primary and StandBy clusters |
| Primary Destination IP* (enter for Primary instances only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instances only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instances only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instances only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Data Path* | Enter the data Path | Enter a raw device or partition path; uses a shared volume |
| Redo Path* | Enter the Redo Path | Enter a raw device or partition path; uses a shared volume |
| Archive Path* | Enter the Archive Path | Enter a raw device or partition path; uses a shared volume |
| Backup Path* | Backup Path input | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [Configuring SSH Public Keys Between Nodes](#4VCx1BdX0fpROq6CwGnH)for details.
{% endhint %}

**Data Path, Redo Path, Archive Path**accept either a raw device path (`/dev/sdb`) or a partition path (`/dev/sdb1`). **Backup Path**only accepts a file system path.

However, the following conditions must be met.

- All entered paths must be **shared volumes**.
- The entered path must **actually exist**on the target installation host.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

{% hint style="info" %}
**Note**

- The input fields above are automatically configured for the number of nodes required by the selected topology.
- For a stable operating environment, all instances in the cluster are automatically configured with the same specifications.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
Node roles are displayed as **Leader/Replica**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the nodes | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Data Path* | Data Path input | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Each Path**only accepts a file system path. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the target installation host.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host on which to install the database, in accordance with the selected topology configuration | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to be used for the replication connection | - |
| Network Interface* | Select the network interface | - |
| Data Path* | Enter the Data Path | Only a file system path can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to be used for SSH connections between instances | Enter the common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the designated path in advance. For detailed configuration instructions, refer to [Configuring the SSH Public Key Between Nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**All Paths**can only be entered as file system paths. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.
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
| SYS User Password* | The password of the highest-privilege administrator account (SYS) of the database |
| Target Memory Size* | Target memory size |
| Shared Memory Size* | Shared memory size |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS timezone where the database will be installed |
| VIP* | Select whether to use VIP |
| Primary Node #N Vip | Database virtual IP<br>(Enabled when VIP use is selected) |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | The maximum number of concurrently allowed sessions |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | The size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | The sub data file size for storing system operation-related data |
| User Tablespace Data File Size (MB) | The tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (MB) | The temporary tablespace data file size used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>Name of the database to use</td></tr><tr><td>User Id*</td><td>Account ID of the database super-privilege administrator</td></tr><tr><td>User Password*</td><td>Password of the database super-privilege administrator account</td></tr><tr><td>Character Set*</td><td>Character encoding to use for the database</td></tr><tr><td>Timezone*</td><td>OS timezone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>Database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (cannot be modified)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>Displayed as an empty value because it cannot be determined during exploration, and cannot be modified</td></tr><tr><td>Connection Pooler Port</td><td>Port on which the connection pool in OpenSQL receives client connections<ul><li>Default value: 6432</li><li>Input range: 1024~65535</li></ul></td></tr><tr><td>Extension</td><td>Select Extensions to be installed together when creating the OpenSQL database (multiple selection possible)</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- The Connection Pooler Port and Extension items are exposed only when the OpenSQL engine is selected.
- Even if Extension installation fails, it does not affect database creation, and Extensions that failed to install can be checked in the system notifications.
{% endhint %}
{% endtab %}
{% endtabs %}

---

**Reviewing the configuration information**

You can enter this step only after validation of all the options entered in the previous steps has been completed.

Review all the entered configuration information at a glance, and if there are no issues, click the **Install** button to start the installation. To modify the content, click the **Previous** button.

{% hint style="info" %}
**Note**

**4. Database Configuration** In the step, **Next** when you click the button, validation of the entered information is performed automatically, and only after passing the validation can you enter the **5. Reviewing the configuration information** step.

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

If the check fails, an error message is displayed on the item where the error occurred, and you cannot move to the next step.
{% endhint %}

---

### Installation progress status

You can check the database installation progress in real time on the dashboard. The installation proceeds divided into major steps and detailed steps as shown in the table below, and the steps actually performed may differ depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major step | Detailed step |
| --- | --- |
| Verifying installation prerequisites | 1. sudo permission verification<br>2. Required file verification<br>3. parameter config verification |
| Infrastructure configuration | 1. Kernel environment configuration<br>2. Required package installation<br>3. Disk udev rule configuration<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero configuration | 1. Tibero instance Tip file & DSN file creation<br>2. Volume configuration change due to cm sequential installation<br>3. Volume wait due to cm sequential installation<br>4. Tip convert (primary <→ standby)<br>5. Standby installation completion tag configuration change<br>6. Standby installation completion tag wait<br>7. RMGR backup and transfer<br>8. RMGR backup wait |
| DB installation | 1. CM gen (resource registration and execution)<br>2. DB create (TAS also included depending on topology)<br>3. Disk snapshot creation<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero status check | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Main Steps | Detailed Steps |
| --- | --- |
| Infrastructure configuration | 1. Kernel environment configuration<br>2. Required package installation<br>3. PgAgent installation<br>4. mount volume<br>5. Data directory preparation |
| OpenSQL configuration | 1. Module configuration |
| OpenSQL Installation | 1. Post-bootstrap Configuration |
| OpenSQL Status Check | 1. Applying Settings After Initialization |
| PGAgent Installation | 1. PgAgent Configuration |
{% endtab %}
{% endtabs %}
