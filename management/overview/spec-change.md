This is a function that changes the node configuration and DR configuration of a database in operation.

Spec change is **Overview** or **Change Detection DB**It starts at, and proceeds through 5 steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Info Verification. At each step, you check the current settings and modify the necessary items, then at the final step you perform a final review of the changes and apply them.

Spec changes are possible when the database operation status is `Running` or `Degraded`. The range of changeable items differs depending on the DB engine (Tibero/OpenSQL) and the installation method (Installed DB/Registered DB).

{% hint style="info" %}
**Note**

- `Degraded` If there is even one node that is in progress or unavailable in the state, the Spec Change button cannot be used.
- `Degraded` If it is in the state and only issues caused by a Failover Primary exist, the Spec Change button can be used.
{% endhint %}

## How to Access Spec Change <a href="#spec-change-entry" id="spec-change-entry"></a>

The Spec Change page can be accessed in the following two ways.

- **Access from Overview** : Overview page > Operations > **Spec change** Click
- **Access from Change Detection DB** : Dashboard > Explore > Change Detection DB > **Spec change** Click

## Spec change <a href="#spec-change" id="spec-change"></a>

When accessed from Overview, proceed in the following order.

1. On the Overview page, **Operations > Spec Change**Click to access the Spec Change page.
2. **Engine options** At the step, check the node configuration and configure Scale In/Out if necessary.
3. **DR configuration** At the step, adjust whether DR is used, the failover automation level, and the Standby node settings.
4. **Instance Configuration** At the step, check the information for each node and modify the Backup Path if necessary.
5. **Database Configuration** At the step, enter configuration information such as the VIP of the added node.
6. **Configuration Information Confirmation** At the step, after reviewing the changes, **Complete**Click.

## Accessing Spec Change from Overview <a href="#spec-change-from-overview" id="spec-change-from-overview"></a>

When accessed through the Operations menu on the Overview page, you can perform two tasks: DB node configuration change and DR modification.

### DB Node Configuration Change <a href="#db" id="db"></a>

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**allows Scale In/Out of the Primary node within the topology range of the standard architecture. For example, a TAC configuration can adjust Primary nodes from a minimum of 2 to a maximum of 4.
- **Registered DB**does not support Scale In/Out.
- If Scale Out was performed, and the DB is using VIP, **Database Configuration** you must enter the VIP of the added node at the step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in Spec Change.
- Scale In/Out is only possible for nodes whose prerequisite environment configuration has been completed, the same as a new installation.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- **Installed DB**supports Single ↔ HA topology conversion, and in the HA topology it also supports Scale In/Out of Standby nodes. The adjustable number of nodes is 1 for Single and 2–3 for HA.
- **Registered DB**does not support topology changes or Scale In/Out.
- If topology conversion or Scale Out was performed, and the DB is using VIP, **Database Configuration** you must enter the VIP of the added node at the step.
{% endtab %}
{% endtabs %}

### DR Modification <a href="#dr" id="dr"></a>

{% tabs %}
{% tab title="Tibero" %}
- **Installed DB**can change DR usage ↔ non-usage, and can also change the failover automation level. When DR is in use, the Open Mode and Log Replication Type of the Standby node can also be changed.
- **Registered DB**does not support changing whether DR is used, and when DR is in use, only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used is automatically determined by the topology selected at the **Engine options** step. When converting from Single → HA, DR is automatically enabled, and when converting from HA → Single, DR is automatically disabled.
- **Installed DB**As described above, whether DR is used is changed only through topology conversion, and the failover automation level can be changed separately.
- **Registered DB**does not support changing whether DR is used, and only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz)please refer to.
{% endhint %}

## Spec Change Steps <a href="#spec-change-steps" id="spec-change-steps"></a>

The items that can be configured at each step differ depending on the engine.

