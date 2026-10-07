Provides summary information and notifications about the status of all databases and instances in operation. **OwlDB console screen > OwlDB logo**Click to view the dashboard.

<figure>
<img src="../.gitbook/assets/image-ccc625b2.png" alt="">
<figcaption>Figure 1. Dashboard</figcaption>
</figure>

# Check overall status summary information <a href="#status-summary" id="status-summary"></a>

For all databases/instances in operation, check the top-level status value (**Status**) and detailed status value (**Health**). Clicking each status allows you to check the list of DB Services/instances with that status value in '[Check DB Service/instance list](#service-instance-list)'.

{% tabs %}
{% tab title="Status" %}
Status, the top-level status, is displayed at the database level.

- Because the status can change per instance node, the count is displayed next to the status in the form (n/m). n: number of available instance nodes m: total number of instance nodes
- Types marked in Bold with `*`appended afterward can be filtered on the dashboard.

| Type | Description |
| --- | --- |
| Deploying | Installing new resource |
| Registering | Registering new resource |
| **Running** * | Database available for normal use |
| **Updating** * | Applying planned tasks/changes to the database (e.g., full restart, role switch, spec change, migration, recovery, etc.) |
| **Degraded** * | Some databases/components unavailable (e.g., individual instance restart, etc.) |
| **Failover** * | Performing Auto Failover due to database failure detection (occurs when the entire Primary DB cluster is Unavailable) |
| **Down** * | Entire database unavailable (both Primary and Standby unavailable) |
| Starting | Transitioning from Stopped to Running |
| Stopping | Transitioning from Running to Stopped (full DB shutdown) |
| Stopped | All resources temporarily unused (resources deactivated) |
| Unregistering | Deregistering a registered DB Service (excluded from management targets upon completion) |
| Terminating | Permanently deleting all resources/data (access/recovery not possible upon completion) |

The * notation indicates a required input item.
{% endtab %}
{% tab title="Health" %}
Health, the detailed status, is displayed per instance node.

- Displayed by combining the DB Boot Mode, Instance, cluster management tool (hereinafter CMT), and Agent connection status.
- Features restricted according to the message can be checked via a banner when entering each menu.

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
|   | unavailable | Issue: VM Down | VM where the node is located Down |
| ⚫ Retired | retired | Issue: Failovered Primary | Performing Failover (former) Primary abnormally terminated |
{% endtab %}
{% tab title="OpenSQL" %}
The Health of an OpenSQL instance node is determined by combining the Patroni, OpenProxy, etcd, and Agent status.

| Display | Status | Message | Description |
| --- | --- | --- | --- |
| 🟢 Available | available | - | Node normal |
| 🟡 Limited | limited | Issue: etcd Inactive | etcd abnormal |
|   | limited | Issue: OpenProxy Inactive | OpenProxy abnormal |
| 🔴 Unavailable | unavailable | Issue: DB Down | DB Down due to Patroni abnormal |
|   | unavailable | Issue: Agent Disconnect | Agent connection lost with the VM where the node is located |
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

# Check DB Service/instance list <a href="#service-instance-list" id="service-instance-list"></a>

Check the list of all DB Services and their sub-instances. '[Check overall status summary information](#status-summary)'The displayed DB Service and instance lists vary according to the status value selected in.

- You can perform restart operations on the sub-instances of the selected database.
- You can perform start, stop, delete, role switch, and license renewal management operations on the selected database.
- Provides a database/instance alias search feature.

{% hint style="info" %}
**Note**

You can perform various management operations on the DB Service. For details, **Operation** you can check on the page.
{% endhint %}

### Switch list view (card view/list view) <a href="#undefined-2" id="undefined-2"></a>

You can view the DB Service/instance list in two ways: card view and list view. When switching views, the sorting feature is reset, but filtering is maintained.

