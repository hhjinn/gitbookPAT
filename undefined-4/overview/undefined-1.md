This is a feature that changes the node configuration and DR configuration of the operating database.

Spec change **Overview** or **Change Detection DB**It starts from, and proceeds in 5 steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Information Confirmation. In each step, you check the current settings and modify the necessary items, then in the final step, you finally confirm and apply the changes.

Spec change is possible when the database operation status is `Running` or `Degraded`. The range of changeable items varies depending on the DB engine (Tibero/OpenSQL) and installation method (installed DB/registered DB).

{% hint style="info" %}
**Note**

- `Degraded` If there is any node that is in progress or unavailable in the state, the spec change button cannot be used.
- `Degraded` If in the state and only an issue caused by a Failovered Primary exists, the spec change button can be used.
{% endhint %}

## How to Enter Spec Change

The spec change page can be entered in the following two ways.

- **Enter from Overview** : Overview page > Actions > **Spec Change** Click
- **Enter from Change Detection DB** Dashboard > Explore > Change Detection DB > **Spec Change** Click

## Spec Change

If you entered from the Overview, proceed in the following order.

1. On the Overview page, **Operations > Spec Change**Click to enter the Spec Change page.
2. **Engine Options** In this step, check the node configuration and configure Scale In/Out if necessary.
3. **DR Configuration** In this step, adjust whether to use DR, the failover automation level, and the Standby node settings.
4. **Instance Configuration** In this step, check the per-node information and modify the Backup Path if necessary.
5. **Database Configuration** In this step, enter configuration information such as the VIP of the added node.
6. **Configuration Information Review** In this step, after reviewing the changes, **Complete**Click.

## Entering Spec Change from the Overview

If you entered through the Operations menu on the Overview page, you can perform two tasks: DB node configuration change and DR modification.

### DB Node Configuration Change

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**Scale In/Out of the Primary node is possible within the topology scope of the standard architecture. For example, a TAC configuration can adjust the Primary nodes from a minimum of 2 to a maximum of 4.
- **Registered DB**Does not support Scale In/Out.
- If you perform Scale Out, and the DB is using a VIP, **Database Configuration** you must enter the VIP of the added node in this step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in Spec Change.
- Scale In/Out is only possible for nodes whose prerequisite environment configuration has been completed, the same as for a new installation.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- **Installed DB**Supports Single ↔ HA topology conversion, and in the HA topology, Scale In/Out of the Standby node is also supported. The adjustable number of nodes is 1 for Single and 2–3 for HA.
- **Registered DB**Does not support topology change or Scale In/Out.
- If you perform topology conversion or Scale Out, and the DB is using a VIP, **Database Configuration** you must enter the VIP of the added node in this step.
{% endtab %}
{% endtabs %}

### DR Modification

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**You can change DR enabled ↔ disabled, and you can also change the failover automation level. When DR is in use, you can also change the Open Mode and Log Replication Type of the Standby node.
- **Registered DB**Does not support changing whether DR is used, and when DR is in use, you can only change the failover automation level and the Standby node settings.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used **Engine Options** is automatically determined according to the topology selected in this step. Converting from Single → HA automatically enables DR, and converting from HA → Single automatically disables DR.
- **Installed DB**As described above, whether DR is used can only be changed through topology conversion, and the failover automation level can be changed separately.
- **Registered DB**Does not support changing whether DR is used, and you can only change the failover automation level and the Standby node settings.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz)please refer to.
{% endhint %}

## Spec Change Steps

The items that can be configured in each step differ depending on the engine.

{% tabs %}
{% tab title="Engine Options" %}
This is the step for checking the database alias, engine, and topology information. Most items cannot be changed, and only the node configuration can be adjusted for installed DBs. **Tibero**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Cannot be changed |
| Database Engine Type | Tibero | Cannot be changed |
| Topology | Single, TAC | Cannot be changed |
| Node Count | Single 1, TAC 2–4 | Cannot be changed (reflects the Scale In/Out results below) |
| Scale In/Out | Primary node list | Available only for installed DBs |

**OpenSQL**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Cannot be changed |
| Database Engine Type | OpenSQL | Cannot be changed |
| Topology | Single, HA | Installed DBs can convert between Single ↔ HA |
| Node Count | Single 1, HA 2–3 | Cannot be changed (reflects topology conversion/Scale results) |
| PostgreSQL Version | PostgreSQL version to use | Cannot be changed |

**Scale In/Out (Tibero TAC installed DB)**

For an installed Tibero TAC topology DB, a Scale In/Out item is displayed below the Node Count. You can check the currently configured Primary node list and perform the following operations.

- **Add** : When you click the Add button, you can select an instance from the list of installable hosts. The selected host is added to the table and becomes a Scale Out target.
- **Delete** : After selecting the node to remove from the table, click the Delete button. The selected node becomes a Scale In target and is displayed dimmed in the table.
- **Reset** : Reverts the added or deleted changes to their initial state.

When you convert the topology from Single → HA in an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is checked and adjusted in this step. **DR Configuration** in this step.
{% endtab %}
{% tab title="DR Configuration" %}
This is the step for changing whether DR is used, the failover automation level, and the Standby node settings. The change method differs depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether the DR configuration is used. It can be changed directly only for installed DBs. |
| Failover Automation Level | Supports Level 0 (manual), Level 1 (automatic failover), and Level 3 (full automation).<br>Level 2 (automatic configuration recovery) is not supported in OwlDB v1.3. |
| Standby Count | Installation: fixed at 1 / Registration: 1–9 |
| Standby Mode | Selecting the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Selecting the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether DR is used can be changed directly only for installed DBs, and it operates as follows depending on the direction of the change.

