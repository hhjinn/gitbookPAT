Provides summary information and notifications about the status of all databases and instances in operation. You can view the dashboard by clicking **OwlDB console screen > OwlDB logo**.

<figure>
<img src="../.gitbook/assets/image-dbea59f1.png" alt="">
<figcaption>Figure 1. Dashboard</figcaption>
</figure>

# Checking the overall status summary <a href="#status-summary" id="status-summary"></a>

Check the top-level status value (**Status**) and detailed status value (**Health**) for all databases/instances in operation. Clicking each status lets you view the list of DB Services/instances with that status value in '[Checking the DB Service/instance list](#service-instance-list)'.

{% tabs %}
{% tab title="Status" %}
Status, the top-level status, is displayed on a per-database basis.

- Because the status can change for each instance node, the count is displayed next to the status in the form (n/m). n: Number of available instance nodes m: Total number of instance nodes
- Types marked in Bold with a `*` appended can be filtered on the dashboard.

| Type | Description |
| --- | --- |
| Provisioning | Creating a new resource |
| **Running** * | Database available for normal use |
| **Updating** * | Applying planned operations or changes to the database (e.g., full restart, role switchover, spec change, migration, recovery, etc.) |
| **Degraded** * | Some databases or components unavailable (e.g., individual instance restart, etc.) |
| **Failover** * | The system has detected a database failure and is performing Auto Failover (occurs when the entire Primary DB cluster becomes Unavailable) |
| **Down** * | Entire database unavailable (both Primary and Standby unavailable) |
| Starting | Transitioning from Stopped to Running |
| Stopping | Transitioning from Running to Stopped (rolling back transactions and stopping processes, then transitioning to VM shutdown) |
| Stopped | All resources temporarily not in use (resources disabled) |
| Terminating | Permanently deleting all resources and data (access and recovery not possible upon completion) |

The * mark indicates a required input item.
{% endtab %}
{% tab title="Health" %}
Health, the detailed status, is displayed per instance node.

- Displayed by combining DB Boot Mode, Instance, Cluster Management Tool (hereinafter CMT), and Agent connection status.
- Features that are restricted according to the message can be checked via a banner when entering each menu.

The Health determination criteria for each engine are as follows.

- Tibero: Determined by combining DB Boot Mode, Cluster Management Tool (CMT), and Agent status.
- OpenSQL: Determined by combining Patroni, OpenProxy, etcd, and Agent status.

{% tabs %}
{% tab title="Tibero" %}
| Display | Status | Message | Description |
| --- | --- | --- | --- |
| 🟢 Available | available | - | Node normal |
| 🟡 Limited | limited | Issue: CMT Inactive | Cluster Management Tool (CMT) abnormal |
|   | limited | Issue: Mount Mode | Node started in Mount mode |
| 🔴 Unavailable | unavailable | Issue: Nomount Mode | Node started in Nomount mode |
|   | unavailable | Issue: DB Down | Node DB Down |
|   | unavailable | Issue: Agent Disconnect | Agent connection lost with the VM where the node is located |
|   | unavailable | Issue: VM Down | VM where the node is located Down |
| ⚫ Retired | retired | Issue: Failovered Primary | Performing Failover; (old) Primary terminated abnormally |
{% endtab %}
{% tab title="OpenSQL" %}
The Health of an OpenSQL instance node is determined by combining Patroni, OpenProxy, etcd, and Agent status.

| Display | Status | Message | Description |
| --- | --- | --- | --- |
| 🟢 Available | available | - | Node normal |
| 🟡 Limited | limited | Issue: etcd Inactive | etcd abnormal |
|   | limited | Issue: OpenProxy Inactive | OpenProxy abnormal |
| 🔴 Unavailable | unavailable | Issue: DB Down | DB Down due to Patroni abnormal |
|   | unavailable | Issue: Agent Disconnect | Agent connection lost with the VM where the node is located |
{% endtab %}
{% endtabs %}

The `🔵 In Progress` status applies commonly to both Tibero and OpenSQL.

| Level | Message |
| --- | --- |
| Individual instance node | Reboot, Switchover, Failover, Rebuilding, Modify Spec (TAC Scale In/Out) |
| All instances | Modify Spec (Scale Up/Down), Migration, Restoring, Patch/Upgrade, Starting, Stopping |

{% hint style="info" %}
**Note**

A Cluster Management Tool is a component required to operate a database in a cluster configuration. (e.g., Tibero Cluster Manager (CM), Tibero Active Storage (TAS), etc.)
{% endhint %}
{% endtab %}
{% endtabs %}

# Checking the DB Service/instance list <a href="#service-instance-list" id="service-instance-list"></a>

Check the list of all DB Services and their sub-instances. The DB Service and instance list displayed varies according to the status value selected in '[Checking the overall status summary](#status-summary)'.

- You can perform restart operations on the sub-instances of the selected database.
- You can perform start, stop, delete, role switchover, and license renewal management operations on the selected database.
- Provides a search function for database/instance aliases.

