You can register and manage an already operating database in OwlDB. In addition to databases built on a standard architecture, registration is also supported for databases with a non-standard architecture that customers have configured themselves.

Once registration is complete, you can use features provided by OwlDB such as monitoring and parameter management.

**Check the following items before registration.**

- The registration feature is available only to administrator accounts.
- The boot mode of the DB node to be registered must be `Normal`, `Recovery`, `Readonly` . Nodes started in any other boot mode are treated as abnormal nodes.
- Standby multi-node configurations do not support registration.

## Registration Procedure

1. On the OwlDB console screen > Dashboard > **Discover** click the button.
2. In the discovery results, **Registrable DB** check the list, and for the relevant database **Register** click the button to move to the registration page.
3. Enter the registration options step by step. Entering registration options consists of a total of 5 steps, and for detailed information on each step, please refer to the '**Registration Options**' below.
4. Check the entered information and **Register** click the button.
5. When the status of the relevant database in the dashboard list is displayed as **Running**registration has been completed successfully.

**Register** When you click the button, the license conditions are verified, then the registration request begins and you are moved to the dashboard. You can check whether registration has started, completed, or failed through system notifications.

![List of registrable databases and the register button on the discovery results screen](Discovery results list and register button screen)

{% hint style="warning" %}
**Caution**

Registration may fail during the registration process in the following cases.

- When the connection to the target database fails
- When the database status changes during the registration process
- When the connection to the Agent fails
- When there is no license file on the target node or the number of license cores is insufficient

If registration fails, an error message is displayed, and processing ends without moving to the dashboard.
{% endhint %}

---

## Registration Options

When you move to the registration page, the database information collected through discovery is automatically entered. Automatically entered items cannot be changed, and only some items that cannot be collected need to be entered.

**Engine Options**

This is the step for setting the database name, engine, and topology information.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>A name to identify the database service<ul><li>Only 6–30 characters of English uppercase and lowercase letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used</li><li>If not entered<code>owldb-001</code>automatically generated in a format such as</li></ul></td></tr><tr><td>Database Engine Type</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a redundancy configuration</li><li><strong>OpenSQL</strong> : An Open Source-based customized DBMS technology platform</li></ul></td></tr><tr><td>Topology</td><td>The topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count</td><td>The number of nodes making up the cluster<ul><li><strong>Tibero</strong>Single : 1</li><li>TAC : 2~8<strong>OpenSQL</strong></li><li>Single : 1</li><li>HA : 2~3</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>The PostgreSQL version shown when OpenSQL is selected</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

OpenSQL HA can be discovered as either 2-node or 3-node.

- 2-node HA: Configured with 1 Leader + 1 Replica + 1 Quorum Node (etcd)
- 3-node HA: Configured with 1 Leader + 2 Replicas
{% endhint %}

**DR Configuration**

This is the step for setting whether to use DR and the failover automation level.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Enable DR</td><td>Whether to use DR configuration</td><td>-</td></tr><tr><td>Failover Automation Level*</td><td>Failover Automation Level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic Failover</strong></li><li><strong>Level 2: Automatic Configuration Recovery (Not supported On-Premise)</strong></li><li><strong>Level 3: Fully Automated</strong></li></ul></td><td><ul><li>Tibero Single: Supports Levels 0, 1, 3</li><li>Tibero TAC: Supports Levels 0, 1</li><li>OpenSQL: Supports Levels 0, 3</li></ul></td></tr><tr><td>{Standby/Replica} Count</td><td>Number of Standby/Replica DBs</td><td>-</td></tr><tr><td>Standby Mode</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td><td>Entered only for Tibero</td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary (Leader) to Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>(Tibero): A replication mode that transmits Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>(Tibero): A replication mode that collects and transmits archive log files after a log switch, once the files are generated</li><li><strong>ASYNC</strong>(OpenSQL): A replication mode that transmits data asynchronously through a replication connection</li></ul></td><td>-</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * symbol indicates a required field.

{% hint style="info" %}
**Note**

- When DR usage is selected in the Enable DR item **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** items are displayed.
- When OpenSQL is discovered as an HA configuration, the Enable DR item is automatically displayed as **Use DR**and cannot be changed. To change the Topology, please go back to the previous step.
{% endhint %}

**Instance Configuration**

Input fields for each node are automatically configured according to the topology. Each node area can be expanded or collapsed in an accordion style. From the second node onward, **Same as previous** selecting the checkbox applies the Port and Backup Path values entered for the previous node as they are.

