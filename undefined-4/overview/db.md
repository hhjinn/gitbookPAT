Queries the status of databases running in OwlDB and performs operations such as modify, stop, start, delete, role switchover, license renewal, and spec changes.

{% hint style="info" %}
**Note**

- When the instance status `Running`is not, some information may be missing.
- Since the database management page displays times based on the database timezone, there may be a difference from the local system time (browser time).
{% endhint %}

---

## Viewing Database Information

1. **Management > Overview** Click the menu.
2. **DB Service Name** Click the dropdown button to select the database whose information you want to view.
3. Check the detailed information in the Operation Info, Instance, Version, and (DR/HA) Switchover History Management tabs.

{% hint style="info" %}
**Note**

For the database operation status, **Overall Status Summary Information** please refer to the page.
{% endhint %}

{% tabs %}
{% tab title="Operation Info" %}
> 📷 **[이미지]** 이미지

You can check detailed database information such as account information that can access the database, control files, logs, and checkpoints, and you can visually verify the database configuration as a diagram.

{% hint style="info" %}
**Note**

All items that display a date and time are shown based on the database timezone.
{% endhint %}
{% endtab %}
{% tab title="Instance" %}
> 📷 **[이미지]** 이미지

Check the list and information of the configured instances.

- **Instance alias**When you click, the "[Instance Management](#dF57s45IXBUgU7RX1UvL)" page opens.
- After first selecting one or more instances with the ☑️ icon, **Restart** click the button, or without selecting any, **Restart** click the button directly, and in the modal that opens you can select the instances to restart and the restart options. For details, please refer to "[Instance Restart](#undefined-2)".

{% hint style="info" %}
**Note**

When using a DR configuration, the Primary(Leader) DB and Standby(Replica) DB are queried separately.
{% endhint %}
{% endtab %}
{% tab title="Installation Info" %}
Check the database version information, system and compile information, and patch (or Extension) information.

System and compile information displays binary OS information and the like as a list, regardless of the engine. Basic Info and patch information differ depending on the engine as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Tibero</th><th>OpenSQL</th></tr></thead><tbody><tr><td>Basic Info</td><td><ul><li>Major version</li><li>Minor version</li><li>Patchset version</li></ul></td><td><ul><li>OpenSQL version (e.g., 3.0)</li><li>PostgreSQL version (e.g., 17.5)</li></ul></td></tr><tr><td>System and compile information</td><td>Displays a list of binary OS information and the like</td><td>Displays a list of binary OS information and the like</td></tr><tr><td>Patch Information / Extensions</td><td><ul><li>Displays a list of applied patch status</li><li>If there are none, displays the message "There are no applied patches"</li></ul></td><td><ul><li>Displays a list of currently installed Extensions</li><li>Extensions added via DDL during operation are also reflected as of the time of the query</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Items whose values cannot be retrieved are displayed as `-`.
{% endhint %}
{% endtab %}
{% tab title="(DR/HA) Switchover History Management" %}
This tab is provided for a Tibero DR configuration or an OpenSQL HA configuration.

Check the history of database role switchover events that have occurred.

{% hint style="info" %}
**Note**

OpenSQL role switchover is performed by Patroni, and OwlDB checks that the node role has changed and reflects it in the history. At this time, since it does not distinguish whether the switchover was a Switchover performed by the user or a Failover performed by Patroni, the type is all recorded as `Failover`.
{% endhint %}

<table data-full-width="true"><thead><tr><th>Column name</th><th>Description</th><th>Data format</th><th>Default value</th><th>Required value</th></tr></thead><tbody><tr><td>ID</td><td><ul><li>A number that uniquely identifies the switchover event</li><li>Format: {event type-random string 16 bytes}</li><li>Event type: FO / SO / FB</li></ul></td><td><ul><li>FO-3f9a7c1e2d8b45f0</li><li>SO-b17e4a93d2c68f5e</li><li>FB-7a2d9e14c6b83f05</li></ul></td><td>O</td><td>X</td></tr><tr><td>Start time</td><td>The time when the switchover event occurred</td><td>yyyy.mm.dd HH:mm:ss</td><td>O</td><td>O</td></tr><tr><td>Completion time</td><td>The time at which the conversion event completed</td><td>yyyy.mm.dd HH:mm:ss</td><td>X</td><td>X</td></tr><tr><td>Type</td><td>Conversion event type</td><td><ul><li>Failover</li><li>Switchover</li><li>Failback</li></ul></td><td>O</td><td>O</td></tr><tr><td>Target</td><td>The target that performed the event</td><td><ul><li>Switchover: {user ID}</li><li>Failover: {user ID} / system(Auto Failover)</li><li>Failback: {user ID}</li></ul></td><td>O</td><td>X</td></tr><tr><td>Result</td><td>Displays the status of the event</td><td><ul><li>Success</li><li>Failure</li></ul></td><td>O</td><td>X</td></tr><tr><td>Cause/Remarks</td><td>Displays the cause of the event, the value entered by the user, or the reason for failure</td><td><ul><li>A value optionally entered by the user during role switching (up to 200 characters, blank if not entered)</li><li>Trigger condition for Auto Failover</li><li>On Failback failure: Standby/Replica Reboot Failed or Switchover Failed</li><li>On Failover failure: Standby/Replica Promotion Failed</li><li>On post-processing failure after a successful Failover: Cluster Normalization Failed(Primary scale out failed / New Standby/Replica creation failed, displayed with commas in case of multiple failures)</li></ul></td><td>O</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

---

## Editing DB Service information

1. Next to the DB Service alias **pencil icon**Click.
2. Edit the DB Service alias and description.
3. **Save** Click the button.
