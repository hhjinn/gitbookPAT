This feature changes the node configuration and DR configuration of a running database.

Spec change **Overview** or **Change Detection DB**It starts here and proceeds through five steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Review. In each step, you check the current settings and modify the necessary items, then in the final step you review and apply the changes.

Spec change is available when the database operating status is `Running` or `Degraded`The range of items that can be changed varies depending on the DB engine (Tibero/OpenSQL) and the installation method (Installed DB/Registered DB).

{% hint style="info" %}
**Note**

- `Degraded` If there is any node that is in progress or unavailable in this state, the Spec Change button cannot be used.
- `Degraded` If the state has only issues caused by a Failover Primary, the Spec Change button can be used.
{% endhint %}

## How to Access Spec Change

The Spec Change page can be accessed in the following two ways.

- **Access from Overview** : Overview page > Actions > **Spec Change** Click
- **Access from Change Detection DB** : Dashboard > Explore > Change Detection DB > **Spec Change** Click

## Spec Change

When accessed from Overview, proceed in the following order.

1. On the Overview page, **Actions > Spec Change**Click this to access the Spec Change page.
2. **Engine Options** In this step, check the node configuration and set Scale In/Out if necessary.
3. **DR Configuration** In this step, adjust whether to use DR, the failover automation level, and the Standby node settings.
4. **Instance Configuration** In this step, check the information for each node and modify the Backup Path if necessary.
5. **Database Configuration** In this step, enter configuration information such as the VIP of the added node.
6. **Configuration Review** In this step, review the changes and then click **Complete**Click.

## Accessing Spec Change from Overview

When accessed through the Actions menu on the Overview page, you can perform two tasks: DB node configuration change and DR modification.

### DB Node Configuration Change

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**allows Scale In/Out of the Primary node within the topology range of the standard architecture. For example, a TAC configuration allows the Primary node to be adjusted from a minimum of 2 to a maximum of 4.
- **Registered DB**does not support Scale In/Out.
- If Scale Out was performed, for a DB using VIP, you must enter the VIP of the added node in the **Database Configuration** step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in Spec Change.
- Scale In/Out is only possible for nodes whose prior environment configuration has been completed, the same as for a new installation.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- **Installed DB**supports Single ↔ HA topology conversion, and in the HA topology it also supports Scale In/Out of the Standby node. The adjustable number of nodes is 1 for Single and 2–3 for HA.
- **Registered DB**does not support topology change or Scale In/Out.
- If a topology conversion or Scale Out was performed, for a DB using VIP, you must enter the VIP of the added node in the **Database Configuration** step.
{% endtab %}
{% endtabs %}

### DR Modification

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**can change DR usage ↔ non-usage, and can also change the failover automation level. When DR is in use, the Open Mode and Log Replication Type of the Standby node can also be changed.
- **Registered DB**does not support changing whether DR is used; when DR is in use, only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used is automatically determined by the topology selected in the **Engine Options** step. Converting from Single → HA automatically enables DR, and converting from HA → Single automatically disables DR.
- **Installed DB**As described above, whether DR is used changes only through topology conversion, and the failover automation level can be changed separately.
- **Registered DB**does not support changing whether DR is used; only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, refer to the [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz)Please refer to it.
{% endhint %}

## Spec Change Steps

The items that can be configured at each step vary depending on the engine.

{% tabs %}
{% tab title="Engine Options" %}
This step verifies the database alias, engine, and topology information. Most items cannot be changed; only the node configuration can be adjusted, and only for installed DBs. **Tibero**

| Item | Description | Changeable |
| --- | --- | --- |
| Database Alias | An alias for identifying the database | Cannot be changed |
| Database Engine Type | Tibero | Cannot be changed |
| Topology | Single, TAC | Cannot be changed |
| Node Count | 1 for Single, 2–4 for TAC | Cannot be changed (reflects the Scale In/Out results below) |
| Scale In/Out | Primary node list | Available only for installed DBs |

