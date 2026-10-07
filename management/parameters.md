On the **Management > Parameters > Settings** page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data format, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, where changes are reflected without a restart but are restored upon restart.

The range of parameters that can be viewed and modified varies depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select the Primary instance.
{% endhint %}

# Parameter Lookup <a href="#view-parameters" id="view-parameters"></a>

When you enter the **Management > Parameters > Settings** menu, the parameter list appears.

Tibero displays the DB parameter list directly. OpenSQL (Azure) switches between the **DB** tab and the **OpenHA** tab to view each set of parameters.

1. Click the **Management > Parameters > Settings** menu.
2. If you are using OpenSQL, click the **DB** or **OpenHA** tab to select the type of parameter to view.
3. Apply a search or filter to find the desired parameter. You can filter by whether a parameter is dynamic. You can search directly by name and parameter value to find them.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data format</td><td>The parameter's data type</td></tr><tr><td>Default</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current value</td><td>Value currently applied to the DB</td></tr><tr><td>Config value</td><td>Value to be applied when the DB restarts</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- When the instance status is `Unavailable`, the current value is not displayed and the Config value is shown instead.
- On the OpenSQL **OpenHA** tab, the `pg_hba` and `slot` parameters cannot be queried or modified. These parameters can only be configured in the **Connection Information Management** menu.
{% endhint %}

# Modify parameter <a href="#modify-parameters" id="modify-parameters"></a>

When you click the **Edit** button in the parameter list, it switches to edit mode, where you can edit parameter values directly in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, your changes will not be saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, only the tab selected at the time you clicked the **Edit** button (**DB** or **OpenHA**) switches to an editable state. While editing, you cannot move to another tab; move only after saving or canceling.
{% endhint %}

1. Click the **Management > Parameters > Settings** menu.
2. If you are using OpenSQL, click the **DB** or **OpenHA** tab to select the type.
3. Click the **Edit** button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select a parameter. Click the **Restore default value** or **Restore current value** button.
6. Click the **Save** button.
7. Check the preview of your changes.
8. Click **Apply** or **Apply temporarily** to save your changes.

{% hint style="info" %}
**Note**

Parameter names highlighted in blue are values currently being edited.
{% endhint %}

## Editable targets by instance status <a href="#editable-by-instance-status" id="editable-by-instance-status"></a>

<table><thead><tr><th>Modification target</th><th>Conditions for modification</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance status is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>When the instance status is <code>Unavailable</code> (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter edit mode while the instance is down and save without entering a value, it is processed with the reference value (Config value first, or the default value if there is none).

{% hint style="info" %}
**Note**

In a Tibero multi-node configuration, global parameters cannot be modified.
{% endhint %}

## Save method by modified parameter type <a href="#save-method-by-parameter-type" id="save-method-by-parameter-type"></a>

<table><thead><tr><th>Modified parameter type</th><th>Save method</th></tr></thead><tbody><tr><td>Mix of dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are applied after the DB restarts (connection sessions are terminated; takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Applied to the current value immediately without a restart</li><li><strong>Apply temporarily</strong>: Applied to the current value without a restart; reverts to the existing Config value when the DB restarts</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary apply is supported only by the Tibero engine. OpenSQL does not support temporary apply, so only **Apply** can be used when modifying parameters.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Even if you enter an invalid value, it is saved without an error, so check the OpenHA log after modifying. The `[ERROR]` log also includes a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and set it back to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — An unconvertible value was entered for an item that must be a number, such as `ttl`. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — A value that is not filtered out at save time, such as `maximum_lag_on_failover`, caused an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Load template <a href="#load-template" id="load-template"></a>

In Tibero parameter edit mode, load a previously saved parameter template and apply it to the modification list in bulk.

{% hint style="info" %}
**Note**

Loading templates is supported only by the Tibero engine. In OpenSQL, the **Load** button is not displayed.
{% endhint %}

1. In parameter edit mode, click the **Load** button.
2. Select the template to apply from the template list.
3. Check the preview.
4. Click the **Apply** button.
5. Once the template is applied to the modification list, click **Save** to apply the parameters.

---

# Parameter template <a href="#parameter-templates" id="parameter-templates"></a>

On the **Management > Parameters > Templates** page, query and apply parameter templates. You can load a predefined template to set multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates can only be used with the Tibero engine. If you are using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speed and a high transaction frequency |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. Go to **Management > Parameters > Templates**.
2. Check the parameter template list.
3. When you click a template name, check the detailed parameter contents of that template in the drawer.
4. Click the **Apply** button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. Click the **Apply** button.

---

# Parameter modification history <a href="#parameter-change-history" id="parameter-change-history"></a>

On the **Management > Parameters > Modification History** page, you can query the change history of database parameters.

Check the history list grouped by modification request, and click each modification to query in detail the previous value, modified value, dynamic status, and apply method of the changed parameters. You can add or edit a description for the modification history, allowing you to keep a record of the reason for the change.

1. Click **Management > Parameters > Modification History**.
2. Select the desired period from the query period dropdown.
3. Check the parameter modification history in the table.

## View modification history details <a href="#change-history-details" id="change-history-details"></a>

1. In the modification history table, click the **Modification date** you want to check.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Edit modification history description <a href="#edit-change-description" id="edit-change-description"></a>

{% hint style="warning" %}
**Caution**

If you do not click the **Save** button after entering edit mode, your changes will not be saved.
{% endhint %}

1. In the modification history table, click the **Modification date** to open the drawer.
2. Click the **Edit** button at the top of the drawer.
3. Enter the reason for the change or a description in the description input field.
4. Click the **Save** button.
