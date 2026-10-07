This is a feature for changing the node configuration and DR configuration of an operating database.

Spec change starts from **Overview** or **Change Detection DB** and proceeds in five steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Information Review. In each step, you review the current settings and modify the necessary items, and in the final step, you review and apply the changes.

Spec change is possible when the database operation status is `Running` or `Degraded`. The range of changeable items varies depending on the DB engine (Tibero/OpenSQL) and installation method (Installed DB/Registered DB).

{% hint style="info" %}
**Note**

- In the `Degraded` state, if there is even one node in progress or unavailable, the spec change button cannot be used.
- In the `Degraded` state, if the only issue is a Failed-over Primary, the spec change button can be used.
{% endhint %}

## How to Access Spec Change <a href="#spec-change-entry" id="spec-change-entry"></a>

The spec change page can be accessed in the following two ways.

- **Access from Overview**: Overview page > Actions > click **Spec Change**
- **Access from Change Detection DB**: Dashboard > Explore > Change Detection DB > click **Spec Change**

## Spec Change <a href="#spec-change" id="spec-change"></a>

If accessed from Overview, proceed in the following order.

1. Click **Actions > Spec Change** on the Overview page to enter the spec change page.
2. In the **Engine Options** step, review the node configuration and, if necessary, set Scale In/Out.
3. In the **DR Configuration** step, adjust whether DR is used, the failover automation level, and the Standby node settings.
4. In the **Instance Configuration** step, review the information for each node and, if necessary, modify the Backup Path.
5. In the **Database Configuration** step, enter configuration information such as the VIP of the added node.
6. In the **Configuration Information Review** step, review the changes and then click **Finish**.

## Accessing Spec Change from Overview <a href="#spec-change-from-overview" id="spec-change-from-overview"></a>

When accessed through the Actions menu on the Overview page, you can perform two operations: changing the DB node configuration and modifying DR.

### Changing the DB Node Configuration <a href="#db" id="db"></a>

{% tabs %}
{% tab title="Tibero" %}
- For an **Installed DB**, Scale In/Out of the Primary node is possible within the topology range of the standard architecture. For example, a TAC configuration can adjust the Primary nodes from a minimum of 2 to a maximum of 4.
- A **Registered DB** does not support Scale In/Out.
- If you perform Scale Out, and the DB is using VIP, you must enter the VIP of the added node in the **Database Configuration** step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in spec change.
- As with a new installation, Scale In/Out is only possible for nodes whose environment has been pre-configured.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- An **Installed DB** supports Single ↔ HA topology switching, and in the HA topology, Scale In/Out of the Standby node is also supported. The adjustable number of nodes is 1 for Single and 2–3 for HA.
- A **Registered DB** does not support topology changes or Scale In/Out.
- If you perform a topology switch or Scale Out, and the DB is using VIP, you must enter the VIP of the added node in the **Database Configuration** step.
{% endtab %}
{% endtabs %}

### Modifying DR <a href="#dr" id="dr"></a>

{% tabs %}
{% tab title="Tibero" %}
- An **Installed DB** can change DR usage ↔ non-usage and can also change the failover automation level. When DR is in use, the Open Mode and Log Replication Type of the Standby node can also be changed.
- A **Registered DB** does not support changing whether DR is used, and when DR is in use, only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used is automatically determined by the topology selected in the **Engine Options** step. Switching from Single → HA automatically enables DR, and switching from HA → Single automatically disables DR.
- As described above, for an **Installed DB**, whether DR is used is changed only through topology switching, and the failover automation level can be changed separately.
- A **Registered DB** does not support changing whether DR is used; only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, refer to the [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz).
{% endhint %}

## Spec Change Steps <a href="#spec-change-steps" id="spec-change-steps"></a>

The items that can be configured in each step vary depending on the engine.