{% tabs %}
{% tab title="Engine options" %}
This step verifies the database alias, engine, and topology information. Most items cannot be changed, and only the node configuration can be adjusted for Installed DBs. **Tibero**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Not changeable |
| Database Engine Type | Tibero | Not changeable |
| Topology | Single, TAC | Not changeable |
| Node Count | 1 for Single, 2–4 for TAC | Not changeable (reflects the Scale In/Out results below) |
| Scale In/Out | Primary node list | Available only for Installed DBs |

**OpenSQL**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias for identifying the database | Not changeable |
| Database Engine Type | OpenSQL | Not changeable |
| Topology | Single, HA | Installed DBs support Single ↔ HA conversion |
| Node Count | 1 for Single, 2–3 for HA | Not changeable (reflects topology conversion/Scale results) |
| PostgreSQL Version | PostgreSQL version to use | Not changeable |

**Scale In/Out (Tibero TAC Installed DB)**

For an installed Tibero TAC topology DB, a Scale In/Out item is displayed below Node Count. You can check the currently configured Primary node list and perform the following tasks.

- **Add** : Clicking the Add button lets you select an instance from the list of installable hosts. The selected host is added to the table and becomes a Scale Out target.
- **Delete** : After selecting the node to remove from the table, click the Delete button. The selected node becomes a Scale In target and is displayed dimmed in the table.
- **Reset** : Reverts added or deleted changes to the initial state.

When converting the topology from Single → HA in an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is checked and adjusted at the **DR configuration** step.
{% endtab %}
{% tab title="DR configuration" %}
This step changes whether DR is used, the failover automation level, and the Standby node settings. The change method differs depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether the DR configuration is used. It can be changed directly only for Installed DBs. |
| Failover Automation Level | Supports Level 0 (Manual), Level 1 (Automatic Failover), and Level 3 (Full Automation). Level 2 (Automatic Configuration Recovery) is not supported in OwlDB v1.3. |
| Standby Count | Installed: fixed at 1 / Registered: 1–9 |
| Standby Mode | Select the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Select the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether DR is used can be changed directly only for Installed DBs, and it behaves as follows depending on the direction of the change.

| Direction of Change | Behavior |
| --- | --- |
| Non-usage → Usage | Newly builds a Standby node and sets the failover automation level to Level 1 by default. |
| Usage → Non-usage | Performs Standby node cleanup. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | **Engine options** Automatically determined based on the topology selection in the step (HA → used, Single → not used) |
| Failover Automation Level | Supports Step 0 (manual), Step 3 (full automation) |
| Standby Count | 1 to 2 |
| Standby Scale In/Out | The installation DB can be adjusted directly using the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication method (fixed) |

**Standby node settings** (Displayed when DR is used)

