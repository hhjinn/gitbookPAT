Provides summary information and alerts on the status of all operating databases and instances. **OwlDB Console Screen > OwlDB Logo**Click to view the dashboard.

> 📷 **[이미지]** 이미지

# Checking the overall status summary

For all operating databases/instances, check the top-level status value (**Status**) and the detailed status value (**Health**). Clicking each status lets you view the '[Checking the DB Service/instance list](#db-service인스턴스-목록-확인)' where you can see the list of DB Services/instances with that status value.

{% tabs %}
{% tab title="Status" %}
The top-level Status is displayed per database.

- Because the status can change per instance node, the count is displayed next to the status in the form (n/m). n: number of available instance nodes m: total number of instance nodes
- Types written in bold with a `*`appended can be filtered on the dashboard.

| Type | Description |
| --- | --- |
| Deploying | Installing new resources |
| Registering | Registering new resources |
| **Running** * | Database available for normal use |
| **Updating** * | Applying planned tasks/changes to the database (e.g., full restart, role switch, spec change, migration, recovery, etc.) |
| **Degraded** * | Some databases/components unavailable (e.g., individual instance restart, etc.) |
| **Failover** * | Performing Auto Failover due to database failure detection (occurs when the entire Primary DB cluster is Unavailable) |
| **Down** * | Entire database unavailable (both Primary and Standby unavailable) |
| Starting | Transitioning from Stopped to Running |
| Stopping | Transitioning from Running to Stopped (full DB shutdown) |
| Stopped | All resources temporarily unused (resources deactivated) |
| Unregistering | Unregistering a registered DB Service (excluded from management targets upon completion) |
| Terminating | Permanently deleting all resources/data (access/recovery impossible upon completion) |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.
{% endtab %}
{% tab title="Health" %}
The detailed status Health is displayed per instance node.

- It is displayed by combining the DB Boot Mode, Instance, cluster management tool (hereafter CMT), and Agent connection status.
- The features restricted according to the message can be checked via a banner when entering each menu.

The Health determination criteria for each engine are as follows.

- Tibero: Determined by combining the DB Boot Mode, cluster management tool (CMT), and Agent status.
- OpenSQL: Determined by combining the Patroni, OpenProxy, etcd, and Agent status.

{% tabs %}
{% tab title="Tibero" %}
| Display | Status | Message | Description |
| --- | --- | --- | --- |
| 🟢 Available | available | - | Node normal |
| 🟡 Limited | limited | Issue: CMT Inactive | Cluster management tool (CMT) abnormal |
|   | limited | Issue: Mount Mode | Node started in Mount mode |
| 🔴 Unavailable | unavailable | Issue: Nomount Mode | Node started in Nomount mode |
|   | unavailable | Issue: DB Down | Node DB Down |
|   | unavailable | Issue: Agent Disconnect | Agent connection lost with the VM where the node is located |
|   | unavailable | Issue: VM Down | Node location VM Down |
| ⚫ Retired | retired | Issue: Failovered Primary | Abnormal termination of (old) Primary during Failover |
{% endtab %}
{% tab title="OpenSQL" %}
The Health of an OpenSQL instance node is determined by combining the Patroni, OpenProxy, etcd, and Agent statuses.

| Display | Status | Message | Description |
| --- | --- | --- | --- |
| 🟢 Available | available | - | Node normal |
| 🟡 Limited | limited | Issue: etcd Inactive | etcd abnormal |
|   | limited | Issue: OpenProxy Inactive | OpenProxy abnormal |
| 🔴 Unavailable | unavailable | Issue: DB Down | DB Down due to Patroni abnormality |
|   | unavailable | Issue: Agent Disconnect | Connection lost between the node location VM and Agent |
{% endtab %}
{% endtabs %}

`🔵 In Progress` The status applies commonly to both Tibero and OpenSQL.

| Level | Message |
| --- | --- |
| Individual instance node | Reboot, Switchover, Failover, Rebuilding, Modify Spec (TAC Scale In/Out) |
| All instances | Modify Spec (Scale Up/Down), Migration, Restoring, Patch/Upgrade, Starting, Stopping |

{% hint style="info" %}
**Note**

A cluster management tool is a component required to operate databases in a cluster configuration. (e.g., Tibero Cluster Manager(CM), Tibero Active Storage(TAS), etc.)
{% endhint %}
{% endtab %}
{% endtabs %}

# Checking the DB Service/instance list

Check the list of all DB Services and their subordinate instances. '[Checking the overall status summary information](#전체-상태-요약-정보-확인)' The displayed list of DB Services and instances varies depending on the status value selected in '