{% tabs %}
{% tab title="Engine Options" %}
This is the step for reviewing the database alias, engine, and topology information. Most items cannot be changed, and only the node configuration can be adjusted for Installed DBs. **Tibero**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | An alias for identifying the database | Cannot be changed |
| Database Engine Type | Tibero | Cannot be changed |
| Topology | Single, TAC | Cannot be changed |
| Node Count | 1 for Single, 2–4 for TAC | Cannot be changed (reflects the Scale In/Out result below) |
| Scale In/Out | Primary node list | Available only for Installed DBs |

**OpenSQL**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | An alias for identifying the database | Cannot be changed |
| Database Engine Type | OpenSQL | Cannot be changed |
| Topology | Single, HA | Installed DBs can switch Single ↔ HA |
| Node Count | 1 for Single, 2–3 for HA | Cannot be changed (reflects topology switch/Scale results) |
| PostgreSQL Version | PostgreSQL version to use | Cannot be changed |

**Scale In/Out (Tibero TAC installed DB)**

For an installed Tibero TAC topology DB, a Scale In/Out item is displayed below Node Count. You can check the currently configured Primary node list and perform the following tasks.

- **Add**: Clicking the Add button lets you select an instance from the list of hosts available for installation. The selected host is added to the table and becomes a Scale Out target.
- **Delete**: After selecting the node to remove from the table, click the Delete button. The selected node becomes a Scale In target and is dimmed in the table.
- **Reset**: Reverts added or deleted changes to their initial state.

When you switch the topology from Single → HA on an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is reviewed and adjusted in the **DR Configuration** step.
{% endtab %}
{% tab title="DR Configuration" %}
This step changes whether DR is used, the failover automation level, and the Standby node settings. The change method differs depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether to use the DR configuration. This can be changed directly only for installed DBs. |
| Failover Automation Level | Supports Level 0 (manual), Level 1 (automatic failover), and Level 3 (full automation).<br>Level 2 (automatic configuration recovery) is not supported in OwlDB v1.3 |
| Standby Count | Install: fixed at 1 / Register: 1–9 |
| Standby Mode | Select the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Select the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether DR is used can only be changed directly on installed DBs, and it behaves as follows depending on the change direction.

| Change direction | Behavior |
| --- | --- |
| Not used → Used | Newly builds a Standby node and sets the failover automation level to Level 1 by default. |
| Used → Not used | Performs Standby node cleanup. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | Automatically determined by the topology selection in the **Engine Options** step (HA → used, Single → not used) |
| Failover Automation Level | Supports Level 0 (manual) and Level 3 (full automation) |
| Standby Count | 1–2 |
| Standby Scale In/Out | Installed DBs can be adjusted directly with the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication (fixed) |

**Standby Node Settings** (shown when DR is used)

