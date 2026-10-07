Connection Information Management is a menu that checks the connection address of the DB Service and integrally manages access control and external integration settings.

The tabs provided in the menu differ depending on the DB engine.

| Tab | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Endpoint | Checking the endpoint address that external applications can connect to | ✓ | ✓ |
| Access Control | Viewing and managing IP-based access allow/block rules (pg_hba) | — | ✓ |
| OpenProxy | Viewing and modifying OpenProxy parameters and Pool, User, and Shard configurations | — | ✓ |
| Replication Slot | Viewing and managing Replication Slots used for integration with external systems | — | ✓ |

# Common top area <a href="#common-top-area" id="common-top-area"></a>

At the top of the Connection Information Management screen, the identification information of the currently selected DB Service is fixed and displayed across all tabs.

| Item | Description |
| --- | --- |
| Status | Current status of the DB Service |
| DB Type | Database engine type |
| Topology | Database cluster configuration method |

---

# Endpoint tab <a href="#endpoint" id="endpoint"></a>

The Endpoint tab consists of two areas, **Service Endpoint** and **Endpoint Details**, and it views the DB Service's representative connection address and per-instance details.

**Service Endpoint**

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Endpoint</td><td><ul><li>Representative service connection address</li><li>Single: displays Private IP</li><li>HA·TAC·DR: displays VIP</li></ul></td></tr><tr><td>Port</td><td>DB Listener port number</td></tr></tbody></table>

**Endpoint Details**

| Column | Description |
| --- | --- |
| Alias | Instance alias |
| Role | Primary / Standby(Recovery) / Standby(Read Only) |
| VIP | VIP address for instance connection |
| Private IP | Instance internal network address |
| Port | DB Listener port number |
| Health | Instance status |

Only instances that have completed creation appear in the list; instances being created are not displayed. Even if a Failover or Switchover occurs, the Service Endpoint is always displayed based on the current Primary instance.

{% hint style="info" %}
**Note**

If a spec change operation is in progress, an in-progress banner is displayed at the top of the screen, and in this state the **Topology** item in the common top area displays the topology from before the change was applied.
{% endhint %}

## How to view the Endpoint <a href="#view-endpoint" id="view-endpoint"></a>

1. Click **Management > Connection Information Management** in the top menu.
2. Click the **Endpoint** tab.
3. In the **Service Endpoint** area, check the DB Service's representative connection address and port. **Single**: displays Private IP **HA·TAC·DR**: displays VIP
4. In the **Endpoint Details** list, check each instance's alias, role, VIP, Private IP, port, and Health status.
5. Copy the Endpoint address using the 📋 icon of the row you want to copy.
6. Manually refresh the list using the 🔃 icon at the top.

---

# Access Control tab <a href="#access-control" id="access-control"></a>

{% hint style="info" %}
**Note**

The Access Control tab is provided only in the **OpenSQL** environment.
{% endhint %}

Displays the list of pg_hba rules currently applied to the DB Service in table format. The rules are fixed-sorted in ascending order of Priority, and rules positioned higher are applied first.

| Column | Description |
| --- | --- |
| Priority | The order in which the rule is applied. The smaller the number, the earlier it is applied |
| Type | Connection type (`local` / `host` / `hostssl` / `hostnossl`) |
| Database alias | Name of the database to which the rule is applied |
| User | Name of the user to which the rule is applied |
| Address | Client address to allow or block |
| Method | Authentication method |
| Auth Option | Detailed authentication options according to the Method |
| Comment | Description of the rule |

