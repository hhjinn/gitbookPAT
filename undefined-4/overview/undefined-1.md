This feature changes the node configuration and DR configuration of a running database.

Spec change **Overview** or **Change Detection DB**It starts here and proceeds through five steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Review. In each step, you check the current settings and modify the items you need, then in the final step you review and apply the changes.

Spec change is available when the database operating status is `Running` or `Degraded`. The range of items that can be changed varies depending on the DB engine (Tibero/OpenSQL) and the installation method (Installed DB/Registered DB).

{% hint style="info" %}
**Note**

- `Degraded` If there is even one node that is in progress or unavailable in this state, the Spec Change button cannot be used.
- `Degraded` If the only issue is one caused by a Failover Primary while in this state, the Spec Change button can be used.
{% endhint %}

## How to Access Spec Change

The Spec Change page can be accessed in the following two ways.

- **Access from Overview** : Overview page > Actions > **Spec Change** click
- **Access from Change Detection DB** : Dashboard > Explore > Change Detection DB > **Spec Change** click

## Spec Change

If you accessed from the Overview, proceed in the following order.

1. On the Overview page, **Actions > Spec Change**Click to enter the Spec Change page.
2. **Engine Options** In this step, check the node configuration and set Scale In/Out if necessary.
3. **DR Configuration** In this step, adjust whether DR is used, the failover automation level, and the Standby node settings.
4. **Instance Configuration** In this step, check the information for each node and modify the Backup Path if necessary.
5. **Database Configuration** In this step, enter configuration information such as the VIP for the added node.
6. **Configuration Review** In this step, review the changes and then **Complete**click.

## Accessing Spec Change from Overview

When accessed through the Actions menu on the Overview page, you can perform two tasks: changing the DB node configuration and modifying DR.

### Changing DB Node Configuration

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**allows Scale In/Out of the Primary node within the topology range of the standard architecture. For example, a TAC configuration can adjust the Primary nodes from a minimum of 2 to a maximum of 4.
- **Registered DB**does not support Scale In/Out.
- If you performed Scale Out and the DB is using VIP, you must enter the VIP of the added node in the **Database Configuration** step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in spec change.
- Scale In/Out is only possible for nodes whose environment has been pre-configured, the same as with a new installation.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- **Installed DB**supports switching between Single ↔ HA topologies, and in the HA topology it also supports Scale In/Out of the Standby node. The number of adjustable nodes is 1 for Single and 2–3 for HA.
- **Registered DB**does not support topology changes or Scale In/Out.
- If you performed a topology switch or Scale Out and the DB is using VIP, you must enter the VIP of the added node in the **Database Configuration** step.
{% endtab %}
{% endtabs %}

### Modifying DR

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**can change DR usage ↔ non-usage, and can also change the failover automation level. When DR is in use, the Open Mode and Log Replication Type of the Standby node can also be changed.
- **Registered DB**does not support changing whether DR is used; when DR is in use, only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used is automatically determined by the topology selected in the **Engine Options** step. Switching from Single → HA automatically enables DR, and switching from HA → Single automatically disables DR.
- **Installed DB**changes whether DR is used only through topology switching as described above, and the failover automation level can be changed separately.
- **Registered DB**does not support changing whether DR is used; only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, refer to the [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz).
{% endhint %}

## Spec Change Steps

The items that can be configured at each stage vary depending on the engine.

{% tabs %}
{% tab title="Engine Options" %}
This is the stage for verifying the database alias, engine, and topology information. Most items cannot be changed, and only the node configuration can be adjusted for installation DBs. **Tibero**

| Item | Description | Changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Cannot be changed |
| Database Engine Type | Tibero | Cannot be changed |
| Topology | Single, TAC | Cannot be changed |
| Node Count | Single: 1, TAC: 2–4 | Cannot be changed (reflects the Scale In/Out results below) |
| Scale In/Out | Primary node list | Available only for installation DBs |

**OpenSQL**