| Change direction | Operation |
| --- | --- |
| Disabled → Enabled | Newly builds a Standby node and sets the failover automation level to the default Level 1. |
| Enabled → Disabled | Performs cleanup of the Standby node. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | **Engine Options** Automatically determined according to the topology selection in this step (HA → enabled, Single → disabled) |
| Failover Automation Level | Supports Level 0 (manual) and Level 3 (full automation) |
| Standby Count | 1–2 |
| Standby Scale In/Out | Installed DBs can be adjusted directly with the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication method (fixed) |

**Standby Node Settings** (Displayed when DR is used)

The currently configured Standby node list is displayed, and you can change the following items. The detailed options for Log Replication Type are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>Set the Open Mode of the Standby node <code>Recovery</code> or <code>Read Only</code> select from among. (Applicable to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Selects the log replication method for the Standby node. (Applicable to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits the archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node of OpenSQL is fixed to the asynchronous (ASYNC) replication method, and the Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This is the step for checking the configuration information of each Primary / Standby node. Some items may not be retrieved depending on the topology and the installation/registration method, and **Backup Path**can only be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP to be used for communication between OwlDB and the node | Cannot be modified |
| Service Port | Port to be used for communication between OwlDB and the database server | Cannot be modified |
| Interconnect IP | Interconnect IP to be used for communication between nodes within the cluster | Cannot be modified |
| Primary Destination IP | IP to be used for communication from the Standby DB to the Primary DB (enter for the Primary instance only) | Cannot be modified |
| Primary Destination Port | Port to be used for communication from the Standby DB to the Primary DB (enter for the Primary instance only) | Cannot be modified |
| Standby Destination IP | IP to be used for communication from the Primary DB to the Standby DB (enter for the Standby instance only) | Cannot be modified |
| Standby Destination Port | Port to be used for communication from the Primary DB to the Standby DB (enter for the Standby instance only) | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| Redo Path | Redo Path | Cannot be modified |
| Archive Path | Archive Path | Cannot be modified |
| Backup Path | Backup Path | Only file system paths can be entered |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

**Backup Path**only accepts a file system path, and in the case of a TAC configuration **shared volume**be configured as.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP to be used for communication between OwlDB and the node | Cannot be modified |
| Service Port | Port to be used for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | IP to be used for the replication connection in an HA configuration | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

The configuration information for each Primary / Standby node is displayed according to the configuration set in the previous step. When scaling out, input fields for the added nodes are newly created, and when scaling in, the corresponding nodes are removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This is the step for checking the database configuration information. Most items are set automatically through the OwlDB metadata and the actual DB discovery data. **Common items**

| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password | Password for the database highest-privilege administrator account (SYS user) |
| Target Memory Size | Target memory size |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |

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
| WAL File Size (MB) | WAL file size |
| Connection Pooler Port | Port on which OpenProxy accepts client connections |

If a Scale Out occurs on a DB that is using VIP, the VIP for the added node must be entered in this step.
{% endtab %}
{% tab title="Configuration Information Review" %}
This performs a final review of the settings configured in the previous steps. Changed items are displayed in blue, and each item can be expanded/collapsed with an accordion. **Complete**Clicking this applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changes, the Complete button is disabled.
- If you change the DR configuration to unused while a Retired instance exists in the Standby, a confirmation modal is displayed. Upon confirmation, the Retired instance is entirely deleted and the DR configuration is changed to unused.
{% endhint %}

{% hint style="info" %}
**Note**

**Complete** When clicked, it checks for the presence of the license file, the CP Max Core count, and whether the license options match. In the following cases, the spec change does not proceed and an error message is displayed.

- If no license file exists on the node: Place the license file and try again.
- If the requested Core count exceeds the maximum Core count of the license: Adjust the Core count and try again.
- If the requested configuration does not match the current license options: Reselect a configuration that matches the license options.
- If processing is delayed because multiple spec change requests occur simultaneously: Try again after a moment.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB

For registered DBs only, when the DB configuration is changed outside of OwlDB, OwlDB automatically detects this. You can check the DB in which a change was detected in the dashboard discovery and **Spec Change**click this to enter.

When entering via this method, the changes made externally are automatically reflected on the spec change page. Changed items are marked separately, and the user can apply them to OwlDB after reviewing the content.

**Example** When a TAC DR DB using VIP (2 Primary nodes / 1 Standby node) is externally expanded to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine Options** : The 2 added nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the 2 added nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the 2 added nodes are created.

After reviewing the content in each step and completing the input, **Complete**clicking this reflects the changes in OwlDB.

## Maximum processing time for spec changes

The spec change processing time may vary depending on the database capacity, load state, and configuration environment. In a typical environment, it is processed within a few minutes, and depending on the conditions, the maximum processing time may differ as follows.

{% tabs %}
{% tab title="Primary creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database configuration | 6 hours | Script execution |
{% endtab %}
{% tab title="Primary removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% tab title="Standby creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Creating CM on a Single Database | 30 minutes | Database shutdown |
|   | 6 hours | Adding cluster resources |
|   | 30 minutes | Database startup |
| Primary node backup | 6 hours |   |
| Standby node configuration | 6 hours |   |
| Primary node parameter change | 30 minutes | Database shutdown, adding parameters to tip |
|   | 30 minutes | Database startup |
{% endtab %}
{% tab title="Standby removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
