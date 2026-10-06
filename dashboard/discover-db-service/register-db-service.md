Databases already in operation can be registered and managed in OwlDB. In addition to databases built on the standard architecture, registration is also supported for databases with non-standard architectures configured directly by the customer.

Once registration is complete, you can use the monitoring, parameter management, and other features provided by OwlDB.

**Check the following before registration.**

- The registration feature can only be used by administrator accounts.
- The boot mode of the DB node to be registered must be `Normal`, `Recovery`, `Readonly` . Nodes started in any other boot mode are treated as abnormal nodes.
- Standby multi-node configurations do not support registration.

## Registration procedure <a href="#registration-procedure" id="registration-procedure"></a>

1. OwlDB console screen > Dashboard > **Explore** Click the button.
2. In the discovery results, **Registrable DB** check the list, and for the corresponding database, **Registered** click the button to move to the registration page.
3. Enter the registration options step by step. Registration option entry consists of a total of 5 steps, and for details on each step, please refer to the '**Registration options**' below.
4. Check the entered information and **Registered** Click the button.
5. When the status of the corresponding database in the dashboard list is displayed as **Running**, registration has been completed successfully.

**Registered** When you click the button, the registration request begins after the license conditions are verified, and you are taken to the dashboard. The start, completion, and failure of registration can be confirmed via system notifications.

![List of registrable databases and the registration button on the discovery results screen](Discovery results list and registration button screen)

{% hint style="warning" %}
**Caution**

Registration may fail during the registration process in the following cases.

- When the connection to the target database fails
- When the database status changes during the registration process
- When the connection to the Agent fails
- When the license file is missing on the target node or the number of license cores is insufficient

If registration fails, an error message is displayed, and processing ends without moving to the dashboard.
{% endhint %}

---

## Registration options <a href="#registration-options" id="registration-options"></a>

When you move to the registration page, the database information collected through discovery is automatically entered. Automatically entered items cannot be changed, and only some items that cannot be collected need to be entered.

**Engine options**

This is the step for setting the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>A name to identify the database service<ul><li>Only 6 to 30 characters of uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used</li><li>If not entered<code>owldb-001</code>Automatically generated in a form such as</li></ul></td></tr><tr><td>Database Engine Type</td><td>The database engine to be used<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB scaling through a multiplexed configuration</li><li><strong>OpenSQL</strong> : An Open Source-based, customer-customized DBMS technology platform</li></ul></td></tr><tr><td>Topology</td><td>The topology type that will determine the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count</td><td>Number of nodes in the cluster configuration<ul><li><strong>Tibero</strong>Single : 1</li><li>TAC : 2~8<strong>OpenSQL</strong></li><li>Single : 1</li><li>HA : 2~3</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>The PostgreSQL version exposed when OpenSQL is selected</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

OpenSQL HA can be discovered as 2node or 3node.

- 2node HA: Configured with 1 Leader + 1 Replica + 1 Quorum Node (etcd)
- 3node HA: Configured with 1 Leader + 2 Replicas
{% endhint %}

**DR configuration**

This is the step for setting whether to use DR and the failover automation level.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Enable DR</td><td>Whether to use DR configuration</td><td>-</td></tr><tr><td>Failover Automation Level*</td><td>Automatic failover level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery (Not supported On-Premise)</strong></li><li><strong>Level 3: Fully automated</strong></li></ul></td><td><ul><li>Tibero Single: Levels 0, 1, 3 supported</li><li>Tibero TAC: Levels 0, 1 supported</li><li>OpenSQL: Levels 0, 3 supported</li></ul></td></tr><tr><td>{Standby/Replica} Count</td><td>Number of Standby/Replica DBs</td><td>-</td></tr><tr><td>Standby Mode</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td><td>Entered only for Tibero</td></tr><tr><td>Log Replication Type</td><td>The log transmission method from Primary (Leader) to Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>(Tibero): A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>(Tibero): A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li><li><strong>ASYNC</strong>(OpenSQL): A replication mode that transmits data asynchronously through a replication connection</li></ul></td><td>-</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