{% tabs %}
{% tab title="Tibero" %}
**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * symbol indicates a required field.

**Path**can only be entered as a file system path.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host to be registered.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Entered only for the Primary instance) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | When using NAT, enter the NAT IP<br>Enter for the Primary instance only |
| Primary Destination Port*<br>(Enter for the Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for the StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | When using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for the StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

**Path**Only a file system path can be entered.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host to be registered.
- For the file system path, the DP Agent execution account must **read, write, and execute permissions**hold.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered,<br>using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

**Backup Path**In this case, only a file system path can be entered.

However, the following conditions must be met.

- All entered paths must **a shared volume**be configured as.
- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must **read, write, and execute permissions**hold.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Primary Destination IP* (Enter for the Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | When using NAT, enter the NAT IP |
| Primary Destination Port*<br>(Enter for the Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for the StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | When using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for the StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | File system paths only, uses shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Backup Path**Only file system paths can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must **actually exist**on the installation target host.
- For the file system path, the DP Agent execution account must have **read, write, and execute permissions**.
{% endtab %}
{% tab title="OpenSQL" %}
The node section names are displayed as **Leader Node**, **Replica Node #{n}**, and when detected as 2node HA, **Quorum Node**is displayed together.

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | When using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server<br>Default:`5432` | When using port forwarding, enter the external Port |
| Replication Connection IP* | In an HA configuration, directly enter or select the IP to be used for the replication connection | Displayed only in HA configuration |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% endtabs %}

**Database Configuration**

This is the step to review the database configuration information and directly enter some items that cannot be collected. Most items are automatically filled in with the discovery results and cannot be modified.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password* | The password of the database's highest-privilege administrator account (SYS) |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS timezone where the database will be installed |
| VIP | Database virtual IP<br>When VIP is not used`-`displayed as |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (cannot be modified) |
| Target Memory Size | Target memory size (cannot be modified) |
| Shared Memory Size | Shared memory size (cannot be modified) |
| Redo Log File Size (MB) | Redo log file size<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |
| System Data File Size (MB) | The size of the data file for storing system tables and key metadata<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |
| Syssub Data File Size (MB) | The size of the sub data file for storing system operation-related data<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |
| User Tablespace Data File Size (MB) | The size of the tablespace data file for storing user data<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |
| Temporary Tablespace Data File Size (MB) | The size of the temporary tablespace data file used for large-scale operations<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |
| Undo Tablespace Data File Size (MB) | Undo tablespace size<br>The value cannot be confirmed during the discovery process, so it is displayed as an empty value and cannot be modified |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| User Id |   |
| User Password* | Password of the database's top-level privileged administrator account |
| Character Set | Character encoding to be used by the database |
| Timezone | OS timezone where the database will be installed |
| VIP | Database virtual IP<br>When VIP is not used`-`Displayed as |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (not editable) |
| Target Memory Size | Target memory size (not editable) |
| Shared Buffers | Shared memory size (not editable) |
| WAL File Size (MB) | WAL file size<br>The value cannot be determined during discovery, so it is displayed as empty and cannot be edited |
| Connection Pooler Port* | Port on which the connection pool listens for client connections<br>Range: 1024~65535 |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% endtabs %}

**Verify Configuration Information**

You can enter this step only after validation confirms that all options entered in the previous steps were entered correctly.

Review the entered configuration information at a glance, and if there are no issues, **Register** click the button to begin registration. To modify the content, **Previous** please click the button.

In the summary area on the right side of the screen, you can expand or collapse the information entered at each step to review it, and required fields left blank are displayed as `[미입력]`.

****The mark indicates a required input field. For other items, information collected through discovery is automatically entered.***

{% hint style="warning" %}
**Caution**

In the instance configuration and database configuration steps, **Next** When you click the button, validation is performed on the items below, and if it fails, registration cannot proceed.

- When the OS Timezone differs between nodes (Tibero): You must connect to each node and unify the Timezone settings.
- When SYS account login fails (Tibero): You must check the entered password.
- When the entered Backup Path does not exist on the host
{% endhint %}

---

## Deregister

Registered databases can be excluded from OwlDB management targets through deregistration rather than deletion. Even after deregistration, the actual database remains intact in the operating environment and can be re-registered if needed.

Deregistration can be performed from the following paths.

- **Overview > Actions > Deregister** Click
- **Dashboard > Select DB Service > Deregister** Click

**Deregister** When you click the button, a confirmation modal is displayed, and after entering the DB Service Name, **Confirm** when you click the button, deregistration is completed.