The currently configured Standby node list is displayed, and you can change the following items. The detailed options for Log Replication Type are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>Select the Open Mode of the Standby node from either <code>Recovery</code> or <code>Read Only</code>. (Applies to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Select the log replication method of the Standby node. (Applies to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that collects and transmits archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node of OpenSQL is fixed to the asynchronous (ASYNC) replication method, and Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This step reviews the configuration information for each Primary / Standby node. Some items are not retrieved depending on the topology and the install/register method, and only **Backup Path** can be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | The IP to use for communication between OwlDB and the node | Cannot be modified |
| Service Port | The Port to use for communication between OwlDB and the database server | Cannot be modified |
| Interconnect IP | The Interconnect IP to use for communication between nodes inside the cluster | Cannot be modified |
| Primary Destination IP | The IP to use for communication from the Standby DB to the Primary DB (enter only for the Primary instance) | Cannot be modified |
| Primary Destination Port | The Port to use for communication from the Standby DB to the Primary DB (enter only for the Primary instance) | Cannot be modified |
| Standby Destination IP | The IP to use for communication from the Primary DB to the Standby DB (enter only for the Standby instance) | Cannot be modified |
| Standby Destination Port | The Port to use for communication from the Primary DB to the Standby DB (enter only for the Standby instance) | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| Redo Path | Redo Path | Cannot be modified |
| Archive Path | Archive Path | Cannot be modified |
| Backup Path | Backup Path | Only a file system path can be entered |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

Only a file system path can be entered for **Backup Path**, and in the case of a TAC configuration it must be configured as a **shared volume**.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | The IP to use for communication between OwlDB and the node | Cannot be modified |
| Service Port | The Port to use for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | In an HA configuration, the IP to use for the replication connection | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

Based on the configuration set in the previous step, the configuration information for each Primary / Standby node is displayed. On Scale Out, input fields for the added node are newly created, and on Scale In, that node is removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This step reviews the database configuration information. Most items are set automatically through OwlDB metadata and actual DB exploration data. **Common Items**

| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password | Password for the database highest-privilege administrator account (SYS user) |
| Target Memory Size | Target memory size |
| Character Set | The character encoding to be used for the database |
| Timezone | The OS time zone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |

**Tibero-specific items**

| Item | Description |
| --- | --- |
| VIP | Select whether to use VIP |
| Primary Node #N VIP | Database virtual IP (enabled when VIP use is selected) |
| Shared Memory Size (MB) | Shared memory size |
| Redo Log File Size (MB) | Redo log file size |
| System Data File Size (MB) | Data file size for storing system tables and key metadata |
| Syssub Data File Size (MB) | Sub data file size for storing system operation-related data |
| User Tablespace Data File Size (MB) | Tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (MB) | Temporary tablespace data file size used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

**OpenSQL-specific items**

| Item | Description |
| --- | --- |
| VIP | Database virtual IP (enabled when a network interface is selected) |
| Shared Buffers (%) | Shared memory size |
| WAL File Size (MB) | WAL file size |
| Connection Pooler Port | The port on which OpenProxy receives client connections |

If Scale Out occurs on a DB that is using a VIP, the VIP for the added node must be entered in this step.
{% endtab %}
{% tab title="Configuration Information Confirmation" %}
This finalizes the review of the content set in the previous steps. Changed items are shown in blue, and each item can be expanded/collapsed with an accordion. Clicking **Finish** applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changes, the Finish button is disabled.
- If you change the DR configuration to not used while a Retired instance exists on the Standby, a confirmation modal is shown. Upon confirmation, all Retired instances are deleted and the DR configuration is changed to not used.
{% endhint %}

{% hint style="info" %}
**Note**

When you click **Finish**, the presence of the license file, the CP Max Core count, and whether the license options match are checked, and in the following cases the spec change does not proceed and an error message is displayed.

- When the license file does not exist on the node: Place the license file and try again.
- When the requested Core count exceeds the license maximum Core count: Adjust the Core count and try again.
- When the requested configuration does not match the current license options: Select a configuration that matches the license options and try again.
- When processing is delayed because many spec change requests occur simultaneously: Try again in a moment.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB <a href="#spec-change-from-detected-db" id="spec-change-from-detected-db"></a>

Only for registered DBs, when the DB configuration is changed outside of OwlDB, OwlDB detects this automatically. You can check the DB where a change was detected in Dashboard exploration and click **Spec Change** to enter.

When you enter this way, the changes made externally are automatically reflected on the spec change page. Changed items are indicated separately, and the user can review the content and then apply it to OwlDB.

**Example** If a TAC DR DB using a VIP (Primary 2 nodes / Standby 1 node) is externally increased to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine Options** : The 2 added nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the 2 added nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the 2 added nodes are created.

After reviewing the content and completing the input in each step, clicking **Finish** applies the changes to OwlDB.

## Maximum spec change processing time <a href="#spec-change-max-duration" id="spec-change-max-duration"></a>

The spec change processing time may vary depending on the database capacity, load state, and configuration environment. In a typical environment it is processed within a few minutes, and depending on the conditions the maximum processing time may differ as follows.

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
| Create CM on Single Database | 30 minutes | Database shutdown |
|   | 6 hours | Add cluster resource |
|   | 30 minutes | Database startup |
| Primary node backup | 6 hours |   |
| Standby node configuration | 6 hours |   |
| Primary node parameter change | 30 minutes | Database shutdown, add parameter to tip |
|   | 30 minutes | Database startup |
{% endtab %}
{% tab title="Standby removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database shutdown and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
