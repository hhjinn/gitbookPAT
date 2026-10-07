You can register and manage already operating databases in OwlDB. Registration is supported not only for databases built on the standard architecture, but also for databases with non-standard architectures configured directly by the customer.

Once registration is complete, you can use features provided by OwlDB such as monitoring and parameter management.

**Check the following items before registration.**

- The registration feature can only be used by an administrator account.
- The boot mode of the DB node to be registered must be `Normal`, `Recovery`, `Readonly` . Nodes started in any other boot mode are handled as abnormal nodes.
- Standby multi-node configurations do not support registration.

## Registration procedure <a href="#registration-procedure" id="registration-procedure"></a>

1. OwlDB console screen > Dashboard > **Explore** Click the button.
2. In the discovery results, **Registrable DBs** Check the list and, for the relevant database, **registered** Click the button to move to the registration page.
3. Enter the registration options step by step. Entering registration options consists of a total of 5 steps, and for details on each step, please refer to '**Registration options**' below.
4. Check the entered information and **registered** Click the button.
5. When the status of the relevant database in the dashboard list is displayed as **Running**, registration has been completed successfully.

**registered** When you click the button, the license conditions are verified, then the registration request begins and you are moved to the dashboard. Registration start, completion, and failure can be checked through system notifications.

![List of registrable databases and the register button on the discovery results screen](discovery results list and register button screen)

{% hint style="warning" %}
**Caution**

Registration may fail during the registration process in the following cases.

- When the connection to the target database fails
- When the database status changes during the registration process
- When the connection to the Agent fails
- When there is no license file on the target node or the number of license cores is insufficient

If registration fails, an error message is displayed and the process ends without moving to the dashboard.
{% endhint %}

---

## Registration options <a href="#registration-options" id="registration-options"></a>

When you move to the registration page, the database information collected through discovery is automatically entered. Automatically entered items cannot be changed, and only some items that cannot be collected need to be entered.

**Engine options**

This is the step for configuring the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name for identifying the database service<ul><li>Only 6-30 characters of uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used</li><li>If not entered<code>owldb-001</code>Automatically generated in a form such as</li></ul></td></tr><tr><td>Database Engine Type</td><td>Database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a multiplexed configuration</li><li><strong>OpenSQL</strong> : An Open Source-based, customer-customized DBMS technology platform</li></ul></td></tr><tr><td>Topology</td><td>Topology type that will determine the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count</td><td>Number of cluster configuration nodes<ul><li><strong>Tibero</strong>Single : 1</li><li>TAC : 2~8<strong>OpenSQL</strong></li><li>Single : 1</li><li>HA : 2~3</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version exposed when OpenSQL is selected</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

The HA of OpenSQL can be discovered as 2node or 3node.

- 2node HA: Configured with 1 Leader + 1 Replica + 1 Quorum Node (etcd)
- 3node HA: Configured with 1 Leader + 2 Replicas
{% endhint %}

**DR configuration**

This is the step for setting whether to use DR and the failover automation level.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Enable DR</td><td>Whether to use the DR configuration</td><td>-</td></tr><tr><td>Failover Automation Level*</td><td>Automatic failover level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery (not supported On-Premise)</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td><td><ul><li>Tibero Single: Supports levels 0, 1, 3</li><li>Tibero TAC: Supports levels 0, 1</li><li>OpenSQL: Supports levels 0, 3</li></ul></td></tr><tr><td>{Standby/Replica} Count</td><td>Number of Standby/Replica DBs</td><td>-</td></tr><tr><td>Standby Mode</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td><td>Entered only in Tibero</td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary (Leader) to Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>(Tibero): A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>(Tibero): A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li><li><strong>ASYNC</strong>(OpenSQL): A replication mode that transmits data asynchronously through a replication connection</li></ul></td><td>-</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

