Install a database in order to operate it in OwlDB. Once the database installation is complete, you can use all the features provided by OwlDB.

**Check the following items before installation.**

- The installation feature can only be used by an administrator account.

### Standard Architecture

OwlDB provides a new database installation feature based on standard architecture. The standard architectures provided by OwlDB On-premise are as follows.

| Configuration | Description |
| --- | --- |
| Single | Single-node configuration |
| TAC (Tibero) | Up to 4-node cluster configuration |
| Single + DR (Tibero) | 1 Standby node fixed for Single configuration |
| TAC + DR (Tibero) | 1 Standby node fixed for TAC configuration |
| HA (OpenSQL) | 1 Replica node fixed for HA configuration |

For TAC and DR configurations, all nodes are configured with the same spec, and in a DR configuration the Standby node is fixed to 1.

{% hint style="info" %}
**Note**

**Note**

A Multi-node Cluster cannot be configured as Standby, and up to 1 Standby node is supported.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you change the database configuration directly from outside without going through OwlDB, some features of OwlDB may not function properly. If a configuration change is needed, be sure to perform it through OwlDB.
{% endhint %}

---

### **Installation Process**

1. **OwlDB console screen** > **Dashboard**to navigate to.
2. At the top of the dashboard, **Install** Press the button to move to the installation page.
3. Enter the installation options step by step. Installation option entry consists of a total of 5 steps, and for details on each step please refer to the [**Installation Option Steps** ](#undefined-2)section below.
4. Once you check the entered information and validation of installation feasibility is complete, **Install** Click the button.
5. Once installation starts, you can check the progress status in the dashboard list. When the status changes to **Running**, the installation has been completed successfully.

{% hint style="info" %}
**Note**

The database installation feature can only be used from the Root account.

You can also access the database installation page through the following paths.

- OwlDB console screen > Dashboard > Card view > + icon
- GNB > DB Alias dropdown > Database Install button

Installation in Progress **Cancel** Clicking the button displays a confirmation modal, and clicking confirm in the modal resets the entered information and moves to the dashboard.

Once an installation request is received, you can check the installation start, completion, and failure status through system notifications.
{% endhint %}

{% hint style="warning" %}
**Caution**

**Install** Clicking the button also runs validation of the license file presence and core count.

- If there is no license file on the target host, installation will not proceed, and you must place the license file and try again.
- If the requested core count exceeds the maximum core count of the held license, installation will not proceed.
- If processing is delayed because many installation requests are received simultaneously, you must try again after a while.
{% endhint %}

---

### **Installation Option Steps**

**Engine Options**

This step configures the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name to identify the DB Service<ul><li>Only 6–30 characters of uppercase/lowercase English letters, numbers, and hyphens (-) can be entered</li><li>Cannot be created as a duplicate within an OwlDB account</li><li>Default value:<code>owldb-001</code>Assigned sequentially starting from</li></ul></td></tr><tr><td>DB Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a redundancy configuration</li><li><strong>OpenSQL</strong> : Open Source-based DBMS</li></ul></td></tr><tr><td>Topology*</td><td><ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count*</td><td><ul><li><strong>Tibero</strong>: Single (1, fixed), TAC (select from 2–4)</li><li><strong>OpenSQL</strong> : Single, HA (1, fixed)</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to use in OpenSQL (not applicable to Tibero)<ul><li>Default value:<strong>17.9</strong></li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

---

**DR Configuration**

This step configures whether to use DR and the failover automation level.

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration (direct selection available)</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover stage</a><ul><li>Stage 0: Manual</li><li>Stage 1: Automatic failover</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Stage 3: Full automation</li><li>Single: Supports levels 0, 1, 3</li><li>TAC: Supports levels 0, 1</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Standby DBs (fixed to a maximum of 1 based on standard architecture)</td></tr><tr><td>Standby Mode*</td><td>Standby Mode Options<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary to Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Single is automatically determined as DR not used, and HA as DR used, and cannot be modified</td></tr><tr><td>Failover Automation Level*</td><td><a href="#undefined">Automatic failover stage</a><ul><li>Stage 0: Manual</li><li>Level 2: Automatic configuration recovery (not supported in OwlDB v1.3)</li><li>Stage 3: Full automation</li><li>Supports levels 0 and 3 (default: level 3)</li></ul></td></tr><tr><td>Standby Count*</td><td>Number of Replica DBs (fixed to a maximum of 1 based on standard architecture)</td></tr><tr><td>Log Replication Type</td><td>Fixed to ASYNC method and cannot be modified</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- When DR use is selected in the Enable DR item **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** items are displayed.
- When using ARCH ASYNC mode, synchronization to Standby may be delayed depending on the cycle at which archive logs are generated (up to approximately 10 minutes).
{% endhint %}

{% hint style="warning" %}
**Caution**

If you set the Failover Automation Level to level 3 (full automation), all processes from recovery to resource cleanup after failover are handled automatically. Since recovery speed is prioritized above all, some recent data as of the time of failure may be lost.
{% endhint %}

---

**Instance Configuration**

The input items for each node are automatically configured according to the topology. The * notation indicates a required input item.

{% tabs %}
{% tab title="Tibero" %}
The node role is displayed as **Primary/Standby**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding<br>Enter the external Port |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| Redo Path* | Enter Redo Path | Only file system paths can be entered |
| Archive Path* | Enter Archive Path | Only file system paths can be entered |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Each Path**can only be entered as a file system path. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | When using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | When using port forwarding, enter the external Port |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| Redo Path* | Enter Redo Path | Only file system paths can be entered |
| Archive Path* | Enter Archive Path | Only file system paths can be entered |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**All Paths**can only be entered as a file system path. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to use for communication between nodes within the Cluster from the list | Enter for both Primary and Standby clusters |
| Data Path* | Enter Data Path | Enter the raw device or partition path, use a shared volume |
| Redo Path* | Enter Redo Path | Enter the raw device or partition path, use a shared volume |
| Archive Path* | Enter Archive Path | Enter the raw device or partition path, use a shared volume |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Data Path, Redo Path, Archive Path**enter the raw device path (`/dev/sdb`) or the partition path (`/dev/sdb1`). **Backup Path**Only file system paths can be entered in this case.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Interconnect IP* | Select the Interconnect IP to use for communication between nodes within the Cluster from the list | Enter for both Primary and Standby clusters |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | When using port forwarding, enter the external Port |
| Data Path* | Enter Data Path | Enter the raw device or partition path, use a shared volume |
| Redo Path* | Enter Redo Path | Enter the raw device or partition path, use a shared volume |
| Archive Path* | Enter Archive Path | Enter the raw device or partition path, use a shared volume |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**Data Path, Redo Path, Archive Path**enter the raw device path (`/dev/sdb`) or the partition path (`/dev/sdb1`). **Backup Path**Only file system paths can be entered in this case.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

{% hint style="info" %}
**Note**

- The above input items are automatically configured for as many nodes as required by the selected topology.
- For a stable operating environment, all instances within the cluster are automatically configured with identical specifications.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
The node role is displayed as **Leader/Replica**.

**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding<br>Enter the external Port |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Each Path**can only be entered as a file system path. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**HA**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | In accordance with the selected topology configuration,<br>select the host on which to install the database | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Replication Connection IP* | Enter or select the IP to use for the replication connection | - |
| Network interface* | Select the network interface | - |
| Data Path* | Enter Data Path | Only file system paths can be entered |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | Enter the private key path to use for SSH connections between instances | Enter a common private key path so that the same private key is used for SSH connections between instances |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

When using a DR configuration, the SSH Key File must be placed in the specified path in advance. For detailed configuration instructions, see [SSH public key configuration between nodes](#4VCx1BdX0fpROq6CwGnH).
{% endhint %}

**All Paths**can only be entered as a file system path. Entering the same path or different paths redundantly is also allowed.

However, the following conditions must be met.

- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.
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
| SYS User Password* | The password of the database super-privileged administrator account (SYS) |
| Target Memory Size* | Target memory size |
| Shared Memory Size* | Shared memory size |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS time zone where the database will be installed |
| VIP* | Select whether to use VIP |
| Primary Node #N Vip | Database virtual IP<br>(enabled when VIP use is selected) |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | Size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | Size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Database Name*</td><td>The name of the database to be used</td></tr><tr><td>User Id*</td><td>Database top-privilege administrator account ID</td></tr><tr><td>User Password*</td><td>The password of the database super-privileged administrator account</td></tr><tr><td>Character Set*</td><td>The character encoding to be used for the database</td></tr><tr><td>Timezone*</td><td>The OS time zone where the database will be installed</td></tr><tr><td>VIP*</td><td>Database virtual IP</td></tr><tr><td>Database Listener Port</td><td>The database listener port for network communication</td></tr><tr><td>Max Session Count</td><td>Maximum number of concurrently allowed sessions</td></tr><tr><td>Shared Buffers</td><td>Shared memory size (cannot be modified)</td></tr><tr><td>WAL File Size (MB)</td><td>WAL file size<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified</td></tr><tr><td>Connection Pooler Port</td><td>The port on which the connection pool receives client connections in OpenSQL<ul><li>Default: 6432</li><li>Input range: 1024–65535</li></ul></td></tr><tr><td>Extension</td><td>Select Extensions to install together when creating the OpenSQL database (multiple selection available)</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

- The Connection Pooler Port and Extension items are displayed only when the OpenSQL engine is selected.
- Even if Extension installation fails, it does not affect database creation, and the Extensions that failed to install can be checked in system notifications.
{% endhint %}
{% endtab %}
{% endtabs %}

---

**Configuration Information Review**

You can enter this step only after validation of all the options entered in the previous steps is complete.

Review the entered configuration information at a glance, and if there are no issues, **Install** Click the button to start the installation. To modify the content, **Previous** please click the button.

{% hint style="info" %}
**Note**

**4. Database Configuration** In the step, **Next** when you click the button, validation of the entered information is automatically performed, and only after passing the validation can you **5. Verify Configuration Information** enter the step.

**[Tibero]** Additionally check the following items.

- Whether the entered Data/Redo/Archive/Backup Path actually exists on the target installation host
- Whether SSH connection between nodes is possible
- Whether the OS Timezone of all nodes matches
- Whether the sum of the entered Data File sizes does not exceed the available storage space

**[OpenSQL]** Additionally check the following items.

- Whether the entered Data Path actually exists on the target installation host
- Whether SSH connection between nodes is possible
- Whether the SSH Key File Path actually exists and whether it is a Private key
- Whether the OS Timezone of all nodes matches
- Whether the sum of the entered Data File sizes does not exceed the available storage space

If the validation fails, an error message is displayed on the item where the error occurred, and you cannot move to the next step.
{% endhint %}

---

### Installation Progress Status

You can monitor the database installation progress in real time from the dashboard. The installation proceeds in major steps and detailed steps as shown in the table below, and the steps actually performed may vary depending on the selected topology.

{% tabs %}
{% tab title="Tibero" %}
| Major Step | Detailed Step |
| --- | --- |
| Installation Prerequisite Validation | 1. sudo privilege validation<br>2. Required file validation<br>3. parameter config validation |
| Infrastructure Setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. Disk udev rule configuration<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero Configuration | 1. Create Tibero instance Tip file & DSN file<br>2. Volume configuration change due to cm sequential installation<br>3. Volume wait due to cm sequential installation<br>4. Tip convert (primary ↔ standby)<br>5. Standby installation complete tag configuration change<br>6. Standby installation complete tag wait<br>7. RMGR backup and transfer<br>8. RMGR backup wait |
| DB Installation | 1. CM gen (resource registration and execution)<br>2. DB create (TAS also included, depending on topology)<br>3. Create Disk snapshot<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on (reboot)<br>6. cm complete (service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero Status Check | 1. Tb probe |
{% endtab %}
{% tab title="OpenSQL" %}
| Major Step | Detailed Step |
| --- | --- |
| Infrastructure Setup | 1. Kernel environment configuration<br>2. Required package installation<br>3. PgAgent installation<br>4. mount volume<br>5. Data directory preparation |
| OpenSQL Configuration | 1. Module configuration |
| OpenSQL Installation | 1. Post-bootstrap configuration |
| OpenSQL Status Check | 1. Apply configuration after initialization |
| PGAgent Installation | 1. PgAgent configuration |
{% endtab %}
{% endtabs %}
