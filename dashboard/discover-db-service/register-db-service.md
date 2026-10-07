You can register and manage databases that are already in operation in OwlDB. Registration is supported not only for databases built on a standard architecture, but also for databases with a non-standard architecture configured directly by the customer.

Once registration is complete, you can use features provided by OwlDB such as monitoring and parameter management.

**Check the following items before registration.**

- The registration feature can only be used by administrator accounts.
- The boot mode of the DB node to be registered must be `Normal`, `Recovery`, or `Readonly`. Nodes started in any other boot mode are treated as abnormal nodes.
- Standby multi-node configurations are not supported for registration.

## Registration Procedure <a href="#registration-procedure" id="registration-procedure"></a>

1. Click the OwlDB console screen > Dashboard > **Discover** button.
2. In the discovery results, check the **Registrable DBs** list and click the **Register** button for the relevant database to move to the registration page.
3. Enter the registration options step by step. Entering the registration options consists of a total of 5 steps, and for details on each step, please refer to '**Registration Options**' below.
4. Review the entered information and click the **Register** button.
5. When the status of the relevant database is displayed as **Running** in the dashboard list, registration has been completed successfully.

When you click the **Register** button, the registration request starts after the license conditions are verified, and you are moved to the dashboard. The start, completion, and failure of registration can be checked through system notifications.

![List of registrable databases and the register button on the discovery results screen](discovery results list and register button screen)

{% hint style="warning" %}
**Caution**

Registration may fail during the registration process in the following cases.

- When the connection to the target database fails
- When the database status changes during the registration process
- When the connection to the Agent fails
- When the target node has no license file or the number of license cores is insufficient

If registration fails, an error message is displayed, and the process ends without moving to the dashboard.
{% endhint %}

---

## Registration Options <a href="#registration-options" id="registration-options"></a>

When you move to the registration page, the database information collected through discovery is automatically entered. Automatically entered items cannot be changed, and only some items that cannot be collected need to be entered.

**Engine Options**

This is the step for setting the database name, engine, and topology information.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Service Name*</td><td>Name for identifying the database service<ul><li>Only 6–30 characters of uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used</li><li>If not entered, it is automatically generated in a form such as <code>owldb-001</code></li></ul></td></tr><tr><td>Database Engine Type</td><td>The database engine to use<ul><li><strong>Tibero</strong>: An RDBMS that enables stable service operation and DB scaling through a redundancy configuration</li><li><strong>OpenSQL</strong>: An Open Source-based, customer-customized DBMS technology platform</li></ul></td></tr><tr><td>Topology</td><td>Topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong> : Single, HA</li></ul></td></tr><tr><td>Node Count</td><td>Number of cluster configuration nodes<ul><li><strong>Tibero</strong>Single : 1</li><li>TAC : 2~8<strong>OpenSQL</strong></li><li>Single : 1</li><li>HA : 2~3</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version exposed when OpenSQL is selected</td></tr></tbody></table>

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

OpenSQL HA can be discovered as 2node or 3node.

- 2node HA: Configured with 1 Leader + 1 Replica + 1 Quorum Node (etcd)
- 3node HA: Configured with 1 Leader + 2 Replicas
{% endhint %}

**DR Configuration**

This is the step for setting whether to use DR and the failover automation level.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Enable DR</td><td>Whether to use DR configuration</td><td>-</td></tr><tr><td>Failover Automation Level*</td><td>Automatic failover level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery (Not supported On-Premise)</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td><td><ul><li>Tibero Single: Supports levels 0, 1, 3</li><li>Tibero TAC: Supports levels 0, 1</li><li>OpenSQL: Supports levels 0, 3</li></ul></td></tr><tr><td>{Standby/Replica} Count</td><td>Number of Standby/Replica DBs</td><td>-</td></tr><tr><td>Standby Mode</td><td>Standby Mode option<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td><td>Entered only in Tibero</td></tr><tr><td>Log Replication Type</td><td>Log transmission method from Primary (Leader) to Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>(Tibero): A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>(Tibero): A replication mode that, after a log switch, collects and transmits archive log files once they are generated</li><li><strong>ASYNC</strong>(OpenSQL): A replication mode that transmits data asynchronously through the replication connection</li></ul></td><td>-</td></tr></tbody></table>

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

- If you select DR usage in the Enable DR item, the **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** items are exposed.
- If OpenSQL is discovered as an HA configuration, the Enable DR item is automatically shown as **DR enabled** and cannot be changed. To change the Topology, please go back to the previous step.
{% endhint %}

**Instance Configuration**

Input items for each node are automatically configured according to the topology. Each node area can be expanded or collapsed in an accordion form. From the second node onward, if you select the **Same as previous** checkbox, you can apply the Port and Backup Path values entered for the previous node as is.

{% tabs %}
{% tab title="Tibero" %}
**Single**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding,<br>enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