**OpenSQL**

| Item | Description | Changeable |
| --- | --- | --- |
| Database Alias | An alias for identifying the database | Cannot be changed |
| Database Engine Type | OpenSQL | Cannot be changed |
| Topology | Single, HA | Installed DBs can switch between Single ↔ HA |
| Node Count | 1 for Single, 2–3 for HA | Cannot be changed (reflects the topology switch/Scale results) |
| PostgreSQL Version | The PostgreSQL version to use | Cannot be changed |

**Scale In/Out (Tibero TAC installed DB)**

For an installed Tibero TAC topology DB, a Scale In/Out item is displayed below Node Count. You can verify the list of currently configured Primary nodes and perform the following operations.

- **Add** : Clicking the Add button lets you select an instance from the list of installable hosts. The selected host is added to the table and becomes a Scale Out target.
- **Delete** : After selecting the node to remove from the table, click the Delete button. The selected node becomes a Scale In target and is displayed dimmed in the table.
- **Reset** : Reverts any added or deleted changes to their initial state.

When switching the topology from Single → HA on an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is **DR Configuration** verified and adjusted in this step.
{% endtab %}
{% tab title="DR Configuration" %}
This step changes whether DR is used, the failover automation level, and the Standby node settings. The change method varies depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether the DR configuration is used. This can be changed directly only for installed DBs. |
| Failover Automation Level | Level 0 (manual), Level 1 (automatic failover), and Level 3 (fully automated) are supported.<br>Level 2 (automatic configuration recovery) is not supported in OwlDB v1.3 |
| Standby Count | Installation: fixed at 1 / Registration: 1–9 |
| Standby Mode | Select the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Select the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether DR is used can be changed directly only on installed DBs, and it behaves as follows depending on the direction of change.

| Change Direction | Behavior |
| --- | --- |
| Disabled → Enabled | Newly builds the Standby node and sets the failover automation level to Level 1 by default. |
| Enabled → Disabled | Performs Standby node clean up. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | **Engine Options** Automatically determined based on the topology selection in the previous step (HA → enabled, Single → disabled) |
| Failover Automation Level | Supports Level 0 (manual) and Level 3 (fully automated) |
| Standby Count | 1 to 2 |
| Standby Scale In/Out | Installed DBs can be adjusted directly using the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication (fixed) |

**Standby node settings** (Shown when DR is enabled)

