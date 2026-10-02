**Management > Parameters > Settings** On this page, you can query and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, where the changes are reflected without a restart but are restored upon restart.

The range of parameters that can be queried and modified differs depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

While a Standby (Recovery) instance is selected, parameters cannot be modified. To modify them, you must select the Primary instance.
{% endhint %}

# Parameter Lookup

**Management > Parameters > Settings** When you enter the menu, the parameter list appears.

Tibero displays the DB parameter list directly. OpenSQL (Azure) **DB** tab and **OpenHA** tab to switch between them and look up each set of parameters.

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL, **DB** or **OpenHA** click the tab to select the type of parameter to look up.
3. Apply a search or filter to find the parameter you want. You can filter by whether a parameter is dynamic. You can also search directly by name and parameter value.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter Name</td></tr><tr><td>Data format</td><td>Parameter Data Type</td></tr><tr><td>Default value</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current Value</td><td>The value currently applied to the DB</td></tr><tr><td>Config Value</td><td>The value that will take effect when the DB is restarted</td></tr><tr><td>Dynamic Parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- If the instance status is not `Unavailable`In this case, the current value is not displayed, and the Config value is shown instead.
- OpenSQL **OpenHA** In the tab, `pg_hba`and `slot` parameters cannot be looked up or modified. These parameters are **Connection Information Management** set only from the menu.
{% endhint %}

# Modifying Parameters

In the parameter list, **Edit** click the button to switch to edit mode, where you can directly edit parameter values in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen in edit mode without saving, your changes will not be saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, **Edit** only the tab (**DB** or **OpenHA**) selected at the time you clicked the button becomes editable. While editing, you cannot switch to another tab; move only after saving or canceling.
{% endhint %}

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL, **DB** or **OpenHA** click the tab to select the type.
3. **Edit** Click the button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select the parameter. **Restore Default Value** or **Restore Current Value** Click the button.
6. **Save** Click the button.
7. Review the preview of your changes.
8. **Apply** or **Temporary Apply**Click to save your changes.

{% hint style="info" %}
**Note**

A parameter name highlighted in blue is a value that is currently being edited.
{% endhint %}

## Editable targets by instance state

<table><thead><tr><th>Modification Target</th><th>Modification Conditions</th></tr></thead><tbody><tr><td>Current Value</td><td>When the instance state is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config Value</td><td><ul><li>Instance status <code>Unavailable</code>when (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter edit mode while the instance is down and save without entering a value, it is handled as the baseline value (Config value takes priority; if none, the default value).

{% hint style="info" %}
**Note**

In a Tibero multi-node configuration, global parameters cannot be modified.
{% endhint %}

## Save behavior by modified parameter type

<table><thead><tr><th>Modified Parameter Type</th><th>Save Behavior</th></tr></thead><tbody><tr><td>Mix of dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes take effect after a DB restart (connection sessions are terminated; takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Applied to the current value immediately without a restart</li><li><strong>Temporary Apply</strong>: Applied to the current value without a restart; restored to the existing Config value when the DB is restarted</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary apply is supported only by the Tibero engine. OpenSQL does not support temporary apply, so when modifying parameters **Apply**only this can be used.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Even if you enter an invalid value, it is saved without an error, so check the OpenHA logs after modifying. `[ERROR]` A stack trace is also left in the log.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and set it back to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — `ttl` A value that cannot be converted was entered for an item that must be a number. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — `maximum_lag_on_failover`A value that is not filtered at save time caused an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Load Template

In Tibero parameter edit mode, load a previously saved parameter template and apply it to the modification list in bulk.

{% hint style="info" %}
**Note**

Load template is supported only by the Tibero engine. In OpenSQL, **Load** the button is not displayed.
{% endhint %}

1. In parameter edit mode, **Load** Click the button.
2. Select the template to apply from the template list.
3. Review the preview.
4. **Apply** Click the button.
5. Once the template is reflected in the modification list, **Save**Click to apply the parameters.

---

# Parameter Template

**Management > Parameters > Templates** Look up and apply parameter templates on the page. You can load a predefined template to set multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates are available only in the Tibero engine. When using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speed and high transaction frequency |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. **Management > Parameters > Templates**Navigate to
2. Check the list of parameter templates.
3. Click a template name to view the parameter details of that template in the drawer.
4. **Apply** Click the button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. **Apply** Click the button.

---

# Parameter Modification History

**Management > Parameters > Modification History** On this page, you can query the change history of database parameters.

Review the history list grouped by modification request, and click each modification to view detailed information about the changed parameters, including the previous value, modified value, whether it is dynamic, and the application method. You can add or edit a description for a modification history entry to keep a record of the reason for the change.

1. **Management > Parameters > Modification History**Click it.
2. Select the desired period from the query period dropdown.
3. Check the parameter modification history in the table.

## Viewing Modification History Details

1. To check in the modification history table **Modification Date**Click it.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, use the table filter or search to find a specific parameter.

## Editing a Modification History Description

{% hint style="warning" %}
**Caution**

After entering edit mode **Save** If you do not click the button, your changes will not be saved.
{% endhint %}

1. In the modification history table **Modification Date**Click to open the drawer.
2. At the top of the drawer **Edit** Click the button.
3. Enter the reason or description for the change in the description input field.
4. **Save** Click the button.
