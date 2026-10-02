You can register and manage databases that are already in operation in OwlDB. In addition to databases built on the standard architecture, registration is also supported for databases with non-standard architectures configured directly by the customer.

Once registration is complete, you can use features provided by OwlDB such as monitoring and parameter management.

**Check the following items before registration.**

- The registration feature can only be used by administrator accounts.
- The boot mode of the DB node to be registered must be `Normal`, `Recovery`, `Readonly` . Nodes started in any other boot mode are treated as abnormal nodes.
- Standby multi-node configurations are not supported for registration.

## Registration Procedure

1. OwlDB console screen > Dashboard > **Explore** Click the button.
2. In the discovery results, **Registrable DB** Check the list and, for the corresponding database, **Registered** Click the button to move to the registration page.
3. Enter the registration options step by step. Entering the registration options consists of a total of 5 steps, and for details on each step, please refer to '**Registration Options**' below.
4. Review the entered information and **Registered** Click the button.
5. When the status of the corresponding database in the dashboard list is displayed as **Running**, the registration has been completed successfully.

**Registered** When you click the button, the license conditions are verified and then the registration request begins, and you are moved to the dashboard. The start, completion, and failure of registration can be checked through system notifications.

![List of registrable databases and the registration button on the discovery results screen](Discovery results list and registration button screen)

{% hint style="warning" %}
**Caution**

During registration, it may fail in the following cases.

- When the connection to the target database fails
- When the database status changes during registration
- When the connection to the Agent fails
- When the target node has no license file or has an insufficient number of license cores

If registration fails, an error message is displayed, and processing ends without moving to the dashboard.
{% endhint %}

---

## Registration Options

When you move to the registration page, the database information collected through discovery is automatically entered. Automatically entered items cannot be changed, and only some items that cannot be collected need to be entered.

**Engine Options**

This step configures the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>A name to identify the database service<ul><li>Only 6–30 characters of uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used</li><li>If not entered<code>owldb-001</code>It is automatically generated in a form such as</li></ul></td></tr><tr><td>Database Engine Type</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB expansion through a redundancy configuration</li><li><strong>OpenSQL</strong> : An Open Source-based, customer-customized DBMS technology platform</li></ul></td></tr><tr><td>Topology</td><td>The topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count</td><td>Number of nodes in the cluster configuration<ul><li><strong>Tibero</strong>Single : 1</li><li>TAC : 2~8<strong>OpenSQL</strong></li><li>Single : 1</li><li>HA : 2~3</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>The PostgreSQL version exposed when OpenSQL is selected</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

The HA of OpenSQL can be discovered as 2node or 3node.

- 2node HA: Composed of 1 Leader + 1 Replica + 1 Quorum Node (etcd)
- 3node HA: Composed of 1 Leader + 2 Replicas
{% endhint %}

**DR Configuration**

This step configures whether to use DR and the failover automation level.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Enable DR</td><td>Whether to use the DR configuration</td><td>-</td></tr><tr><td>Failover Automation Level*</td><td>Automatic failover stage<ul><li><strong>Stage 0: Manual</strong></li><li><strong>Stage 1: Automatic failover</strong></li><li><strong>Stage 2: Automatic configuration recovery (not supported On-Premise)</strong></li><li><strong>Stage 3: Full automation</strong></li></ul></td><td><ul><li>Tibero Single: Supports stages 0, 1, 3</li><li>Tibero TAC: Supports stages 0, 1</li><li>OpenSQL: Supports stages 0, 3</li></ul></td></tr><tr><td>{Standby/Replica} Count</td><td>Number of Standby/Replica DBs</td><td>-</td></tr><tr><td>Standby Mode</td><td>Standby Mode Options<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td><td>Entered only for Tibero</td></tr><tr><td>Log Replication Type</td><td>The log transmission method from Primary (Leader) to Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>(Tibero): A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>(Tibero): A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li><li><strong>ASYNC</strong>(OpenSQL): A replication mode that transmits data asynchronously through a replication connection</li></ul></td><td>-</td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

- When DR use is selected in the Enable DR item **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** The item is exposed.
- When OpenSQL is discovered as an HA configuration, the Enable DR item is automatically **Use DR**and cannot be changed. To change the Topology, please go to the previous step.
{% endhint %}

**Instance Configuration**

Input items for each node are automatically configured according to the topology. Each node area can be expanded or collapsed in an accordion form. From the second node onward, **Same as previous** If you select the checkbox, the Port and Backup Path values entered for the previous node can be applied as-is.