The list of currently configured Standby nodes is displayed, and the following items can be changed. The detailed options for Log Replication Type are as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>The Open Mode of the Standby node is set to <code>Recovery</code> or <code>Read Only</code> , whichever is selected. (Applies to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Selects the log replication method for the Standby node. (Applies to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node of OpenSQL is fixed to asynchronous (ASYNC) replication, and Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This is the step for checking the configuration information for each Primary / Standby node. Some items may not be displayed depending on the topology and installation/registration method, and **Backup Path**only can be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Not editable |
| Service IP | IP used for communication between OwlDB and the node | Not editable |
| Service Port | Port used for communication between OwlDB and the database server | Not editable |
| Interconnect IP | Interconnect IP used for communication between nodes within the cluster | Not editable |
| Primary Destination IP | IP used for communication from the Standby DB to the Primary DB (entered only for the Primary instance) | Not editable |
| Primary Destination Port | Port used for communication from the Standby DB to the Primary DB (entered only for the Primary instance) | Not editable |
| Standby Destination IP | IP used for communication from the Primary DB to the Standby DB (entered only for the Standby instance) | Not editable |
| Standby Destination Port | Port used for communication from the Primary DB to the Standby DB (entered only for the Standby instance) | Not editable |
| Data Path | Data Path | Not editable |
| Redo Path | Redo Path | Not editable |
| Archive Path | Archive Path | Not editable |
| Backup Path | Backup Path | Only a file system path can be entered |
| SSH Port | SSH Port | Not editable |
| SSH User | SSH User | Not editable |
| SSH Key File Path | SSH Key File Path | Not editable |

**Backup Path**can only accept a file system path, and in the case of a TAC configuration, it **shared volume**must be configured as.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP used for communication between OwlDB and nodes | Cannot be modified |
| Service Port | Port used for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | IP used for the replication connection in an HA configuration | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

Configuration information for each Primary / Standby node is displayed according to the configuration set in the previous step. When scaling out, an input field for the added node is newly created, and when scaling in, the corresponding node is removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This is the step for reviewing the database configuration information. Most items are automatically set through OwlDB metadata and actual DB discovery data. **Common items**

| Item | Description |
| --- | --- |
| Database Name | Name of the database to be used |
| SYS User Password | Password for the highest-privilege database administrator account (SYS user) |
| Target Memory Size | Target memory size |
| Character Set | Character encoding to be used for the database |
| Timezone | OS time zone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrent sessions allowed |

**Tibero-specific items**

| Item | Description |
| --- | --- |
| VIP | Select whether to use VIP |
| Primary Node #N VIP | Database virtual IP (enabled when VIP use is selected) |
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

If Scale Out has occurred in a DB that is using VIP, the VIP for the added node must be entered in this step.
{% endtab %}
{% tab title="Verify Configuration Information" %}
Perform a final review of the settings configured in the previous steps. Changed items are displayed in blue, and each item can be expanded or collapsed using an accordion. **Complete**Clicking this applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changes, the Complete button is disabled.
- If you change the DR configuration to disabled while a Retired instance exists in the Standby, a confirmation modal is displayed. Upon confirmation, all Retired instances are deleted and the DR configuration is changed to disabled.
{% endhint %}

{% hint style="info" %}
**Note**

**Complete** When clicked, it verifies the presence of the license file, the CP Max Core count, and whether the license options match. In the following cases, the spec change does not proceed and an error message is displayed.

- If the license file does not exist on the node: place the license file and try again.
- If the requested Core count exceeds the maximum Core count of the license: adjust the Core count and try again.
- If the requested configuration does not match the current license options: reselect a configuration that matches the license options.
- If processing is delayed due to multiple simultaneous spec change requests: try again after a moment.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB

For registered DBs only, if the DB configuration is changed outside of OwlDB, OwlDB automatically detects it. In Dashboard Exploration, check the DB where the change was detected and **Spec Change**click it to enter.

When entering this way, externally changed details are automatically reflected on the spec change page. Changed items are marked separately, and the user can review the contents before applying them to OwlDB.

**Example** If a TAC DR DB using VIP (Primary 2 nodes / Standby 1 node) is externally expanded to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine Options** : The added 2 nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the added 2 nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the added 2 nodes are created.

After reviewing the contents and completing the input in each step, **Complete**clicking this applies the changes to OwlDB.

## Maximum spec change processing time

The spec change processing time may vary depending on the database capacity, load state, and configuration environment. In a typical environment, it is processed within a few minutes, and the maximum processing time may differ depending on the conditions as follows.

{% tabs %}
{% tab title="Primary Creation" %}
| Logic | Maximum Processing Time | Remarks |
| --- | --- | --- |
| Database Configuration | 6 hours | Script Execution |
{% endtab %}
{% tab title="Primary Removal" %}
| Logic | Maximum Processing Time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% tab title="Standby Creation" %}
| Logic | Maximum Processing Time | Remarks |
| --- | --- | --- |
| Creating CM on a Single Database | 30 minutes | Database shutdown |
|   | 6 hours | Cluster resource addition |
|   | 30 minutes | Database startup |
| Primary node backup | 6 hours |   |
| Standby node configuration | 6 hours |   |
| Primary node parameter change | 30 minutes | Database shutdown, adding parameter to tip |
|   | 30 minutes | Database startup |
{% endtab %}
{% tab title="Standby Removal" %}
| Logic | Maximum Processing Time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
