On the **Management > Parameters > Settings** page, you can view and modify the parameter values required for database operation.

Each parameter displays its name, data type, default value, current value, Config value, and whether it is a dynamic parameter. Dynamic parameters can be applied immediately without restarting the database, while static parameters require a restart after modification. Tibero also supports temporary application, which reflects changes without a restart but is restored upon restart.

The range of parameters that can be viewed and modified differs depending on the DB engine. Tibero provides only DB parameters, while OpenSQL provides DB parameters and OpenHA parameters, separated into tabs.

{% hint style="warning" %}
**Caution**

Parameters cannot be modified while a Standby (Recovery) instance is selected. To modify them, you must select the Primary instance.
{% endhint %}

# Viewing parameters <a href="#view-parameters" id="view-parameters"></a>

When you enter the **Management > Parameters > Settings** menu, the parameter list appears.

Tibero displays the DB parameter list directly. OpenSQL (Azure) lets you switch between the **DB** tab and the **OpenHA** tab to view each set of parameters.

1. Click the **Management > Parameters > Settings** menu.
2. If you are using OpenSQL, click the **DB** or **OpenHA** tab to select the type of parameter to view.
3. Apply a search or filter to find the parameter you want. You can filter by whether a parameter is dynamic. You can also search directly by name and parameter value.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Parameter name</td></tr><tr><td>Data type</td><td>Parameter data type</td></tr><tr><td>Default value</td><td>The value applied when the user has not set it separately</td></tr><tr><td>Current value</td><td>The value currently applied to the DB</td></tr><tr><td>Config value</td><td>The value that will take effect when the DB is restarted</td></tr><tr><td>Dynamic parameter</td><td><ul><li><strong>Yes</strong>: Can be applied immediately without a restart</li><li><strong>No (restart required)</strong>: A DB restart is required to apply it</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- When the instance status is `Unavailable`, the current value is not displayed and the Config value is shown instead.
- On the OpenSQL **OpenHA** tab, the `pg_hba` and `slot` parameters cannot be viewed or modified. These parameters are configured only in the **Connection Information Management** menu.
{% endhint %}

# Modifying parameters <a href="#modify-parameters" id="modify-parameters"></a>

When you click the **Modify** button in the parameter list, the view switches to edit mode, where you can edit parameter values directly in the list.

{% hint style="warning" %}
**Caution**

- If you leave the screen without saving in edit mode, your changes are not saved.
- A banner indicating that editing is in progress is displayed at the top of the screen.
{% endhint %}

{% hint style="info" %}
**Note**

When you enter edit mode in OpenSQL, only the tab selected at the time you clicked the **Modify** button (**DB** or **OpenHA**) becomes editable. You cannot move to another tab while editing; move only after saving or canceling.
{% endhint %}

1. Click the **Management > Parameters > Settings** menu.
2. If you are using OpenSQL, click the **DB** or **OpenHA** tab to select the type.
3. Click the **Edit** button.
4. Modify the parameter value.
5. To revert a specific parameter to its original value, restore it. Select the parameter. Click the **Restore default value** or **Restore current value** button.
6. Click the **Save** button.
7. Review the preview of the changes.
8. Click **Apply** or **Apply temporarily** to save the changes.

{% hint style="info" %}
**Note**

A parameter name highlighted in blue is a value that is currently being modified.
{% endhint %}

## Items that can be modified by instance status <a href="#editable-by-instance-status" id="editable-by-instance-status"></a>

<table><thead><tr><th>Items to modify</th><th>Modification conditions</th></tr></thead><tbody><tr><td>Current value</td><td>When the instance status is normal (<code>Available</code> or <code>Limited</code>)</td></tr><tr><td>Config value</td><td><ul><li>When the instance status is <code>Unavailable</code> (DB Down/Nomount)</li><li>Parameters without a Config value cannot be modified</li></ul></td></tr></tbody></table>

If you enter edit mode while the instance is down and save without entering a value, it is handled using the baseline value (Config value takes priority; if none, the default value).

{% hint style="info" %}
**Note**

In a Tibero multi-node configuration, global parameters cannot be modified.
{% endhint %}

## Save method by modified parameter type <a href="#save-method-by-parameter-type" id="save-method-by-parameter-type"></a>