- You can perform a restart operation on the subordinate instances of the selected database.
- You can perform start, stop, delete, role switch, and license renewal management operations on the selected database.
- Provides a search function for database/instance aliases.

{% hint style="info" %}
**Note**

You can perform various management operations on the DB Service. For details, see **Operations** You can check this on the page.
{% endhint %}

### Switching the list view (Card View/List View)

You can view the DB Service/instance list in two ways: card view and list view. When switching views, the sort function is reset, but filtering is maintained.

{% tabs %}
{% tab title="Card View" %}
The card view organizes and displays the key information of a DB Service in the form of individual cards so that it is easy to grasp visually. Up to 4 cards are displayed in a row, and the size of each card is fixed. If the content exceeds the fixed size, internal scrolling is enabled. In the last slot of the list of currently operating DB Service cards, `➕` An icon is displayed, and clicking it moves to the DB Service creation page.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service information</td><td>Displays a summary of the key information of the database at the top of the card<br>Display items<ul><li>DB Type Logo</li><li>DB Service Name</li><li>Status</li><li>DB Type</li><li>DB version (in the case of OpenSQL, the OpenSQL version and PostgreSQL version are displayed simultaneously)</li><li>Topology</li><li>Eventlog : I / W / E (Info / Warning / Error respectively)</li></ul></td></tr><tr><td>Node information</td><td>Displays the instance information associated under the database. Can be collapsed/expanded in accordion form based on Role<br>Sort rules<ul><li>Tibero: displayed in the order Primary → Standby(Read Only) → Standby(Recovery)</li><li>OpenSQL: displayed in the order Leader → Replica</li><li>Sorted by AZ within the same Role</li></ul>Display items<ul><li>Role : displays the number of instances of that role</li><li>Data volume (Tibero) / Volume (OpenSQL): displays usage (%), bar chart, and Threshold</li></ul>Table items<ul><li>Instance alias</li><li>Health</li><li>vCPU: current CPU usage (%)</li><li>Memory: current memory usage (%)</li><li>Active sessions: current number of active sessions / Max Session Count (not displayed for Standby(Recovery))</li><li>Availability Zone: AZ information where the instance is located</li></ul></td></tr><tr><td>Cluster information</td><td>Displays Connection Health as a cluster diagram<ul><li>Single: Displays only Primary/Leader DB information</li><li>Single +DR / HA: Displays Primary/Leader DB and Standby/Replica DB information. Displays P-S connection status monitoring information; Standby/Replica DBs are each created per AZ</li><li>Tibero TAC: Displays multiple nodes inside the Primary DB Box</li><li>Tibero TAC + DR: Displays Primary DB and Standby DB information. Displays P-S connection status monitoring information; multiple nodes are displayed inside the Primary DB Box; Standby DBs are each created per AZ</li><li>DR configuration connection status monitoring: Represents the connection status between Primary - Standby / Leader - Replica using line color and displayed information</li><li>Normal: green,<code>Connected</code></li><li>Failed: red,<code>Disconnected</code></li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="List View" %}
The list view displays database and sub-instance information in a tree-form table to make the hierarchical structure easy to understand.

{% hint style="info" %}
**Note**

'Default value' refers to items exposed by default on the initial screen, and 'Required value' refers to items that cannot be set to hidden.
{% endhint %}