{% tabs %}
{% tab title="Card View" %}
The card view organizes and displays the key information of DB Services in individual card form to make it easy to grasp visually. Up to 4 cards are displayed per row, and the size of each card is fixed. If the content exceeds the fixed size, internal scrolling is enabled. In the last slot of the currently operating DB Service card list, `➕` an icon is displayed, and clicking it navigates to the DB Service creation page.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service information</td><td>Summarizes and displays the key information of the database at the top of the card<br>Display items<ul><li>DB Type Logo</li><li>DB Service Name</li><li>Status</li><li>DB Type</li><li>DB version (for OpenSQL, the OpenSQL version and PostgreSQL version are displayed simultaneously)</li><li>Topology</li><li>Eventlog : I / W / E (Info / Warning / Error respectively)</li></ul></td></tr><tr><td>Node information</td><td>Displays the instance information linked under the database. Can be collapsed/expanded in accordion form based on Role<br>Sorting rules<ul><li>Tibero: Displayed in the order Primary → Standby(Read Only) → Standby(Recovery)</li><li>OpenSQL: Displayed in the order Leader → Replica</li><li>Sorted by AZ within the same Role<br>Display items</li><li>Role : Displays the number of instances for that role</li><li>Data volume (Tibero) / Volume (OpenSQL): Displays usage (%), a bar chart, and Threshold<br>Table items</li><li>Instance alias</li><li>Health</li><li>vCPU: current CPU usage (%)</li><li>Memory: current memory usage (%)</li><li>Active sessions: current number of active sessions / Max Session Count (not shown for Standby(Recovery))</li><li>Availability zone: AZ information where the instance is located</li></ul></td></tr><tr><td>Cluster information</td><td>Displays Connection Health as a cluster diagram<ul><li>Single: Displays only Primary/Leader DB information</li><li>Single +DR / HA: Displays Primary/Leader DB and Standby/Replica DB information. Displays P-S connection status monitoring information; Standby/Replica DB is created per AZ</li><li>Tibero TAC: Displays multiple nodes inside the Primary DB Box</li><li>Tibero TAC + DR: Displays Primary DB and Standby DB information. Displays P-S connection status monitoring information; multiple nodes are displayed inside the Primary DB Box; Standby DB is created per AZ</li><li>DR configuration connection status monitoring: Represents the connection status between Primary - Standby / Leader - Replica through line color and displayed information</li><li>Normal: green,<code>Connected</code></li><li>Failed: red,<code>Disconnected</code></li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="List View" %}
The list view displays database and sub-instance information as a tree-form table so that the hierarchical structure is easy to understand.

{% hint style="info" %}
**Note**

'Default value' refers to items exposed by default on the initial screen, and 'required value' refers to items that cannot be hidden.
{% endhint %}

<table><thead><tr><th>Column</th><th>Description</th><th>DB Service level</th><th>Instance level</th><th>Default value</th><th>Required value</th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Name display<ul><li>When the DB Service name is clicked, moves to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/doc/5d7daeb2-e4f4-4409-aa74-dcaf1d65ab46/README.md">View service meta information</a>' page</li><li>When the instance alias is clicked, moves to the '<a href="https://github.com/hhjinn/gitbookPAT/tree/dori/SNGSMUVdPJMBNhWqlXnC/doc/dF57s45IXBUgU7RX1UvL/README.md">Instance management</a>' page</li></ul></td><td>DB Service name</td><td>Instance alias</td><td>O</td><td>O</td></tr><tr><td><strong>Creation date</strong></td><td>The DB Service creation date in <code>yyyy.mm.dd HH\:mm:ss</code> format</td><td>DB Service creation date</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Status</strong></td><td>Status or Health display</td><td><ul><li>Creating</li><li>Running(n/m)</li><li>Updating(n/m)</li><li>Degraded(n/m)</li><li>Failover(n/m)</li><li>Down(n/m)</li><li>Starting</li><li>Stopping</li><li>Stopped</li><li>Terminating</li><li>Deploying (on-premise)</li><li>Registering (on-premise)</li><li>Unregistering (on-premise)</li></ul></td><td><ul><li>🟢 (available)</li><li>🟡 (limited)</li><li>🔵 (in progress)</li><li>🔴 (unavailable)</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Type</strong></td><td>Displays the database engine type</td><td><ul><li>Tibero</li><li>OpenSQL</li></ul></td><td>-</td><td>O</td><td>X</td></tr><tr><td><strong>Configuration</strong></td><td>Topology or role display</td><td><ul><li>Tibero: Single, TAC(+DR)</li><li>OpenSQL: Single, HA</li></ul></td><td><ul><li>Tibero: Primary, Standby(Read Only), Standby(Recovery)</li><li>OpenSQL: Leader, Replica</li></ul></td><td>O</td><td>O</td></tr><tr><td><strong>Availability zone (Cloud)</strong></td><td>In a Cloud environment, displays the availability zone (AZ) information where the instance is located</td><td>-</td><td>Availability zone (AZ) information</td><td>O</td><td>X</td></tr><tr><td><strong>vCPU</strong></td><td>Displays the number of CPUs and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Memory</strong></td><td>Displays Memory and usage as a bar chart</td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Active sessions</strong></td><td>Displays the number of currently active sessions as a bar chart<ul><li>Displayed only for Primary / Standby(RO) / Leader / Replica</li><li>Standby(Recovery) :<code>-</code>displayed as</li></ul></td><td>O</td><td>O</td><td>O</td><td>X</td></tr><tr><td><strong>Data Volume (Tibero/OpenSQL)</strong></td><td>Displays data volume usage as a bar chart + Threshold display</td><td>O</td><td>X</td><td>O</td><td>X</td></tr><tr><td><strong>Redo log Volume (Tibero)</strong></td><td>Displays redo log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Archive log Volume (Tibero)</strong></td><td>Displays archive log volume usage as a bar chart</td><td>O</td><td>X</td><td>X</td><td>X</td></tr><tr><td><strong>Root Volume</strong></td><td>Displays Root volume usage as a bar chart</td><td>O</td><td>O</td><td>X</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### DB Service List user settings <a href="#db-service-list" id="db-service-list"></a>