{% hint style="info" %}
**Note**

You can perform various management operations on the DB Service. For details, see the **Operations** page.
{% endhint %}

### List view toggle (card view/list view) <a href="#undefined-2" id="undefined-2"></a>

You can view the DB Service/instance list in two ways: card view and list view. When switching views, the sorting function is reset, but filtering is retained.

{% tabs %}
{% tab title="Card View" %}
Card view organizes and displays the key information of a DB Service in individual card form so it is easy to grasp visually. Up to 4 cards are displayed per row, and the size of each card is fixed. If the content exceeds the fixed size, internal scrolling is enabled. A `➕` icon is displayed in the last slot of the currently operating DB Service card list, and clicking it navigates to the DB Service creation page.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service information</td><td>Displays a summary of the key database information at the top of the card<br>Displayed items<ul><li>DB Type Logo</li><li>DB Service Name</li><li>Status</li><li>DB Type</li><li>DB version (for OpenSQL, the OpenSQL version and PostgreSQL version are displayed simultaneously)</li><li>Topology</li><li>Eventlog: I / W / E (Info / Warning / Error respectively)</li></ul></td></tr><tr><td>Node information</td><td>Displays the information of instances linked under the database. Can be collapsed/expanded in accordion form based on Role<br>Sorting rules<ul><li>Tibero: Displayed in the order Primary → Standby(Read Only) → Standby(Recovery)</li><li>OpenSQL: Displayed in the order Leader → Replica</li><li>Sorted by AZ within the same Role</li></ul>Displayed items<ul><li>Role : Displays the number of instances of that role</li><li>Data volume (Tibero) / Volume (OpenSQL): Displays usage (%), a bar chart, and Threshold</li></ul>Table items<ul><li>Instance alias</li><li>Health</li><li>vCPU: Current CPU usage (%)</li><li>Memory: Current memory usage (%)</li><li>Active sessions: Current number of active sessions / Max Session Count (not displayed for Standby(Recovery))</li><li>Availability zone: Information on the AZ where the instance is located</li></ul></td></tr><tr><td>Cluster information</td><td>Displays Connection Health as a cluster diagram<ul><li>Single: Displays only Primary/Leader DB information</li><li>Single +DR / HA: Displays Primary/Leader DB and Standby/Replica DB information. Displays P-S connection status monitoring information; Standby/Replica DBs are each created per AZ</li><li>Tibero TAC: Display multiple nodes within the Primary DB Box</li><li>Tibero TAC + DR: Display Primary DB and Standby DB information. Display P-S connection status monitoring information, display multiple nodes within the Primary DB Box, Standby DB is created separately per AZ</li><li>DR Configuration Connection Status Monitoring: Represents the connection status between Primary - Standby / Leader - Replica using line color and displayed information</li><li>Normal: green, <code>Connected</code></li><li>Failed: red, <code>Disconnected</code></li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="List View" %}
The list view displays database and sub-instance information in a tree-structured table for easy understanding of the hierarchical structure.

{% hint style="info" %}
**Note**

'Default' refers to items exposed by default on the initial screen, and 'Required' refers to items that cannot be hidden.
{% endhint %}

<table><thead><tr><th>Column</th><th>Description</th><th>DB Service Level</th><th>Instance Level</th><th>Default</th><th>Required</th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Name Display<ul><li>Clicking the DB Service name navigates to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/TgpIeVAAe6TUtfrjaD2G/doc/5d7daeb2-e4f4-4409-aa74-dcaf1d65ab46/README.md">Service Meta Information Lookup</a>' page</li><li>Clicking the instance alias navigates to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/TgpIeVAAe6TUtfrjaD2G/doc/dF57s45IXBUgU7RX1UvL/README.md">Instance Management</a>' page</li></ul></td><td>DB Service Name</td><td>Instance alias</td><td>O</td><td>O</td></tr><tr><td><strong>Creation Date</strong></td><td>Displays the DB Service creation date in <code>yyyy.mm.dd HH\:mm:ss</code> format</td><td>DB Service Creation Date</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Status</strong></td><td>Status or Health Display</td><td><ul><li>Creating</li><li>Running(n/m)</li><li>Updating(n/m)</li><li>Degraded(n/m)</li><li>Failover(n/m)</li><li>Down(n/m)</li><li>Starting</li><li>Stopping</li><li>Stopped</li><li>Terminating</li><li>Deploying (on-premise)</li><li>Registering (on-premise)</li><li>Unregistering (on-premise)</li></ul></td><td><ul><li>🟢 (available)</li><li>🟡 (limited)</li><li>🔵 (in progress)</li><li>🔴 (unavailable)</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Type</strong></td><td>Display database engine type</td><td><ul><li>Tibero</li><li>OpenSQL</li></ul></td><td>-</td><td>O</td><td>X</td></tr><tr><td><strong>Configuration</strong></td><td>Display topology or role</td><td><ul><li>Tibero: Single, TAC(+DR)</li><li>OpenSQL: Single, HA</li></ul></td><td><ul><li>Tibero: Primary, Standby(Read Only), Standby(Recovery)</li><li>OpenSQL: Leader, Replica</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Availability Zone (Cloud)</strong></td><td>In a Cloud environment, displays the availability zone (AZ) information where the corresponding instance is located</td><td>-</td><td>Availability Zone (AZ) Information</td><td>O</td><td>X</td></tr><tr><td><strong>vCPU</strong></td><td>Display CPU count and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Memory</strong></td><td>Display Memory and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Active Sessions</strong></td><td>Display the number of currently active sessions as a bar chart<ul><li>Displayed only for Primary / Standby(RO) / Leader / Replica</li><li>Standby(Recovery): Displayed as <code>-</code></li></ul></td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Data Volume (Tibero)</strong></td><td>Display data volume usage as a bar chart + Threshold display</td><td>O</td><td>X</td><td>O</td><td>X</td></tr><tr><td><strong>Redo log Volume (Tibero)</strong></td><td>Display redo log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Archive log Volume (Tibero)</strong></td><td>Display archive log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Root Volume</strong></td><td>Display Root volume usage as a bar chart</td><td>O</td><td>O</td><td>X</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### DB Service List User Settings <a href="#db-service-list" id="db-service-list"></a>

