**Management > Parameters > Settings** On the page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, which reflects changes without a restart but reverts upon restart.

The range of parameters that can be viewed and modified differs depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select the Primary instance.
{% endhint %}

# View parameters <a href="#view-parameters" id="view-parameters"></a>

**Management > Parameters > Settings** When you enter the menu, the parameter list appears.

Tibero displays the DB parameter list directly. OpenSQL (Azure) **DB** tab and **OpenHA** tab to switch between and view each set of parameters.

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL, **DB** or **OpenHA** click the tab to select the type of parameter to view.
3. Apply a search or filter to find the desired parameter. You can filter by whether a parameter is dynamic. You can also search directly by name and parameter value.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data Format</td><td>Parameter data type</td></tr><tr><td>Default</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current value</td><td>The value currently applied to the DB</td></tr><tr><td>Config value</td><td>The value that will take effect when the DB is restarted</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- If the instance status is not `Unavailable`In this case, the current value is not displayed, and the Config value is displayed instead.
- OpenSQL **OpenHA** In the tab, `pg_hba`and `slot` parameters cannot be viewed or modified. These parameters **Connection Information Management** are configured only from the menu.
{% endhint %}

# Modify parameters <a href="#modify-parameters" id="modify-parameters"></a>

In the parameter list, **Edit** click the button to switch to edit mode, where you can edit parameter values directly in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, the changes are not saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, **Edit** only the tab selected at the time the button was clicked (**DB** or **OpenHA**) switches to an editable state. While editing, you cannot move to another tab; move only after saving or canceling.
{% endhint %}

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL, **DB** or **OpenHA** click the tab to select the type.
3. **Edit** Click the button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select the parameter. **Restore default value** or **Restore current value** Click the button.
6. **Save** Click the button.
7. Review the preview of the modifications.
8. **Apply** or **Temporary application**Click to save the changes.

{% hint style="info" %}
**Note**

A parameter name highlighted in blue is a value that is currently being modified.
{% endhint %}

## Targets that can be modified by instance state <a href="#editable-by-instance-status" id="editable-by-instance-status"></a>

<table><thead><tr><th>Modification target</th><th>Modification conditions</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance state is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>Instance Status <code>Unavailable</code>When (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter edit mode while the instance is down and save without entering a value, it is processed as the reference value (Config value takes priority; if none, the default value).

{% hint style="info" %}
**Note**

In a Tibero multi-node configuration, global parameters cannot be modified.
{% endhint %}

## Save method by modified parameter type <a href="#save-method-by-parameter-type" id="save-method-by-parameter-type"></a>

<table><thead><tr><th>Modified parameter type</th><th>Save method</th></tr></thead><tbody><tr><td>Mix of dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are applied after a DB restart (connection sessions are terminated; takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Immediately applied to the current value without a restart</li><li><strong>Temporary application</strong>: Applied to the current value without a restart; reverts to the existing Config value when the DB is restarted</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary application is supported only by the Tibero engine. Since OpenSQL does not support temporary application, when modifying parameters **Apply**can only be used.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Since even invalid values are saved without errors, check the OpenHA log after modifying. `[ERROR]` The log also includes a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and reset it to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — `ttl` A value that cannot be converted was entered in an item that must be a number, such as. Check the input value and reset it.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — `maximum_lag_on_failover`A value that is not filtered out at save time, such as, caused an exception at actual runtime. Check the value and reset it.
{% endhint %}

## Load template <a href="#load-template" id="load-template"></a>

In Tibero parameter edit mode, load a previously saved parameter template and apply it to the modification list in bulk.

{% hint style="info" %}
**Note**

Loading templates is only supported in the Tibero engine. In OpenSQL, **Load** the button is not displayed.
{% endhint %}

1. In parameter edit mode, **Load** Click the button.
2. Select the template to apply from the template list.
3. Check the preview.
4. **Apply** Click the button.
5. Once the template is reflected in the edit list, **Save**click to apply the parameters.

---

# Parameter Templates <a href="#parameter-templates" id="parameter-templates"></a>

**Management > Parameters > Templates** On this page, you can view and apply parameter templates. You can load a predefined template to configure multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates are only available in the Tibero engine. When using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speeds and high transaction frequency |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. **Management > Parameters > Templates**Navigate to
2. Check the parameter template list.
3. Clicking a template name lets you view the detailed parameters of that template in the drawer.
4. **Apply** Click the button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. **Apply** Click the button.

---

# Parameter Change History <a href="#parameter-change-history" id="parameter-change-history"></a>

**Management > Parameters > Change History** On this page, you can view the change history of database parameters.

You can view the history list grouped by change request, and click each change to view detailed information about the modified parameters, including their previous value, modified value, whether they are dynamic, and the application method. You can add or edit descriptions for the change history, allowing you to record the reasons for changes.

1. **Management > Parameters > Change History**.
2. Select the desired period from the query period dropdown.
3. Check the parameter change history in the table.

## Viewing Change History Details <a href="#change-history-details" id="change-history-details"></a>

1. To check in the change history table, **Modified date**.
2. Check the list of modified parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Editing Change History Descriptions <a href="#edit-change-description" id="edit-change-description"></a>

{% hint style="warning" %}
**Caution**

After entering edit mode, **Save** if you do not click the button, the changes will not be saved.
{% endhint %}

1. In the change history table, **Modified date**click to open the drawer.
2. At the top of the drawer, **Edit** Click the button.
3. Enter the reason or description for the change in the description input field.
4. **Save** Click the button.