<table data-full-width="true"><thead><tr><th>Column</th><th>Description</th><th>DB Service Level</th><th>Instance Level</th><th>Default Value</th><th>Required Value</th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Name display<ul><li>Clicking the DB Service name navigates to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/doc/5d7daeb2-e4f4-4409-aa74-dcaf1d65ab46/README.md">Service Meta Information Lookup</a>' page</li><li>Clicking the instance alias navigates to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/doc/dF57s45IXBUgU7RX1UvL/README.md">Instance Management</a>' page</li></ul></td><td>DB Service name</td><td>Instance alias</td><td>O</td><td>O</td></tr><tr><td><strong>Creation date</strong></td><td>Displays the DB Service creation date in <code>yyyy.mm.dd HH:mm:ss</code> format</td><td>DB Service creation date</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Status</strong></td><td>Displays Status or Health</td><td><ul><li>Creating</li><li>Running(n/m)</li><li>Updating(n/m)</li><li>Degraded(n/m)</li><li>Failover(n/m)</li><li>Down(n/m)</li><li>Starting</li><li>Stopping</li><li>Stopped</li><li>Terminating</li><li>Deploying (on-premise)</li><li>Registering (on-premise)</li><li>Unregistering (on-premise)</li></ul></td><td><ul><li>🟢 (available)</li><li>🟡 (limited)</li><li>🔵 (in progress)</li><li>🔴 (unavailable)</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Type</strong></td><td>Displays the database engine type</td><td><ul><li>Tibero</li><li>OpenSQL</li></ul></td><td>-</td><td>O</td><td>X</td></tr><tr><td><strong>Configuration</strong></td><td>Displays the topology or role</td><td><ul><li>Tibero: Single, TAC(+DR)</li><li>OpenSQL: Single, HA</li></ul></td><td><ul><li>Tibero: Primary, Standby(Read Only), Standby(Recovery)</li><li>OpenSQL: Leader, Replica</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Availability Zone (Cloud)</strong></td><td>In a Cloud environment, displays the Availability Zone (AZ) information where the instance is located</td><td>-</td><td>Availability Zone (AZ) information</td><td>O</td><td>X</td></tr><tr><td><strong>vCPU</strong></td><td>Displays the CPU count and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Memory</strong></td><td>Displays Memory and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Active Sessions</strong></td><td>Displays the number of currently active sessions as a bar chart<ul><li>Displayed only for Primary / Standby(RO) / Leader / Replica</li><li>Standby(Recovery) :<code>-</code>Displayed as</li></ul></td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Data Volume (Tibero/OpenSQL)</strong></td><td>Displays data volume usage as a bar chart + displays Threshold</td><td>O</td><td>X</td><td>O</td><td>X</td></tr><tr><td><strong>Redo log Volume (Tibero)</strong></td><td>Displays redo log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Archive log Volume (Tibero)</strong></td><td>Displays archive log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Root Volume</strong></td><td>Displays Root volume usage as a bar chart</td><td>O</td><td>O</td><td>X</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### DB Service List User Settings

The ⚙️ at the top right of the DB Service List screen **Settings** Clicking the icon allows you to configure the screen display method to suit your user environment.

- Card View : Configures the information area displayed on the card.
- List View : Configures the columns displayed in the list.

The configured settings are saved per user and are applied identically thereafter.

{% tabs %}
{% tab title="Card View" %}
In Card View, you can select the information area to display on the card.

1. On the DB Service List screen, ⚙️ **Settings**Click.
2. **Configuration**Turn the items to display ON/OFF.
3. On the right **Preview**Check the changes in advance.
4. **Save**Click to apply.

### Configuration Items

| Item | Description |
| --- | --- |
| DB Service Info | Displays DB Service basic information |
| Node Info | Displays status and resource information per node |
| Cluster Info | Displays Cluster information and Connection Health |

{% hint style="info" %}
**Note**

You can check the change results in real time in the Preview area on the right. Changes are not applied to the actual screen until saved.
{% endhint %}
{% endtab %}
{% tab title="List View" %}
In List View, you can select the columns to display in the list or change the column order.

1. On the DB Service List screen, ⚙️ **Settings**Click.
2. Toggle the columns to display ON/OFF.
3. Change the column order via Drag & Drop.
4. **Save**Click to apply.

### Key Features

| Feature | Description |
| --- | --- |
| Show/Hide Columns | Set whether to display columns using switches |
| Column Search | Quickly find the desired item by searching for the column name |
| Change Column Order | Drag to change to the desired order |
| Reset to Default | Restore to the default column configuration |
{% endtab %}
{% endtabs %}

# Top N Chart View

On the dashboard screen, view the top instances by CPU Usage, Memory Usage, and Session Load as charts for the DB Services for which you have view permission. The Top N chart is displayed in the right area of the dashboard, and data is automatically configured according to the view permission scope of the logged-in user without any separate settings. If there is no DB Service for which you have view permission, a no-data state is displayed in the chart area.

{% hint style="info" %}
**Note**

**Differences in Supported Engines by Environment**

- AWS environment: Only Tibero engine instances are included in the view scope.
- Azure environment: Both Tibero and OpenSQL engine instances are included in the view scope.
{% endhint %}

### Displayed Metrics

<table data-full-width="true"><thead><tr><th>Metric</th><th>Description</th></tr></thead><tbody><tr><td>CPU Usage</td><td>Chart of the top 5 instances by CPU usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li></ul></td></tr><tr><td>Memory Usage</td><td>Chart of the top 5 instances by Memory usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li></ul></td></tr><tr><td>Session Load</td><td>Chart of the top 5 instances by Session Load (%)<ul><li>A metric that represents the ratio of Active Session relative to Max Session</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li><li>Tooltip: (Active Session/Max Session)X100</li></ul></td></tr></tbody></table>

### Expand/Collapse Chart Area

Clicking the button at the top of the Top N chart area collapses or expands the chart area.

- The default state is expanded.
- If the user changes it to the collapsed state, the collapsed state is maintained even if they navigate to another screen and return within the same session.
- When you log out or the session ends and you connect with a new session, it is reset to the expanded state.