The ⚙️ at the top right of the DB Service List screen **Settings** Clicking the icon allows you to configure the screen display method to suit your environment.

- Card View: Configures the information area displayed on the card.
- List View: Configures the columns displayed in the list.

The configured settings are saved per user and are applied identically thereafter.

{% tabs %}
{% tab title="Card View" %}
In Card View, you can select the information area to display on the card.

1. On the DB Service List screen, ⚙️ **Settings**Click.
2. **Configuration**Turn the items to display ON/OFF in
3. On the right **Preview**Preview the changes in
4. **Save**Click to apply.

### Setting items <a href="#undefined-5" id="undefined-5"></a>

| Item | Description |
| --- | --- |
| DB Service Info | Displays DB Service basic information |
| Node Info | Displays status/resource information per node |
| Cluster Info | Displays Cluster information/Connection Health |

{% hint style="info" %}
**Note**

You can check the change results in real time in the Preview area on the right. They are not applied to the actual screen until saved.
{% endhint %}
{% endtab %}
{% tab title="List View" %}
In List View, you can select the columns to display in the list or change the column order.

1. On the DB Service List screen, ⚙️ **Settings**Click.
2. Turn the columns to display ON/OFF.
3. Change the column order by Drag & Drop.
4. **Save**Click to apply.

### Key Features <a href="#undefined-6" id="undefined-6"></a>

| Feature | Description |
| --- | --- |
| Show/hide columns | Set whether to display columns using the switch |
| Column search | Quickly find the desired item by searching for a column name |
| Change column order | Drag to change to the desired order |
| Initialize to default values | Restore to default column configuration |
{% endtab %}
{% endtabs %}

# View Top N charts <a href="#top-n-charts" id="top-n-charts"></a>

On the dashboard screen, view the top instances by CPU Usage, Memory Usage, and Session Load as charts for DB Services for which you have view permission. The Top N charts are displayed in the right area of the dashboard, and data is automatically composed according to the view permission scope of the logged-in user without any separate configuration. If there is no DB Service for which you have view permission, a no-data state is displayed in the chart area.

{% hint style="info" %}
**Note**

**Supported engine differences by environment**

- AWS environment: Only Tibero engine instances are included in the view scope.
- Azure environment: Both Tibero and OpenSQL engine instances are included in the view scope.
{% endhint %}

### Displayed metrics <a href="#undefined-7" id="undefined-7"></a>

<table><thead><tr><th>Metric</th><th>Description</th></tr></thead><tbody><tr><td>CPU Usage</td><td>Chart of the top 5 instances by CPU usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li></ul></td></tr><tr><td>Memory Usage</td><td>Chart of the top 5 instances by Memory usage<ul><li>Targets all instances regardless of engine type</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li></ul></td></tr><tr><td>Session Load</td><td>Chart of the top 5 instances by Session Load (%)<ul><li>A metric representing the ratio of Active Sessions to Max Sessions</li><li>X-axis: Instance Alias</li><li>Displays data labels (%)</li><li>Tooltip: (Active Session/Max Session)X100</li></ul></td></tr></tbody></table>

### Expand/collapse the chart area <a href="#undefined-8" id="undefined-8"></a>

Clicking the button at the top of the Top N chart area collapses or expands the chart area.

- The default state is expanded.
- If the user changes it to the collapsed state, the collapsed state is maintained even after navigating to another screen and returning within the same session.
- When you log out or the session ends and you connect with a new session, it is reset to the expanded state.
