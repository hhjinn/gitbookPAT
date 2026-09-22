**Management > Parameters > Settings** On this page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, in which changes are reflected without a restart but are reverted upon restart.

The range of parameters that can be viewed and modified varies depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select the Primary instance.
{% endhint %}

# Viewing Parameters

**Management > Parameters > Settings** When you enter the menu, the parameter list appears.

Tibero displays the DB parameter list directly. For OpenSQL (Azure), **DB** tab and **OpenHA** tab, switching between them to view each set of parameters.

1. **Management > Parameters > Settings** Click the menu.
2. If you are using OpenSQL, **DB** or **OpenHA** Click the tab to select the type of parameter to view.
3. Apply a search or filter to find the parameter you want. You can filter by whether a parameter is dynamic. You can also search directly by name and parameter value.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data type</td><td>The data type of the parameter</td></tr><tr><td>Default value</td><td>The value applied when the user has not set a separate value</td></tr><tr><td>Current value</td><td>The value currently applied to the DB</td></tr><tr><td>Config value</td><td>The value that will be reflected when the DB restarts</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- When the instance status is `Unavailable`, the current value is not displayed, and the Config value is displayed instead.
- OpenSQL **OpenHA** On the tab, `pg_hba`and `slot` parameters cannot be viewed or modified. These parameters are configured only in the **Connection Information Management** menu.
{% endhint %}

# Modifying Parameters

In the parameter list, **Edit** Clicking the button switches to edit mode, where you can directly edit parameter values in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, your changes will not be saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, **Edit** the tab that was selected at the time the button was clicked (**DB** or **OpenHA**) becomes the only one switched to an editable state. While editing, you cannot move to another tab; move only after saving or canceling.
{% endhint %}

1. **Management > Parameters > Settings** Click the menu.
2. If you are using OpenSQL, **DB** or **OpenHA** Click the tab to select the type.
3. **Edit** Click the button.
4. Modify the parameter values.
5. To revert a specific parameter to its original value, restore it. Select the parameter. **Restore default value** or **Restore current value** Click the button.
6. **Save** Click the button.
7. Review the preview of the changes.
8. **Apply** or **Apply Temporarily**to save the changes.

{% hint style="info" %}
**Note**

Parameter names highlighted in blue are the values currently being modified.
{% endhint %}

## Modifiable targets by instance state

<table data-full-width="true"><thead><tr><th>Modification target</th><th>Modifiable conditions</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance state is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>Instance state <code>Unavailable</code>(DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter modification mode while the instance is down and save without entering a value, it is processed with the reference value (Config value takes priority; if none, the default value).

{% hint style="info" %}
**Note**

Global parameters cannot be modified in a Tibero multi-node configuration.
{% endhint %}

## Save method by modified parameter type

<table data-full-width="true"><thead><tr><th>Modified parameter type</th><th>Save method</th></tr></thead><tbody><tr><td>Mixed dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are reflected after the DB restarts (connected sessions are terminated; takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Reflected in the current value immediately without a restart</li><li><strong>Apply Temporarily</strong>: Reflected in the current value without a restart; restored to the existing Config value when the DB restarts</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Apply Temporarily is supported only by the Tibero engine. OpenSQL does not support Apply Temporarily, so when modifying parameters you can only use **Apply**.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Even if you enter an incorrect value, it is saved without an error, so check the OpenHA log after modifying. `[ERROR]` The log also includes a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and set it back to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — `ttl` A value that cannot be converted was entered into an item that must be a number. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — `maximum_lag_on_failover`A value that is not filtered out at save time raised an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Load template

In Tibero parameter modification mode, load a pre-saved parameter template and apply it to the modification list in bulk.

{% hint style="info" %}
**Note**

Loading templates is supported only by the Tibero engine. In OpenSQL, the **Load** button is not displayed.
{% endhint %}

1. In parameter modification mode, **Load** click the button.
2. Select the template to apply from the template list.
3. Review the preview.
4. **Apply** Click the button.
5. Once the template is reflected in the modification list, **Save**click to apply the parameters.

---

# Parameter template

**Management > Parameters > Templates** View and apply parameter templates on the page. You can load a predefined template to configure multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates can only be used with the Tibero engine. If you are using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speeds and high transaction frequency |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. **Management > Parameters > Templates**Go to.
2. Review the parameter template list.
3. Click a template name to view the parameter details of that template in the drawer.
4. **Apply** Click the button.
5. Review the list of parameters to be changed (Name / Current Value / Modified Value) in the apply confirmation modal.
6. **Apply** Click the button.

---

# Parameter Modification History

**Management > Parameters > Modification History** On this page, you can view the change history of database parameters.

Review the history list grouped by modification request, and click each modification to view details of the changed parameters, including the previous value, modified value, dynamic status, and application method. You can add or edit a description for a modification history, allowing you to keep a record of the reason for the change.

1. **Management > Parameters > Modification History**Click it.
2. Select the desired period from the query period dropdown.
3. Check the parameter modification history in the table.

## Viewing Modification History Details

1. The one you want to check in the modification history table **Modification Date**Click it.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Editing the Modification History Description

{% hint style="warning" %}
**Caution**

After entering edit mode **Save** If you do not click the button, the changes will not be saved.
{% endhint %}

1. In the modification history table **Modification Date**Click it to open the drawer.
2. At the top of the drawer **Edit** Click the button.
3. Enter the reason for the change or a description in the description input field.
4. **Save** Click the button.