Clicking the ⚙️ **Settings** icon at the top right of the DB Service List screen allows you to configure the screen display method to suit your environment.

- Card View: Configures the information area displayed on the card.
- List View: Configures the columns displayed in the list.

Configured settings are saved per user and are applied consistently thereafter.

{% tabs %}
{% tab title="Card View" %}
In Card View, you can select the information area to display on the card.

1. Click ⚙️ **Settings** on the DB Service List screen.
2. Turn ON/OFF the items to display in **Configuration**.
3. Preview the changes in **Preview** on the right.
4. Click **Save** to apply.

### Setting Items <a href="#undefined-5" id="undefined-5"></a>

| Item | Description |
| --- | --- |
| DB Service Info | Display DB Service basic information |
| Node Info | Display status and resource information per node |
| Cluster Info | Display Cluster information and Connection Health |

{% hint style="info" %}
**Note**

You can check the results of the changes in real time in the Preview area on the right. They are not applied to the actual screen until saved.
{% endhint %}
{% endtab %}
{% tab title="List View" %}
In List View, you can select the columns to display in the list or change the column order.

1. Click ⚙️ **Settings** on the DB Service List screen.
2. Turn ON/OFF the columns to display.
3. Change the column order via Drag & Drop.
4. Click **Save** to apply.

### Key Features <a href="#undefined-6" id="undefined-6"></a>

| Feature | Description |
| --- | --- |
| Column Show/Hide | Set whether to display columns using the switch |
| Column Search | Quickly find the desired item by searching the column name |
| Change Column Order | Drag to change to the desired order |
| Reset to Default | Restore to the default column configuration |
{% endtab %}
{% endtabs %}

# Top N Chart Lookup <a href="#top-n-charts" id="top-n-charts"></a>

On the dashboard screen, check the top instances based on CPU Usage, Memory Usage, and Session Load as charts for DB Services you have lookup permission for. The Top N chart is displayed in the right area of the dashboard, and data is automatically configured according to the lookup permission scope of the logged-in user without any separate settings. If there are no DB Services you have lookup permission for, a no-data state is displayed in the chart area.

{% hint style="info" %}
**Note**

**Supported Engine Differences by Environment**

- AWS environment: Only Tibero engine instances are included in the lookup scope.
- Azure environment: Both Tibero and OpenSQL engine instances are included in the lookup scope.
{% endhint %}

### Display Metrics <a href="#undefined-7" id="undefined-7"></a>

<table><thead><tr><th>Metric</th><th>Description</th></tr></thead><tbody><tr><td>CPU Usage</td><td>Chart of the top 5 instances based on CPU usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Display data labels (%)</li></ul></td></tr><tr><td>Memory Usage</td><td>Chart of the top 5 instances based on Memory usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Display data labels (%)</li></ul></td></tr><tr><td>Session Load</td><td>Chart of the top 5 instances based on Session Load (%)<ul><li>A metric representing the proportion of Active Sessions relative to Max Session</li><li>X-axis: Instance Alias</li><li>Display data labels (%)</li><li>Tooltip: (Active Session/Max Session)X100</li></ul></td></tr></tbody></table>

### Expand/Collapse Chart Area <a href="#undefined-8" id="undefined-8"></a>

Clicking the button at the top of the Top N chart area collapses or expands the chart area.

- The default state is the expanded state.
- If the user has changed it to the collapsed state, the collapsed state is maintained even when navigating to another screen and returning within the same session.
- When you log out or the session ends and you connect with a new session, it is reset to the expanded state.