The screen operates in two states: view mode and edit mode. In view mode, you add and remove rules using the **Create**/**Delete** buttons, and in edit mode, the entire table switches to an inline-editable state to change existing rule values or Priority (order).

{% hint style="warning" %}
**Caution**

Fixed rules automatically created by the system cannot be edited or reordered even in edit mode. At least the top 3 rules are system fixed rules, and there can be up to 4 depending on the Barman configuration. The Priority of rules added by the user can be assigned starting from the number after the system fixed rules.
{% endhint %}

## Viewing rules <a href="#view-rules" id="view-rules"></a>

1. In **Management > Connection Information Management**, click the **Access Control** tab.
2. Check the list of currently applied pg_hba rules in ascending order of Priority. The system rules fixed at the top of the list cannot be modified or deleted.

## Creating a rule <a href="#create-rule" id="create-rule"></a>

1. Click the **Create** button.
2. Enter the following items in the right drawer.

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Priority</td><td>Rule application order (the smaller the number, the higher the priority)</td><td><ul><li>If not entered, it is added as the last order</li><li>Can be entered starting after the system fixed rule number</li></ul></td></tr><tr><td>Type *</td><td>Connection type</td><td><ul><li>Select one of <code>local</code> / <code>host</code> / <code>hostssl</code> / <code>hostnossl</code></li><li>Default value <code>host</code></li></ul></td></tr><tr><td>Database *</td><td>Database to which the rule will be applied</td><td><ul><li>Select one or more from the database list, or select one of the special keywords (<code>all</code>, <code>sameuser</code>, <code>samerole</code>)</li><li>A special keyword and the database list cannot be selected at the same time</li></ul></td></tr><tr><td>User *</td><td>User to which the rule will be applied</td><td>Select one or more from the user list, or select <code>all</code></td></tr><tr><td>Address *</td><td>Client address to allow access</td><td><ul><li>Directly enter a CIDR or Hostname, or select a special keyword (<code>all</code>, <code>samehost</code>, <code>samenet</code>)</li><li>Disabled if Type is <code>local</code></li><li>When a single IP is entered, it is automatically converted to CIDR format (IPv4: <code>/32</code>, IPv6: <code>/128</code>)</li></ul></td></tr><tr><td>Method *</td><td>Authentication method</td><td><ul><li>Select from the dropdown</li><li>Default value <code>scram-sha-256</code></li></ul></td></tr><tr><td>Auth Option</td><td>Detailed authentication options for the Method</td><td><ul><li>The input method differs depending on the Method</li><li>Disabled when <code>trust</code> or <code>reject</code> is selected</li><li>Dropdown selection when <code>scram-sha-256</code> or <code>md5</code> is selected</li><li>For other Methods, enter in <code>key=value</code> format</li></ul></td></tr><tr><td>Comment</td><td>A note about the rule</td><td>Line breaks cannot be entered</td></tr></tbody></table>

The * notation indicates a required input item.

1. After completing the input, click the **Create** button.

## Editing a Rule <a href="#edit-rule" id="edit-rule"></a>

1. Click the **Edit** button.
2. Once switched to edit mode, modify each table entry inline. When you change the Priority, the order of other affected rules is automatically adjusted. If you change the Type to `local`, the Address field is disabled.
3. When editing is complete, click the **Save** button.
4. Review the before-and-after changes in the change comparison modal.
5. Click the **Save** button.

Once saving is complete, the changes are immediately applied to pg_hba.

{% hint style="warning" %}
**Caution**

The connection may be re-validated upon saving. Save after thoroughly reviewing the contents in the change comparison modal.
{% endhint %}

## Deleting a Rule <a href="#delete-rule" id="delete-rule"></a>

1. Select the checkbox of the rule to delete.
2. Click the **Delete** button.
3. Click the **Delete** button in the delete confirmation modal.

---

# OpenProxy Tab <a href="#openproxy" id="openproxy"></a>

{% hint style="info" %}
**Note**

The OpenProxy tab is provided only in the **OpenSQL** environment.
{% endhint %}

Query and modify OpenProxy parameters by Scope. When you select a query range in the **Select Scope** area on the left side of the screen, the list of parameters for that Scope is displayed in the table on the right. The default selection is **General**.

| Scope | Description |
| --- | --- |
| General | Global configuration parameters |
| Virtual Router | HA/VIP-related configuration parameters |
| Pool | Parameters for a specific Pool |
| User | Parameters for a specific user within a specific Pool |
| Shard | Parameters for a specific Shard within a specific Pool |

Pool, User, and Shard are displayed in an accordion structure.

**Parameter List Table**

| Column | Description |
| --- | --- |
| Name | Parameter name |
| Type | Parameter data type |
| Default value | The default value applied when the user has not set it |
| Current value | The currently applied value |
| Dynamic parameter | Whether it can be applied immediately without a restart (`Yes` / `No`) |

Edit mode operates on a session basis rather than a screen basis, so even if you change the Scope, the changes you have already made are retained. Upon saving, the result differs depending on the parameter type.

- **Modifying only dynamic parameters**: applied immediately without a restart
- **Including static parameters**: applied after OpenProxy restart

## Viewing parameters <a href="#view-parameters" id="view-parameters"></a>

1. In **Management > Connection Information Management**, click the **OpenProxy** tab.
2. By default, the parameter list of the **General** Scope is displayed.
3. Select the desired query range in **Select Scope**.
4. Search parameters by name, default value, or current value, or filter by whether they are **dynamic parameters**.

## Modifying parameters <a href="#edit-parameters" id="edit-parameters"></a>

1. Click the **Edit** button.

{% hint style="info" %}
**Note**

The **Edit** button is enabled only when the DB Service status is `Running`.
{% endhint %}

2. Once switched to edit mode, directly modify the **current value** of the parameter you want to change in the table. Parameters pending change are displayed in blue.
3. Even if you change the Scope, the changes being edited are retained.
4. When editing is complete, click the **Save** button.
5. Review the changes in the save confirmation modal.
6. Click the **Apply** button. **Modifying only dynamic parameters**: applied immediately **Including static parameters**: applied after restart

{% hint style="info" %}
**Note**

Changes are not applied to the server until you click the **Apply** button. Clicking the **Cancel** button resets all changes.
{% endhint %}

## Creating a Pool / User / Shard <a href="#create-pool-user-shard" id="create-pool-user-shard"></a>

1. Click the **Edit** button to switch to edit mode.
2. In the **Select Scope** area, click the ➕ icon of the type to create (Pool, User, Shard).
3. Enter the following items in the creation modal.

{% tabs %}
{% tab title="Creating a Pool" %}
<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Pool Name *</td><td>Pool name</td><td>1 to 63 characters; lowercase English letters (a-z), digits (0-9), and underscore (<code>_</code>) are allowed. The first character cannot be a digit. Duplicates are not allowed within the DB Service.</td></tr><tr><td>User Name *</td><td>Name of the user to belong to the Pool</td><td>1 to 63 characters; lowercase English letters (a-z), digits (0-9), and underscore (<code>_</code>) are allowed. The first character cannot be a digit.</td></tr><tr><td>Pool Size *</td><td>The maximum number of DB server connections that the user can occupy simultaneously</td><td>Enter an integer. Range: 1 to max connections. Default value: 9</td></tr><tr><td>Password *</td><td>User password</td><td>1 to 30 characters; lowercase English letters (a-z), digits (0-9), and special characters (<code>-</code>, <code>_</code>, <code>#</code>, <code>$</code>) are allowed</td></tr><tr><td>Shard Name *</td><td>Name of the Shard to create in the Pool</td><td>1 to 30 characters; lowercase English letters (a-z), digits (0-9), and underscore (<code>_</code>) are allowed. Duplicates are not allowed within the same Pool.</td></tr><tr><td>Database Name *</td><td>Database to connect to the Pool</td><td>Select from the dropdown</td></tr><tr><td>Servers *</td><td>DB server to connect to</td><td><ul><li>Select one or more from the dropdown</li><li>Displays instance Role and Instance Alias</li></ul></td></tr><tr><td>Use Patroni</td><td>Whether to use Auto Failover via Patroni</td><td>Always enabled (cannot be changed)</td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% tab title="Creating a User" %}
| Item | Description | Input rules |
| --- | --- | --- |
| User Name * | Name of the user to add | 1 to 63 characters; lowercase English letters (a-z), digits (0-9), and underscore (`_`) are allowed. The first character cannot be a digit. Duplicates are not allowed within the same Pool. |
| Pool Size * | The maximum number of DB server connections that the user can occupy simultaneously | Enter an integer. Range: 1 to max connections. Default value: 9 |
| Password * | User password | 1 to 30 characters; lowercase English letters (a-z), digits (0-9), and special characters (`-`, `_`, `#`, `$`) are allowed |

The * notation indicates a required input item.
{% endtab %}
{% tab title="Creating a Shard" %}
<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Shard Name *</td><td>Name of the Shard to add</td><td>1 to 30 characters; lowercase English letters (a-z), digits (0-9), and underscore (<code>_</code>) are allowed. Duplicates are not allowed within the same Pool.</td></tr><tr><td>Database Name *</td><td>Database to connect to the Shard</td><td>Select from the dropdown</td></tr><tr><td>Servers *</td><td>DB server to connect to</td><td><ul><li>Select one or more from the dropdown</li><li>Displays instance Role, Instance Alias, and Health</li><li>Health is for reference only and does not affect server selection</li></ul></td></tr><tr><td>Use Patroni</td><td>Whether to use Auto Failover via Patroni</td><td>Always enabled (cannot be changed)</td></tr></tbody></table>

The * notation indicates a required input item.
{% endtab %}
{% endtabs %}

4. After entering all required items, click the **Create** button. The created item is temporarily added to the list.
5. To finalize, click the **Save** button.

{% hint style="info" %}
**Note**

The created items are not applied to the server until you click the **Save** button. If you leave the page before saving, the changes are reset.
{% endhint %}

## Deleting a Pool / User / Shard <a href="#delete-pool-user-shard" id="delete-pool-user-shard"></a>

1. Click the **Edit** button to switch to edit mode.
2. In the **Select Scope** area, click the 🗑️ icon of the Pool, User, or Shard item you want to delete. The item becomes disabled and the icon changes to 🔃. If you delete a Pool, the Users and Shards under that Pool are also disabled together.
3. To cancel the deletion, click the 🔃 icon.
4. To confirm the deletion, click the **Save** button.

{% hint style="info" %}
**Note**

The 🗑️ icon is enabled when there are two or more Pools. Users and Shards can be deleted when two or more of each exist within the corresponding Pool.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you leave the page before saving, the deletion settings are reset. If you delete the currently selected Pool, User, or Shard, the Scope automatically changes to General.
{% endhint %}

---

# Replication Slot Tab <a href="#replication-slot" id="replication-slot"></a>

{% hint style="info" %}
**Note**

- The Replication Slot tab is provided only in the **OpenSQL** environment.
- **Logical Type Slot** does not support creation, but querying and deletion are possible.
- Only **Permanent Scope Slot** can be created, while **Temporary Scope Slot** can only be queried and cannot be selected or deleted.
{% endhint %}

Displays the list of Replication Slots created on the OpenSQL Primary instance in table format. Even if a Failover or Switchover occurs, the query is always based on the current Primary instance.

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Replication Slot name</td></tr><tr><td>Type</td><td><ul><li><code>Physical</code> (stores WAL logs as-is)</li><li><code>Logical</code> (converts and stores in INSERT·UPDATE·DELETE form)</li></ul></td></tr><tr><td>Scope</td><td><ul><li><code>Permanent</code> (persisted permanently)</li><li><code>Temporary</code> (automatically deleted when the session ends)</li></ul></td></tr><tr><td>Status</td><td><ul><li><code>Connected</code> (Replication Client is connected)</li><li><code>Disconnected</code> (no connected Client)</li></ul></td></tr><tr><td>Backlog(MB)</td><td>Amount of WAL data that has not yet been consumed and is still retained</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Replication Slots created or deleted directly in PostgreSQL outside the scope of OwlDB management are not reflected in the OwlDB console. In this case, related management functions such as Slot status query, deletion, and failure handling may not work properly. Always create and delete Replication Slots in the OwlDB console.
{% endhint %}

## Querying Replication Slots <a href="#view-replication-slots" id="view-replication-slots"></a>

1. In **Management > Connection Information Management**, click the **Replication Slot** tab.
2. Check the list of Replication Slots created on the current Primary instance.
3. Use the **Type** (Physical / Logical) or **Status** (Connected / Disconnected) filter to narrow the list.

## Creating a Replication Slot <a href="#create-replication-slot" id="create-replication-slot"></a>

1. Click the **Create** button.
2. Enter the following items in the right drawer.

| Item | Description | Input rules |
| --- | --- | --- |
| Name * | Unique name of the Replication Slot | Up to 30 characters using lowercase English letters (a-z), numbers (0-9), and underscores (`_`). Spaces and tabs cannot be entered. Duplicates are not allowed. |
| Type * | Slot type | Fixed to `Physical` |
| Scope | Operation management target | Fixed to `Permanent` |

The * notation indicates a required input item.

1. After entering the items, click the **Create** button.

{% hint style="info" %}
**Note**

If the DB Service status is `Updating` or `Failover`, the **Create** button is disabled.
{% endhint %}

## Deleting a Replication Slot <a href="#delete-replication-slot" id="delete-replication-slot"></a>

1. Select the Slot to delete.
2. Click the **Delete** button.
3. After reviewing the content in the deletion confirmation modal, click the **Delete** button.

{% hint style="info" %}
**Note**

For a Slot whose Status is `Connected` and when the DB Service status is `Updating` or `Failover`, the **Delete** button is disabled.
{% endhint %}