| Item | Description | Changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Cannot be changed |
| Database Engine Type | OpenSQL | Cannot be changed |
| Topology | Single, HA | Installation DBs can switch between Single ↔ HA |
| Node Count | Single: 1, HA: 2–3 | Cannot be changed (reflects topology switch/Scale results) |
| PostgreSQL Version | The PostgreSQL version to use | Cannot be changed |

**Scale In/Out (Tibero TAC installation DB)**

For an installed Tibero TAC topology DB, the Scale In/Out item is displayed below Node Count. You can verify the currently configured Primary node list and perform the following operations.

- **Add** : Clicking the Add button lets you select an instance from the list of hosts available for installation. The selected host is added to the table and becomes a Scale Out target.
- **Delete** : Select the node to remove from the table and then click the Delete button. The selected node becomes a Scale In target and is displayed dimmed in the table.
- **Reset** : Reverts the added or deleted changes to their initial state.

When you switch the topology from Single → HA in an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is **DR Configuration** verified and adjusted in the stage.
{% endtab %}
{% tab title="DR Configuration" %}
This is the stage for changing whether DR is used, the failover automation level, and the Standby node settings. The change method varies depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether to use the DR configuration. This can be changed directly only for installation DBs. |
| Failover Automation Level | Supports Level 0 (manual), Level 1 (automatic failover), and Level 3 (fully automated).<br>Level 2 (automatic configuration recovery) is not supported in OwlDB v1.3 |
| Standby Count | Installation: fixed at 1 / Registration: 1–9 |
| Standby Mode | Select the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Select the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether to use DR can be changed directly only for installation DBs, and it behaves as follows depending on the direction of the change.

| Change direction | Behavior |
| --- | --- |
| Disabled → Enabled | Newly builds a Standby node and sets the failover automation level to the default Level 1. |
| Enabled → Disabled | Performs clean up of the Standby node. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | **Engine Options** Automatically determined based on the topology selection in the step (HA → used, Single → not used) |
| Failover Automation Level | Supports Level 0 (manual) and Level 3 (fully automated) |
| Standby Count | 1 to 2 |
| Standby Scale In/Out | Installed DBs can be adjusted directly using the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication (fixed) |

**Standby node settings** (Displayed when DR is used)