The list of currently configured Standby nodes is displayed, and the following items can be changed. The detailed options for Log Replication Type are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>The Open Mode of the Standby node <code>Recovery</code> or <code>Read Only</code> Select from among these. (Applies to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Select the log replication method for the Standby node. (Applies to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits Redo logs generated in real time when transactions occur</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node of OpenSQL is fixedly configured with the asynchronous (ASYNC) replication method, and the Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This step verifies the configuration information for each Primary / Standby node. Depending on the topology and the installation/registration method, some items may not be retrieved, and **Backup Path**only this can be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP to be used for communication between OwlDB and nodes | Cannot be modified |
| Service Port | Port to be used for communication between OwlDB and the database server | Cannot be modified |
| Interconnect IP | Interconnect IP to be used for communication between nodes inside the cluster | Cannot be modified |
| Primary Destination IP | IP to be used for communication from the Standby DB to the Primary DB (entered only for the Primary instance) | Cannot be modified |
| Primary Destination Port | Port to be used for communication from the Standby DB to the Primary DB (entered only for the Primary instance) | Cannot be modified |
| Standby Destination IP | IP to be used for communication from the Primary DB to the Standby DB (entered only for the Standby instance) | Cannot be modified |
| Standby Destination Port | Port to be used for communication from the Primary DB to the Standby DB (entered only for the Standby instance) | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| Redo Path | Redo Path | Cannot be modified |
| Archive Path | Archive Path | Cannot be modified |
| Backup Path | Backup Path | Only a file system path can be entered |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

**Backup Path**only a file system path can be entered, and in the case of a TAC configuration **shared volume**be configured as.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP to be used for communication between OwlDB and nodes | Cannot be modified |
| Service Port | Port to be used for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | IP to be used for the replication connection in an HA configuration | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

Based on the configuration set in the previous step, the configuration information for each Primary / Standby node is displayed. During Scale Out, input fields for the added nodes are newly created, and during Scale In, the corresponding nodes are removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This step verifies the database configuration information. Most items are automatically set through OwlDB metadata and actual DB exploration data. **Common items**

| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password | Password for the database's highest-privilege administrator account (SYS user) |
| Target Memory Size | Target memory size |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |

**Tibero-specific items**

| Item | Description |
| --- | --- |
| VIP | Select whether to use VIP |
| Primary Node #N VIP | Database virtual IP (activated when VIP use is selected) |
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
| VIP | Database virtual IP (activated when a network interface is selected) |
| Shared Buffers (%) | Shared memory size |
| WAL File Size (MB) | WAL file size |
| Connection Pooler Port | Port on which OpenProxy receives client connections |

If Scale Out occurs on a DB that is using VIP, the VIP for the added nodes must be entered in this step.
{% endtab %}
{% tab title="Configuration Information Confirmation" %}
Perform a final review of the content set in the previous steps. Changed items are displayed in blue, and each item can be expanded/collapsed with an accordion. **Complete**Clicking this applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changed contents, the Complete button is disabled.
- If the DR configuration is changed to not used while a Retired instance exists in the Standby, a confirmation modal is displayed. Upon confirmation, the Retired instances are all deleted and the DR configuration is changed to not used.
{% endhint %}

{% hint style="info" %}
**Note**

**Complete** When clicked, the presence of the license file, the CP Max Core count, and whether the license options match are checked, and in the following cases the spec change does not proceed and an error message is displayed.

- When the license file does not exist on the node: Place the license file and try again.
- When the requested Core count exceeds the license's maximum Core count: Adjust the Core count and try again.
- When the requested configuration does not match the current license options: Reselect a configuration that matches the license options.
- When multiple spec change requests occur simultaneously and processing is delayed: Try again after a short while.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB <a href="#spec-change-from-detected-db" id="spec-change-from-detected-db"></a>

For registered DBs only, when the DB configuration is changed outside of OwlDB, OwlDB automatically detects this. You can check the DB in which a change was detected in the dashboard exploration and **Spec change**click this to enter.

When entering through this method, the externally changed details are automatically reflected on the spec change page. Changed items are marked separately, and the user can review the contents and then apply them to OwlDB.

**Example** When a TAC DR DB using VIP (Primary 2 nodes / Standby 1 node) is externally increased to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine options** : The 2 added nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the 2 added nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the 2 added nodes are created.

After reviewing the content and completing the input in each step, **Complete**clicking this applies the changes to OwlDB.

## Maximum processing time for spec change <a href="#spec-change-max-duration" id="spec-change-max-duration"></a>

The spec change processing time may vary depending on the database's capacity, load status, and configuration environment. In a typical environment, it is processed within a few minutes, and depending on the conditions, the maximum processing time may differ as follows.

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
| CM creation on Single Database | 30 minutes | Database shutdown |
|   | 6 hours | Adding cluster resources |
|   | 30 minutes | Starting the database |
| Backing up the primary node | 6 hours |   |
| Configuring the standby node | 6 hours |   |
| Changing primary node parameters | 30 minutes | Stopping the database and adding parameters to the tip |
|   | 30 minutes | Starting the database |
{% endtab %}
{% tab title="Standby removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
