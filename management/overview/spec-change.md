This is a feature for changing the node configuration and DR configuration of an operating database.

Specification change is **Overview** or **Change Detection DB**It starts from and proceeds through five steps: Engine Options → DR Configuration → Instance Configuration → Database Configuration → Configuration Review. In each step, you check the current settings and modify the necessary items, and in the final step, you perform a final review of the changes and apply them.

Spec changes are possible when the database operating status is `Running` or `Degraded`. The range of items that can be changed varies depending on the DB engine (Tibero/OpenSQL) and installation method (Installed DB/Registered DB).

{% hint style="info" %}
**Note**

- `Degraded` If there is even one node that is in progress or unavailable in the state, the spec change button cannot be used.
- `Degraded` If the state exists and only the issue caused by the Failover Primary exists, the spec change button can be used.
{% endhint %}

## How to Access Spec Change <a href="#spec-change-entry" id="spec-change-entry"></a>

The spec change page can be accessed in the following two ways.

- **Access from Overview** : Overview page > Tasks > **Spec Change** Click
- **Access from Change Detection DB** : Dashboard > Explore > Change Detection DB > **Spec Change** Click

## Spec Change <a href="#spec-change" id="spec-change"></a>

When accessing from Overview, proceed in the following order.

1. On the Overview page, **Tasks > Spec Change**Click to enter the spec change page.
2. **Engine options** In the step, check the node configuration and set up Scale In/Out if necessary.
3. **DR configuration** In the step, adjust whether DR is used, the failover automation level, and the Standby node settings.
4. **Instance Configuration** In the step, check the information for each node and modify the Backup Path if necessary.
5. **Database Configuration** In the step, enter configuration information such as the VIP of the added node.
6. **Configuration Information Review** In the step, after reviewing the changes, **Complete**Click.

## Accessing Spec Change from Overview <a href="#spec-change-from-overview" id="spec-change-from-overview"></a>

When accessing through the Tasks menu on the Overview page, you can perform two tasks: DB node configuration change and DR modification.

### DB Node Configuration Change <a href="#db" id="db"></a>

{% tabs %}
{% tab title="Tibero" %}
- **Installation DB**allows Scale In/Out of the Primary node within the topology range of the standard architecture. For example, in a TAC configuration, the Primary nodes can be adjusted from a minimum of 2 to a maximum of 4.
- **Registration DB**does not support Scale In/Out.
- If Scale Out has been performed and the DB is using VIP, **Database Configuration** you must enter the VIP of the node added in the step.

{% hint style="info" %}
**Note**

- Changing the topology itself (e.g., Single → TAC) is not supported in spec change.
- Scale In/Out is only possible for nodes where the prior environment configuration has been completed, the same as a new installation.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
- **Installation DB**allows Single ↔ HA topology switching, and in the HA topology, Scale In/Out of the Standby node is also supported. The number of adjustable nodes is 1 for Single and 2 to 3 for HA.
- **Registration DB**does not support topology changes or Scale In/Out.
- If topology switching or Scale Out has been performed and the DB is using VIP, **Database Configuration** you must enter the VIP of the node added in the step.
{% endtab %}
{% endtabs %}

### DR Modification <a href="#dr" id="dr"></a>

{% tabs %}
{% tab title="Tibero" %}
- **Installation DB**can change DR usage ↔ non-usage, and can also change the failover automation level. When DR is in use, the Open Mode and Log Replication Type of the Standby node can also be changed.
- **Registration DB**does not support changing whether DR is used, and when DR is in use, only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% tab title="OpenSQL" %}
- Whether DR is used is **Engine options** automatically determined according to the topology selected in the step. Switching from Single → HA automatically enables DR, and switching from HA → Single automatically disables DR.
- **Installation DB**As described above, DR usage is only changed through topology switching, and the failover automation level can be changed separately.
- **Registration DB**does not support changing whether DR is used, and only the failover automation level and Standby node settings can be changed.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

