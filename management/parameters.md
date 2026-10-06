**Management > Parameters > Settings** On the page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, where changes are reflected without a restart but reverted upon restart.

The range of parameters that can be viewed and modified varies depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select a Primary instance.
{% endhint %}

# Viewing Parameters <a href="#view-parameters" id="view-parameters"></a>

**Management > Parameters > Settings** When you enter the menu, the parameter list appears.

Tibero displays the DB parameter list directly. OpenSQL (Azure) switches between the **DB** tab and the **OpenHA** tab to view the respective parameters.

1. **Management > Parameters > Settings** Click the menu.
2. If you are using OpenSQL, **DB** or **OpenHA** click the tab to select the type of parameter to view.
3. Apply a search or filter to find the parameter you want. You can filter by whether a parameter is dynamic. You can search directly by name and parameter value.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data format</td><td>Parameter data type</td></tr><tr><td>Default value</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current value</td><td>The value currently applied to the DB</td></tr><tr><td>Config value</td><td>The value that will be reflected when the DB is restarted</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be reflected immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to reflect it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- If the instance status is `Unavailable`In this case, the current value is not displayed, and the Config value is displayed instead.
- OpenSQL **OpenHA** In the tab, `pg_hba`and `slot` parameters cannot be viewed or modified. These parameters are configured only in the **Connection Information Management** menu.
{% endhint %}

# Modifying Parameters <a href="#modify-parameters" id="modify-parameters"></a>

In the parameter list, **Edit** Clicking the button switches to edit mode, where you can directly edit parameter values in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, the changes are not saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, **Edit** only the tab that was selected at the time the button was clicked (**DB** or **OpenHA**) is switched to an editable state. You cannot move to another tab while editing; move after saving or canceling.
{% endhint %}

1. **Management > Parameters > Settings** Click the menu.
2. If you are using OpenSQL, **DB** or **OpenHA** click the tab to select the type.
3. **Edit** Click the button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select a parameter. **Restore default value** or **Restore current value** Click the button.
6. **Save** Click the button.
7. Review the preview of the changes.
8. **Apply** or **Temporary Apply**Click to save the changes.

{% hint style="info" %}
**Note**

A parameter name highlighted in blue indicates a value currently being modified.
{% endhint %}

## Modifiable targets by instance state <a href="#editable-by-instance-status" id="editable-by-instance-status"></a>

<table><thead><tr><th>Modification target</th><th>Modification conditions</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance state is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>Instance status <code>Unavailable</code>When (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter modification mode while the instance is down and save without entering a value, it is processed as the baseline value (Config value takes priority, or the default value if there is none).

{% hint style="info" %}
**Note**

Global parameters cannot be modified in a Tibero multi-node configuration.
{% endhint %}

## Save method by modified parameter type <a href="#save-method-by-parameter-type" id="save-method-by-parameter-type"></a>

<table><thead><tr><th>Modified parameter type</th><th>Save method</th></tr></thead><tbody><tr><td>Mix of dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are applied after a DB restart (connection sessions are terminated, takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Applied immediately to the current value without a restart</li><li><strong>Temporary Apply</strong>: Applied to the current value without a restart; restored to the existing Config value when the DB restarts</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary Apply is supported only by the Tibero engine. OpenSQL does not support Temporary Apply, so when modifying parameters only **Apply**can be used.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Since an incorrect value is saved without error, check the OpenHA log after modification. `[ERROR]` The log also includes a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — The value combination violates a constraint, so Patroni automatically adjusts the value. Check the adjusted value and set it back to the intended value.
- `[ERROR]: Exception when setting dynamic_configuration` — `ttl` A value that cannot be converted was entered for an item that must be a number, such as. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — `maximum_lag_on_failover`A value that is not filtered at save time, such as, caused an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Load template <a href="#load-template" id="load-template"></a>

In Tibero parameter modification mode, load a previously saved parameter template and apply it in bulk to the modification list.

{% hint style="info" %}
**Note**

Loading templates is supported only by the Tibero engine. In OpenSQL, the **Load** button is not displayed.
{% endhint %}

1. In parameter modification mode, **Load** Click the button.
2. Select the template to apply from the template list.
3. Review the preview.
4. **Apply** Click the button.
5. Once the template is reflected in the modification list, **Save**Click to apply the parameters.

---

# Parameter template <a href="#parameter-templates" id="parameter-templates"></a>

**Management > Parameters > Templates** Query and apply parameter templates on the page. You can load a predefined template to set multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates can be used only by the Tibero engine. If you are using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speed and high transaction frequency |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. **Management > Parameters > Templates**Navigate to.
2. Check the parameter template list.
3. Click a template name to view the parameter details of that template in the drawer.
4. **Apply** Click the button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. **Apply** Click the button.

---

# Parameter modification history <a href="#parameter-change-history" id="parameter-change-history"></a>

**Management > Parameters > Modification History** On the page, query the change history of database parameters.

Review the history list grouped by modification request, and click each modification to view in detail the previous value, modified value, dynamic status, and apply method of the changed parameters. You can add or edit a description for the modification history, allowing you to keep a record of the reason for the change.

1. **Management > Parameters > Modification History**Click.
2. Select the desired period from the query period dropdown.
3. Check the parameter modification history in the table.

## View modification history details <a href="#change-history-details" id="change-history-details"></a>

1. In the modification history table, the **Modification date**Click.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Edit modification history description <a href="#edit-change-description" id="edit-change-description"></a>

{% hint style="warning" %}
**Caution**

After entering modification mode, if you **Save** If you do not click the button, the changes are not saved.
{% endhint %}

1. In the modification history table, the **Modification date**Click to open the drawer.
2. At the top of the drawer, the **Edit** Click the button.
3. Enter the reason or description for the change in the description input field.
4. **Save** Click the button.