{% tabs %}
{% tab title="Tibero" %}
**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding<br>Enter the external Port |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Path**Only file system paths can be entered for this.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | When using NAT, enter the NAT IP<br>Enter for Primary instance only |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Path**Only file system paths can be entered for this.

However, the following conditions must be met.

- The entered path must **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered,<br>when using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Backup Path**Only file system paths can be entered in this case.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | When using port forwarding, enter the external Port |
| Primary Destination IP* (enter for Primary instance only) | Directly enter the Primary Destination IP to be used for communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(enter for Primary instance only) | Directly enter the Primary Destination Port to be used for communication from the StandBy DB to the Primary DB | When using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination IP to be used for communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(enter for StandBy instance only) | Directly enter the StandBy Destination Port to be used for communication from the Primary DB to the StandBy DB | When using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only file system paths can be entered,<br>when using a shared volume |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Backup Path**Only file system paths can be entered in this case.

However, the following conditions must be met.

- All entered Paths must **shared volume**be configured as.
- The entered path on the installation target host **actually exist**on the target host for registration.
- For the file system path, the account running the DP Agent **read, write, and execute permissions**must hold.
{% endtab %}
{% tab title="OpenSQL" %}
The node section name is **Leader Node**displayed as **Replica Node #{n}**, and when discovered as 2node HA **Quorum Node**is displayed together.

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server<br>Default:`5432` | When using port forwarding, enter the external Port |
| Replication Connection IP* | In an HA configuration, directly enter or select the IP to be used for the replication connection | Exposed only in HA configurations |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% endtabs %}

**Database Configuration**

This is the step for reviewing the database configuration information and directly entering some items that cannot be collected. Most items are automatically filled in with the discovery results and cannot be modified.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password* | The password of the database super-privileged administrator account (SYS) |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>When not using a VIP`-`displayed as |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | Maximum number of concurrent sessions allowed (cannot be modified) |
| Target Memory Size | Target memory size (cannot be modified) |
| Shared Memory Size | Shared memory size (cannot be modified) |
| Redo Log File Size (MB) | Redo log file size<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| System Data File Size (MB) | The size of the data file for storing system tables and key metadata<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| Syssub Data File Size (MB) | The size of the sub data file for storing system operation-related data<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| User Tablespace Data File Size (MB) | The size of the tablespace data file for storing user data<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| Temporary Tablespace Data File Size (MB) | The size of the temporary tablespace data file used for large-scale operations<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| Undo Tablespace Data File Size (MB) | Undo tablespace size<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| User Id |   |
| User Password* | The password of the database super-privileged administrator account |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>When not using a VIP`-`displayed as |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | Maximum number of concurrent sessions allowed (cannot be modified) |
| Target Memory Size | Target memory size (cannot be modified) |
| Shared Buffers | Shared memory size (cannot be modified) |
| WAL File Size (MB) | WAL file size<br>The value could not be confirmed during discovery, so it is displayed as empty and cannot be modified |
| Connection Pooler Port* | The port on which the connection pool receives client connections<br>Range: 1024~65535 |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.
{% endtab %}
{% endtabs %}

**Configuration Information Review**

You can enter this step only after it has been verified that all options entered in the previous steps were entered correctly.

Review the entered configuration information at a glance, and if there are no issues, **Registered** click the button to begin registration. To modify the content, **Previous** please click the button.

In the summary area on the right side of the screen, you can expand or collapse to review the information entered at each step, and required items for which no value has been entered are `[미입력]`is displayed as.

****The mark indicates a required input item. Other items are automatically filled in with information collected through discovery.***

{% hint style="warning" %}
**Caution**

In the Instance Configuration and Database Configuration steps, **Next** When you click the button, validation is performed on the following items, and if it fails, registration cannot proceed.

- When the OS Timezone differs between nodes (Tibero): You must connect to each node and unify the Timezone settings.
- When login to the SYS account fails (Tibero): You must verify the entered password.
- When the entered Backup Path does not exist on the host
{% endhint %}

---

## Deregistration

A registered database can be excluded from OwlDB management by deregistration rather than deletion. Even after deregistration, the actual database remains intact in the operating environment, and can be re-registered if needed.

Deregistration can be performed from the following paths.

- **Overview > Actions > Deregister** Click
- **Dashboard > Select DB Service > Deregister** Click

**Deregistration** When you click the button, a confirmation modal is displayed, and after entering the DB Service Name and **Confirm** clicking the button, deregistration is completed.