The list of currently configured Standby nodes is displayed, and the following items can be changed. The detailed options for Log Replication Type are as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>Set the Open Mode of the Standby node to <code>Recovery</code> or <code>Read Only</code> (applicable to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Select the log replication method for the Standby node. (applicable to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when transactions occur</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node in OpenSQL is fixed to asynchronous (ASYNC) replication, and Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This step verifies the configuration information for each Primary / Standby node. Some items are not retrieved depending on the topology and installation/registration method, and **Backup Path**only can be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Not editable |
| Service IP | IP used for communication between OwlDB and the node | Not editable |
| Service Port | Port used for communication between OwlDB and the database server | Not editable |
| Interconnect IP | Interconnect IP used for communication between nodes within the cluster | Not editable |
| Primary Destination IP | IP used for communication from the Standby DB to the Primary DB (entered for the Primary instance only) | Not editable |
| Primary Destination Port | Port used for communication from the Standby DB to the Primary DB (entered for the Primary instance only) | Not editable |
| Standby Destination IP | IP used for communication from the Primary DB to the Standby DB (entered for the Standby instance only) | Not editable |
| Standby Destination Port | Port used for communication from the Primary DB to the Standby DB (entered for the Standby instance only) | Not editable |
| Data Path | Data Path | Not editable |
| Redo Path | Redo Path | Not editable |
| Archive Path | Archive Path | Not editable |
| Backup Path | Backup Path | Only file system paths can be entered |
| SSH Port | SSH Port | Not editable |
| SSH User | SSH User | Not editable |
| SSH Key File Path | SSH Key File Path | Not editable |

**Backup Path**can only be entered as a file system path, and for a TAC configuration, it must be configured as a **shared volume**.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP used for communication between OwlDB and nodes | Cannot be modified |
| Service Port | Port used for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | IP used for replication connection in an HA configuration | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

Configuration information for each Primary / Standby node is displayed according to the configuration set in the previous step. During Scale Out, input fields for the added node are newly created, and during Scale In, that node is removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This is the step for verifying the database configuration information. Most items are automatically set through OwlDB metadata and actual DB discovery data. **Common items**

| Item | Description |
| --- | --- |
| Database Name | Name of the database to be used |
| SYS User Password | Password for the database super-privilege administrator account (SYS user) |
| Target Memory Size | Target memory size |
| Character Set | Character encoding to be used for the database |
| Timezone | OS timezone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |

**Tibero-specific items**

| Item | Description |
| --- | --- |
| VIP | Select whether to use VIP |
| Primary Node #N VIP | Database virtual IP (enabled when VIP usage is selected) |
| Shared Memory Size (MB) | Shared memory size |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | Size of the data file for storing system tables and key metadata |
| Syssub Data File Size (MB) | Size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | Size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (MB) | Size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

**OpenSQL-specific items**

| Item | Description |
| --- | --- |
| VIP | Database virtual IP (enabled when a network interface is selected) |
| Shared Buffers (%) | Shared memory size |
| WAL File Size (MB) | WAL File Size |
| Connection Pooler Port | The port on which OpenProxy accepts client connections |

If Scale Out occurs on a DB that is using VIP, the VIP for the added node must be entered in this step.
{% endtab %}
{% tab title="Verify Configuration Information" %}
Perform a final review of the settings configured in the previous steps. Changed items are displayed in blue, and each item can be expanded/collapsed using an accordion. **Complete**Clicking it applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changes, the Complete button is disabled.
- If you change the DR configuration to disabled while a Retired instance exists in the Standby, a confirmation modal appears. Upon confirmation, all Retired instances are deleted and the DR configuration is changed to disabled.
{% endhint %}

{% hint style="info" %}
**Note**

**Complete** When clicked, it checks for the presence of a license file, the CP Max Core count, and whether the license options match. In the following cases, the spec change does not proceed and an error message is displayed.

- If no license file exists on the node: Place the license file and try again.
- If the requested Core count exceeds the license maximum Core count: Adjust the Core count and try again.
- If the requested configuration does not match the current license options: Select a configuration that matches the license options and try again.
- If multiple spec change requests occur simultaneously and processing is delayed: Wait a moment and try again.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB

For registered DBs only, when the DB configuration is changed outside of OwlDB, OwlDB automatically detects it. Check the DB where a change was detected in the dashboard explorer and **Spec Change**Click it to enter.

When you enter this way, the externally changed details are automatically reflected on the spec change page. Changed items are marked separately, and the user can review the content and then apply it to OwlDB.

**Example** If a TAC DR DB using VIP (Primary 2 nodes / Standby 1 node) is externally expanded to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine Options** : The 2 added nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the 2 added nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the 2 added nodes are created.

After reviewing the content in each step and completing the input, **Complete**Clicking it applies the changes to OwlDB.

## Maximum spec change processing time

The spec change processing time may vary depending on the database capacity, load state, and configuration environment. In a typical environment, it is processed within a few minutes, and depending on the conditions, the maximum processing time may vary as follows.

{% tabs %}
{% tab title="Primary creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database configuration | 6 hours | Script execution |
{% endtab %}
{% tab title="Primary removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Stopping the Database and removing cluster resources | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% tab title="Standby creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Creating CM on a Single Database | 30 minutes | Database stop |
|   | 6 hours | Adding cluster resources |
|   | 30 minutes | Database startup |
| Primary node backup | 6 hours |   |
| Standby node configuration | 6 hours |   |
| Primary node parameter change | 30 minutes | Stopping the Database and adding parameters to tip |
|   | 30 minutes | Database startup |
{% endtab %}
{% tab title="Standby removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Stopping the Database and removing cluster resources | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