For the supported failover automation levels, [**Failover Automation document**](#GDPaQdLZBmgq4vRqB2Sz)please refer to.
{% endhint %}

## Spec Change Steps <a href="#spec-change-steps" id="spec-change-steps"></a>

The items that can be configured in each step vary depending on the engine.

{% tabs %}
{% tab title="Engine options" %}
This is the step to check the database alias, engine, and topology information. Most items cannot be changed, and only the node configuration can be adjusted for Installed DBs only. **Tibero**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias to identify the database | Cannot be changed |
| Database Engine Type | Tibero | Cannot be changed |
| Topology | Single, TAC | Cannot be changed |
| Node Count | 1 for Single, 2 to 4 for TAC | Cannot be changed (reflects the Scale In/Out result at the bottom) |
| Scale In/Out | Primary node list | Available for Installed DBs only |

**OpenSQL**

| Item | Description | Whether changeable |
| --- | --- | --- |
| Database Alias | Alias to identify the database | Cannot be changed |
| Database Engine Type | OpenSQL | Cannot be changed |
| Topology | Single, HA | Installed DBs can switch between Single ↔ HA |
| Node Count | 1 for Single, 2 to 3 for HA | Cannot be changed (reflects topology switching/Scale result) |
| PostgreSQL Version | PostgreSQL version to use | Cannot be changed |

**Scale In/Out (Tibero TAC Installed DB)**

In the case of an installed Tibero TAC topology DB, the Scale In/Out item is displayed below Node Count. You can check the currently configured Primary node list and perform the following tasks.

- **Add** : Clicking the Add button allows you to select an instance from the list of installable hosts. The selected host is added to the table and becomes a Scale Out target.
- **Delete** : After selecting the node to remove from the table, click the Delete button. The selected node becomes a Scale In target and is displayed grayed out in the table.
- **Reset** : Reverts the added or deleted changes to their initial state.

When switching the topology from Single → HA in an installed OpenSQL DB, the DR configuration is automatically enabled, and the Standby node configuration is **DR configuration** checked and adjusted in the step.
{% endtab %}
{% tab title="DR configuration" %}
This is the step to change whether DR is used, the failover automation level, and the Standby node settings. The change method varies depending on the engine. **Tibero**

| Item | Description |
| --- | --- |
| Enable DR | Whether the DR configuration is used. It can be changed directly for Installed DBs only. |
| Failover Automation Level | Level 0 (manual), Level 1 (automatic failover), and Level 3 (full automation) are supported.<br>Level 2 (automatic configuration recovery) is not supported in OwlDB v1.3 |
| Standby Count | Installed: fixed at 1 / Registered: 1 to 9 |
| Standby Mode | Select the Open Mode of the Standby node (Recovery / Read Only) |
| Log Replication Type | Select the log transmission method of the Standby node (LGWR ASYNC / ARCH ASYNC) |

Whether DR is used can only be changed directly in Installed DBs, and it operates as follows depending on the direction of change.

| Direction of Change | Behavior |
| --- | --- |
| Non-usage → Usage | A Standby node is newly built, and the failover automation level is set to the default Level 1. |
| Usage → Non-usage | The Standby node cleanup is performed. |

**OpenSQL**

| Item | Description |
| --- | --- |
| Enable DR | **Engine options** Automatically determined based on the topology selection in the step (HA → used, Single → not used) |
| Failover Automation Level | Supports Stage 0 (manual) and Stage 3 (fully automated) |
| Standby Count | 1–2 |
| Standby Scale In/Out | The installation DB can be adjusted directly using the Add/Delete buttons |
| Log Replication Type | Configured with asynchronous (ASYNC) replication (fixed) |

**Standby node settings** (Shown when DR is used)

The list of currently configured Standby nodes is displayed, and the following items can be changed. The detailed options for Log Replication Type are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Open Mode</td><td>The Open Mode of the Standby node <code>Recovery</code> or <code>Read Only</code> Select from among these. (Applies to Tibero)</td></tr><tr><td>Log Replication Type</td><td>Select the log replication method for the Standby node. (Applies to Tibero)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong> : A replication mode that collects and transmits the archive log files generated after a log switch</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Standby node of OpenSQL is fixed to the asynchronous (ASYNC) replication method, and Open Mode and Log Replication Type cannot be selected.
{% endhint %}
{% endtab %}
{% tab title="Instance Configuration" %}
This is the step for checking the configuration information of each Primary / Standby node. Depending on the topology and the installation/registration method, some items may not be queried, and **Backup Path**only this can be modified.

**Tibero**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP used for communication between OwlDB and the node | Cannot be modified |
| Service Port | Port used for communication between OwlDB and the database server | Cannot be modified |
| Interconnect IP | Interconnect IP used for communication between nodes within the cluster | Cannot be modified |
| Primary Destination IP | IP used for communication from the Standby DB to the Primary DB (enter for Primary instances only) | Cannot be modified |
| Primary Destination Port | Port used for communication from the Standby DB to the Primary DB (enter for Primary instances only) | Cannot be modified |
| Standby Destination IP | IP used for communication from the Primary DB to the Standby DB (enter for Standby instances only) | Cannot be modified |
| Standby Destination Port | Port used for communication from the Primary DB to the Standby DB (enter for Standby instances only) | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| Redo Path | Redo Path | Cannot be modified |
| Archive Path | Archive Path | Cannot be modified |
| Backup Path | Backup Path | Only a file system path can be entered |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

**Backup Path**only a file system path can be entered, and in the case of a TAC configuration **a shared volume**be configured as.

**OpenSQL**

| Item | Description | Remarks |
| --- | --- | --- |
| Hostname | Connected host information | Cannot be modified |
| Service IP | IP used for communication between OwlDB and the node | Cannot be modified |
| Service Port | Port used for communication between OwlDB and the database server | Cannot be modified |
| Replication Connection Ip | IP used for the replication connection in an HA configuration | Cannot be modified |
| Network interface | Network interface | Cannot be modified |
| Data Path | Data Path | Cannot be modified |
| SSH Port | SSH Port | Cannot be modified |
| SSH User | SSH User | Cannot be modified |
| SSH Key File Path | SSH Key File Path | Cannot be modified |

The configuration information for each Primary / Standby node is displayed according to the configuration set in the previous step. When scaling out, input fields for the added nodes are newly created, and when scaling in, the corresponding nodes are removed from the screen.
{% endtab %}
{% tab title="Database Configuration" %}
This is the step for checking the database configuration information. Most items are set automatically through OwlDB metadata and actual DB exploration data. **Common items**

| Item | Description |
| --- | --- |
| Database Name | The name of the database to be used |
| SYS User Password | Password for the database super-privileged administrator account (SYS user) |
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
| Syssub Data File Size (MB) | The size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (MB) | Tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (MB) | The size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (MB) | Undo tablespace size |

**OpenSQL-specific items**

| Item | Description |
| --- | --- |
| VIP | Database virtual IP (enabled when a network interface is selected) |
| Shared Buffers (%) | Shared memory size |
| WAL File Size (MB) | WAL file size |
| Connection Pooler Port | Port on which OpenProxy accepts client connections |

If Scale Out occurs in a DB using VIP, the VIP for the added node must be entered in this step.
{% endtab %}
{% tab title="Configuration Information Review" %}
Perform a final review of the settings configured in the previous steps. Changed items are shown in blue, and each item can be expanded/collapsed with an accordion. **Complete**Clicking this applies the spec change.

{% hint style="warning" %}
**Caution**

- If there are no changes, the Complete button is disabled.
- If DR configuration is changed to not used while a Retired instance exists in the Standby, a confirmation modal is shown. Upon confirmation, all Retired instances are deleted and the DR configuration is changed to not used.
{% endhint %}

{% hint style="info" %}
**Note**

**Complete** When clicked, it checks for the presence of a license file, the CP Max Core count, and whether the license options match; in the following cases, the spec change does not proceed and an error message is displayed.

- When no license file exists on the node: place the license file and try again.
- When the requested Core count exceeds the maximum Core count of the license: adjust the Core count and try again.
- When the requested configuration does not match the current license options: select a configuration that matches the license options and try again.
- When processing is delayed because multiple spec change requests occur simultaneously: try again later.
{% endhint %}
{% endtab %}
{% endtabs %}

## Entering spec change from a change-detected DB <a href="#spec-change-from-detected-db" id="spec-change-from-detected-db"></a>

For registered DBs only, OwlDB automatically detects when the DB configuration is changed outside of OwlDB. Check the DB in which the change was detected in the dashboard exploration and **Spec Change**click this to enter.

When entering this way, the externally changed details are automatically reflected on the spec change page. Changed items are marked separately, and the user can review the content and then apply it to OwlDB.

**Example** If a TAC DR DB using VIP (2 Primary nodes / 1 Standby node) is externally expanded to 4 Primary nodes, it is reflected in each step as follows.

1. **Engine options** : The 2 added nodes are displayed in the list.
2. **Instance Configuration** : Fields for entering the configuration information of the 2 added nodes are created.
3. **Database Configuration** : Fields for entering the VIP of the 2 added nodes are created.

After reviewing the content and completing the input in each step, **Complete**clicking this reflects the changes in OwlDB.

## Maximum spec change processing time <a href="#spec-change-max-duration" id="spec-change-max-duration"></a>

The spec change processing time may vary depending on the database capacity, load status, and configuration environment. In a typical environment, it is processed within a few minutes, and depending on the conditions, the maximum processing time may differ as follows.

{% tabs %}
{% tab title="Primary creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database configuration | 6 hours | Script execution |
{% endtab %}
{% tab title="Primary removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database stop and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% tab title="Standby creation" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| CM creation on Single Database | 30 minutes | Database stop |
|   | 6 hours | Add cluster resource |
|   | 30 minutes | Start Database |
| Back up Primary node | 6 hours |   |
| Configure Standby node | 6 hours |   |
| Change Primary node parameters | 30 minutes | Stop Database and add parameters to tip |
|   | 30 minutes | Start Database |
{% endtab %}
{% tab title="Standby removal" %}
| Logic | Maximum processing time | Remarks |
| --- | --- | --- |
| Database stop and cluster resource removal | 30 minutes |   |
| Environment cleanup | 5 minutes |   |
{% endtab %}
{% endtabs %}
