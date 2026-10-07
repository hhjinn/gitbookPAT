**Management > Parameters > Settings** On this page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, where changes are reflected without a restart but are restored when the database is restarted.

The range of parameters that can be viewed and modified varies depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select the Primary instance.
{% endhint %}

# Viewing parameters <a href="#view-parameters" id="view-parameters"></a>

**Management > Parameters > Settings** When you enter the menu, the parameter list appears.

Tibero immediately displays the DB parameter list. OpenSQL (Azure) **DB** tab and **OpenHA** tab to view each parameter.

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL, **DB** or **OpenHA** Click the tab to select the type of parameter to view.
3. Apply a search or filter to find the parameter you want. You can filter by whether a parameter is dynamic. You can also search directly by name and parameter value.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data Format</td><td>Parameter data type</td></tr><tr><td>Default value</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current value</td><td>The value currently applied to the DB</td></tr><tr><td>Config value</td><td>The value that will be reflected when the DB is restarted</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- If the instance status is `Unavailable`In this case, the current value is not displayed, and the Config value is displayed instead.
- OpenSQL **OpenHA** In the tab, `pg_hba`and `slot` parameters cannot be viewed or modified. These parameters are **Connection information management** configured only from the menu.
{% endhint %}

# Modifying parameters <a href="#modify-parameters" id="modify-parameters"></a>

In the parameter list, **Edit** Clicking the button switches to edit mode, where you can directly edit parameter values in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, your changes will not be saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, **Edit** Only the tab selected at the time the button was clicked (**DB** or **OpenHA**) switches to an editable state. While editing, you cannot move to another tab; move only after saving or canceling.
{% endhint %}

1. **Management > Parameters > Settings** Click the menu.
2. When using OpenSQL **DB** or **OpenHA** Click the tab to select the type.
3. **Edit** Click the button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select the parameter. **Restore default value** or **Restore current value** Click the button.
6. **Save** Click the button.
7. Review the preview of your changes.
8. **Apply** or **Temporary application**Click to save your changes.

{% hint style="info" %}
**Note**

Parameter names highlighted in blue are values currently being modified.
{% endhint %}

## Editable targets by instance status <a href="#editable-by-instance-status" id="editable-by-instance-status"></a>

<table><thead><tr><th>Modification target</th><th>Modification conditions</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance status is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>Instance Status <code>Unavailable</code>When (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter edit mode while the instance is down and save without entering a value, it is processed as the reference value (Config value takes priority; if none, the default value).

{% hint style="info" %}
**Note**

Global parameters cannot be modified in a Tibero multi-node configuration.
{% endhint %}

## Saving method by modified parameter type <a href="#save-method-by-parameter-type" id="save-method-by-parameter-type"></a>

<table><thead><tr><th>Modified parameter type</th><th>Saving method</th></tr></thead><tbody><tr><td>Mixed dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are reflected after a DB restart (connection sessions are terminated; takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Reflected immediately in the current value without a restart</li><li><strong>Temporary application</strong>: Reflected in the current value without a restart; restored to the existing Config value when the DB is restarted</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary application is supported only by the Tibero engine. OpenSQL does not support temporary application, so when modifying parameters, **Apply**can only be used.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Even if you enter an invalid value, it is saved without an error, so check the OpenHA logs after modifying them. `[ERROR]` The logs also include a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and set it back to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — `ttl` A value that cannot be converted was entered for an item that must be a number, such as. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — `maximum_lag_on_failover`A value that was not filtered out at save time, such as, caused an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Loading a template <a href="#load-template" id="load-template"></a>

In Tibero parameter edit mode, load a pre-saved parameter template to apply it to the modification list in bulk.

{% hint style="info" %}
**Note**

Loading templates is supported only by the Tibero engine. In OpenSQL, the **Load** button is not displayed.
{% endhint %}

1. In parameter edit mode, **Load** Click the button.
2. Select the template to apply from the template list.
3. Check the preview.
4. **Apply** Click the button.
5. Once the template is reflected in the edit list, **Save**click to apply the parameters.

---

# Parameter Templates <a href="#parameter-templates" id="parameter-templates"></a>

**Management > Parameters > Templates** On this page, you can view and apply parameter templates. You can load a predefined template to set multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates are available only on the Tibero engine. When using the OpenSQL engine, the parameter template menu is not displayed.
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
3. Click a template name to view the parameter details of that template in the drawer.
4. **Apply** Click the button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. **Apply** Click the button.

---

# Parameter Modification History <a href="#parameter-change-history" id="parameter-change-history"></a>

**Management > Parameters > Modification History** On this page, you can view the change history of database parameters.

You can view the history list grouped by modification request, and click each modification to view in detail the previous value, modified value, dynamic status, and application method of the changed parameters. You can add or edit a description for a modification history, allowing you to keep a record of the reason for the change.

1. **Management > Parameters > Modification History**Click.
2. Select the desired period from the inquiry period dropdown.
3. Check the parameter modification history in the table.

## Viewing Modification History Details <a href="#change-history-details" id="change-history-details"></a>

1. To check in the modification history table **Modification date**Click.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Editing the Modification History Description <a href="#edit-change-description" id="edit-change-description"></a>

{% hint style="warning" %}
**Caution**

After entering edit mode, **Save** If you do not click the button, your changes will not be saved.
{% endhint %}

1. In the modification history table, **Modification date**click to open the drawer.
2. At the top of the drawer, **Edit** Click the button.
3. Enter the reason or description for the change in the description input field.
4. **Save** Click the button.