For **Path**, only a file system path can be entered.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host to be registered.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**Single+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Entered only for the Primary instance) | Directly enter the Primary Destination IP to be used in communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP<br>Entered only for the Primary instance |
| Primary Destination Port*<br>(Entered only for the Primary instance) | Directly enter the Primary Destination Port to be used in communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination IP to be used in communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination Port to be used in communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered |

The * notation indicates a required input item.

For **Path**, only a file system path can be entered.

However, the following conditions must be met.

- The entered path must **actually exist** on the target host to be registered.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**TAC**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered,<br>use a shared volume |

The * notation indicates a required input item.

For **Backup Path**, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must be configured as a **shared volume**.
- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.

---

**TAC+DR**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server | If using port forwarding, enter the external Port |
| Primary Destination IP* (Entered only for the Primary instance) | Directly enter the Primary Destination IP to be used in communication from the StandByDB to the PrimaryDB | If using NAT, enter the NAT IP |
| Primary Destination Port*<br>(Entered only for the Primary instance) | Directly enter the Primary Destination Port to be used in communication from the StandBy DB to the Primary DB | If using port forwarding, enter the external Port |
| StandBy Destination IP*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination IP to be used in communication from the Primary DB to the StandBy DB | If using NAT, enter the NAT IP |
| StandBy Destination Port*<br>(Entered only for the StandBy instance) | Directly enter the StandBy Destination Port to be used in communication from the Primary DB to the StandBy DB | If using port forwarding, enter the external Port |
| Backup Path* | Enter the Backup Path | Only a file system path can be entered,<br>use a shared volume |

The * notation indicates a required input item.

For **Backup Path**, only a file system path can be entered.

However, the following conditions must be met.

- All entered Paths must be configured as a **shared volume**.
- The entered path must **actually exist** on the target host for installation.
- The DP Agent execution account must hold **read, write, and execute permissions** for the file system path.
{% endtab %}
{% tab title="OpenSQL" %}
The node section names are shown as **Leader Node** and **Replica Node #{n}**, and if discovered as 2node HA, the **Quorum Node** is also shown.

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname* | Select the host to map to the DB node | - |
| Service IP* | Directly enter or select the IP to be used for communication between OwlDB and the node | If using NAT, enter the NAT IP |
| Service Port* | Directly enter the Port to be used for communication between OwlDB and the database server<br>Default: `5432` | If using port forwarding, enter the external Port |
| Replication Connection IP* | In an HA configuration, directly enter or select the IP to be used for the replication connection | Exposed only in an HA configuration |

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

**Database Configuration**

This is the step for checking the database configuration information and directly entering some items that cannot be collected. Most items are automatically filled in with the discovery results and cannot be modified.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password* | The password of the database super administrator account (SYS) |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>Shown as `-` if VIP is not used |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Memory Size | Shared memory size (not modifiable) |
| Redo Log File Size (MB) | Redo log file size<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| System Data File Size (MB) | Size of the data file that stores system tables and key metadata<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| User Tablespace Data File Size (MB) | Size of the tablespace data file that stores user data<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| Undo Tablespace Data File Size (MB) | Undo tablespace size<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |

The * notation indicates a required input item.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| User Id |   |
| User Password* | The password of the database super administrator account |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| VIP | Database virtual IP<br>Shown as `-` if VIP is not used |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions (not modifiable) |
| Target Memory Size | Target memory size (not modifiable) |
| Shared Buffers | Shared memory size (not modifiable) |
| WAL File Size (MB) | WAL file size<br>It is shown as an empty value because the value could not be confirmed during the discovery process, and it cannot be modified |
| Connection Pooler Port* | The port on which the connection pool listens for client connections<br>Range: 1024~65535 |

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

**Configuration Information Confirmation**

You can enter this step after verification is completed that all options entered in the previous steps have been entered correctly.

Review the configuration information you entered at a glance, and if there are no issues, click the **Register** button to start registration. To modify the content, please click the **Previous** button.

In the summary area on the right side of the screen, you can expand or collapse the information entered at each step to review it, and required items with no value entered are marked as `[Not entered]`.

****The mark indicates a required input item. Other items are automatically filled in with information collected through discovery.***

{% hint style="warning" %}
**Caution**

When you click the **Next** button in the instance configuration and database configuration steps, validation is performed on the following items, and registration cannot proceed if it fails.

- When the OS Timezone differs between nodes (Tibero): You must connect to each node and unify the Timezone settings.
- When SYS account login fails (Tibero): You must check the password you entered.
- When the entered Backup Path does not exist on the host
{% endhint %}

---

## Unregister <a href="#unregister" id="unregister"></a>

A registered database can be excluded from OwlDB management by unregistering rather than deleting it. Even after unregistering, the actual database remains intact in the operating environment, and it can be re-registered if needed.

Unregistration can be performed through the following paths.

- Click **Overview > Tasks > Unregister**
- Click **Dashboard > Select DB Service > Unregister**

When you click the **Unregister** button, a confirmation modal is displayed, and when you enter the DB Service Name and click the **Confirm** button, unregistration is completed.