- When DR use is selected in the Enable DR item **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** item is exposed.
- When OpenSQL is discovered as an HA configuration, the Enable DR item is automatically **Use DR**It is displayed as and cannot be changed. To change the Topology, please go to the previous step.
{% endhint %}

**Instance Configuration**

Input items for each node are automatically configured according to the topology. Each node area can be expanded or collapsed in accordion form. From the second node onward, **Same as previous** Selecting the checkbox allows you to apply the Port and Backup Path values entered for the previous node as they are.

{% tabs %}
{% tab title="Tibero" %}
**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

**Path**Only a file system path can be entered for.

However, the following conditions must be met.

- The entered path must **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandBy DB to the Primary DB | If using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter Backup Path | Only a file system path can be entered |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

**Path**Only a file system path can be entered for.

However, the following conditions must be met.

- The entered path must **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Backup Path* | Enter Backup Path | Only a file system path can be entered,<br>using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

**Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandBy DB to the Primary DB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter Backup Path | Only a file system path can be entered,<br>using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

**Backup Path**In the case of, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path must, on the installation target host, **actually exist**on the registration target host.
- For the file system path, the DP Agent execution account must hold **read, write, and execute permissions**.
{% endtab %}
{% tab title="OpenSQL" %}
The node section name is displayed as **Leader Node**, **Replica Node #{n}**, and if detected as 2node HA **Quorum Node**is displayed together.

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server<br>Default:`5432` | If using port forwarding, enter the external Port |
| Replication Connection IP* | In an HA configuration, directly enter or select the IP to be used for the replication connection | Exposed only in an HA configuration |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

**Database Configuration**

This is the step to check the database configuration information and directly enter some items that cannot be collected. Most items are automatically filled in with the detection results and cannot be modified.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password* | The password of the database highest-privilege administrator account (SYS) |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>If VIP is not used`-`Displayed as |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Memory Size | Shared memory size (not modifiable) |
| Redo Log File Size (MB) | Redo log file size<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| System Data File Size (MB) | Size of the data file for storing system tables and key metadata<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| User Tablespace Data File Size (MB) | Size of the tablespace data file for storing user data<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| Undo Tablespace Data File Size (MB) | Undo tablespace size<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| User Id |   |
| User Password* | The password of the database highest-privilege administrator account |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>If VIP is not used`-`Displayed as |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Buffers | Shared memory size (not modifiable) |
| WAL File Size (MB) | WAL file size<br>The value could not be confirmed during the detection process, so it is displayed as an empty value and cannot be modified |
| Connection Pooler Port* | The port on which the connection pool receives client connections<br>Range: 1024~65535 |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

**Configuration Information Confirmation**

You can enter this step only after verification that all options entered in the preceding steps were entered correctly is complete.

Check the entered configuration information at a glance, and if there are no issues, **registered** Click the button to start registration. To modify the content, **Previous** Please click the button.

In the summary area on the right side of the screen, you can check the information entered at each step by expanding or collapsing it, and required items for which no value has been entered are `[Not entered]`displayed as.

****The marking indicates a required input item. Other items are automatically filled in with information collected through detection.***

{% hint style="warning" %}
**Caution**

In the Instance Configuration and Database Configuration steps, **Next** When you click the button, validation is performed for the items below, and if it fails, registration cannot proceed.

- When the OS Timezone differs between nodes (Tibero): You must connect to each node and unify the Timezone settings.
- When login to the SYS account fails (Tibero): You must check the entered password.
- When the entered Backup Path does not exist on the host
{% endhint %}

---

## Unregister <a href="#unregister" id="unregister"></a>

A registered database can be excluded from OwlDB management by unregistering it rather than deleting it. Even after unregistering, the actual database remains intact in the production environment and can be re-registered if needed.

Unregistration can be performed from the following paths.

- **Overview > Actions > Unregister** Click
- **Dashboard > Select DB Service > Unregister** Click

**Unregister** When you click the button, a confirmation modal appears. Enter the DB Service Name and **Confirm** When you click the button, unregistration is completed.