<table><thead><tr><th>Modified parameter type</th><th>Save method</th></tr></thead><tbody><tr><td>Mix of dynamic and static parameters</td><td><ul><li><strong>Apply</strong>: Changes are reflected after a DB restart (connection sessions are terminated, takes several minutes)</li></ul></td></tr><tr><td>Dynamic parameters only</td><td><ul><li><strong>Apply</strong>: Reflected immediately in the current value without a restart</li><li><strong>Apply temporarily</strong>: Reflected in the current value without a restart; restored to the existing Config value when the DB is restarted</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Temporary application is supported only by the Tibero engine. OpenSQL does not support temporary application, so you can only use **Apply** when modifying parameters.
{% endhint %}

{% hint style="warning" %}
**Caution**

OpenHA parameters are not validated when modified. Since an invalid value is saved without error, check the OpenHA log after modifying. The `[ERROR]` log also includes a stack trace.

- `[WARNING]: Violated the rule "loop_wait + 2*retry_timeout <= ttl"` — the value combination violates the constraint, so Patroni automatically adjusts the values. Check the adjusted values and set them again to your intended values.
- `[ERROR]: Exception when setting dynamic_configuration` — a value that cannot be converted was entered for an item that must be a number, such as `ttl`. Check the input value and set it again.
- `[ERROR]: Unexpected exception raised, please report it as a BUG` — a value that is not filtered at save time, such as `maximum_lag_on_failover`, caused an exception at actual runtime. Check the value and set it again.
{% endhint %}

## Loading a template <a href="#load-template" id="load-template"></a>

In Tibero parameter edit mode, load a previously saved parameter template and apply it in bulk to the modification list.

{% hint style="info" %}
**Note**

Template loading is supported only by the Tibero engine. In OpenSQL, the **Load** button is not displayed.
{% endhint %}

1. In parameter edit mode, click the **Load** button.
2. Select the template to apply from the template list.
3. Review the preview.
4. Click the **Apply** button.
5. Once the template is reflected in the modification list, click **Save** to apply the parameters.

---

# Parameter Template <a href="#parameter-templates" id="parameter-templates"></a>

On the **Management > Parameters > Templates** page, you can view and apply parameter templates. You can load a predefined template to set multiple parameters at once.

{% hint style="info" %}
**Note**

Parameter templates are only available on the Tibero engine. If you are using the OpenSQL engine, the parameter template menu is not displayed.
{% endhint %}

| Template | Description |
| --- | --- |
| OLAP | A template optimized for large-scale data analysis and complex queries |
| OLTP | A template designed to handle fast processing speeds and high transaction frequencies |

{% hint style="info" %}
**Note**

The built-in templates (OLAP, OLTP) cannot be modified or deleted.
{% endhint %}

1. Go to **Management > Parameters > Templates**.
2. Check the list of parameter templates.
3. Click a template name to view the detailed parameters of that template in the drawer.
4. Click the **Apply** button.
5. In the apply confirmation modal, review the list of parameters to be changed (name / current value / modified value).
6. Click the **Apply** button.

---

# Parameter Modification History <a href="#parameter-change-history" id="parameter-change-history"></a>

On the **Management > Parameters > Modification History** page, you can view the change history of database parameters.

You can check the history list grouped by modification request, and click each modification to view detailed information about the changed parameters, including their previous value, modified value, whether they are dynamic, and the application method. You can add or edit descriptions in the modification history to keep a record of the reasons for changes.

1. Click **Management > Parameters > Modification History**.
2. Select the desired period from the query period dropdown.
3. Check the parameter modification history in the table.

## Viewing Modification History Details <a href="#change-history-details" id="change-history-details"></a>

1. In the modification history table, click the **Modification Date** you want to check.
2. Check the list of changed parameters in the drawer that appears on the right side of the screen.
3. If necessary, find a specific parameter using the table filter or search.

## Editing Modification History Description <a href="#edit-change-description" id="edit-change-description"></a>

{% hint style="warning" %}
**Caution**

If you do not click the **Save** button after entering edit mode, your changes will not be saved.
{% endhint %}

1. In the modification history table, click the **Modification Date** to open the drawer.
2. Click the **Edit** button at the top of the drawer.
3. Enter the reason for the change or a description in the description input field.
4. Click the **Save** button.