- When DR usage is selected in the Enable DR item **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** item is exposed.
- When OpenSQL is discovered in an HA configuration, the Enable DR item is automatically **Use DR**It is displayed as read-only and cannot be changed. To change the Topology, please go back to the previous step.
{% endhint %}

**Instance Configuration**

Input fields for each node are automatically configured according to the topology. Each node section can be expanded or collapsed in an accordion format. From the second node onward, **Same as previous** By selecting the checkbox, you can apply the Port and Backup Path values entered for the previous node as-is.

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

The * notation indicates a required input field.

**Path**Only a file system path can be entered.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(Enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Path**Only a file system path can be entered.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered,<br>Using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Backup Path**In this case, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **a shared volume**be configured as.
- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from StandByDB to PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(Enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from StandBy DB to Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from Primary DB to StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from Primary DB to StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered,<br>Using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

**Backup Path**In this case, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **a shared volume**be configured as.
- The entered path must be on the installation target host **actually exist**on the target host for registration.
- The DP Agent execution account must have **read, write, and execute permissions**for the file system path.
{% endtab %}
{% tab title="OpenSQL" %}
The node section name is displayed as **Leader Node**, **Replica Node #{n}**, and when detected as a 2node HA, **Quorum Node**is displayed together.

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server<br>Default value:`5432` | If using port forwarding, enter the external Port |
| Replication Connection IP* | In an HA configuration, directly enter or select the IP to be used for the replication connection | Exposed only in an HA configuration |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% endtabs %}

**Database Configuration**

This step allows you to review the database configuration information and directly enter some items that cannot be collected. Most items are automatically filled in with the detection results and cannot be modified.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password* | The password of the database's highest-privilege administrator account (SYS) |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database Virtual IP<br>When VIP is not used`-`Displayed as |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrent sessions allowed (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Memory Size | Shared memory size (not modifiable) |
| Redo Log File Size (MB) | Redo log file size<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| System Data File Size (MB) | Size of the data file for storing system tables and key metadata<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| User Tablespace Data File Size (MB) | Size of the tablespace data file for storing user data<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| Undo Tablespace Data File Size (MB) | Undo tablespace size<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| User Id |   |
| User Password* | The password of the database's highest-privilege administrator account |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database Virtual IP<br>When VIP is not used`-`Displayed as |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrent sessions allowed (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Buffers | Shared memory size (not modifiable) |
| WAL File Size (MB) | WAL file size<br>Displayed as an empty value and cannot be modified because the value could not be determined during the detection process |
| Connection Pooler Port* | The port on which the connection pool receives client connections<br>Range: 1024~65535 |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.
{% endtab %}
{% endtabs %}

**Configuration Information Review**

You can enter this step only after it has been verified that all options entered in the previous steps were entered correctly.

Review the entered configuration information at a glance, and if there are no issues, **Registered** Click the button to start registration. To modify the content, **Previous** Please click the button.

In the summary area on the right side of the screen, you can expand or collapse the information entered at each step to review it, and required items for which no value was entered are `[Not Entered]`displayed as.

****The marking indicates a required input item. Other items are automatically filled in with information collected through detection.***

{% hint style="warning" %}
**Caution**

In the Instance Configuration and Database Configuration steps, **Next** When you click the button, validation is performed on the items below, and if it fails, registration cannot proceed.

- When the OS Timezone differs between nodes (Tibero): You must connect to each node and unify the Timezone settings.
- When login to the SYS account fails (Tibero): You must check the entered password.
- When the entered Backup Path does not exist on the host
{% endhint %}

---

## Deregistration <a href="#unregister" id="unregister"></a>

A registered database can be excluded from OwlDB management through deregistration rather than deletion. Even after deregistration, the actual database remains in the operational environment and can be re-registered if needed.

Deregistration can be performed from the following path.

- **Overview > Actions > Deregister** Click
- **Dashboard > Select DB Service > Deregister** Click

**Deregistration** When you click the button, a confirmation modal is displayed. Enter the DB Service Name and **Confirm** click the button to complete the deregistration.
