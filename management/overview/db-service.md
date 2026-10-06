Checks the status of databases running in OwlDB and performs operations such as modifying, stopping, starting, deleting, role switching, license renewal, and spec changes.

{% hint style="info" %}
**Note**

- If the instance status is not `Running`some information may be missing.
- Because the database management page displays times based on the database timezone, there may be a difference from the local system time (browser time).
{% endhint %}

---

## Viewing Database Information <a href="#database-info" id="database-info"></a>

1. **Management > Overview** Click the menu.
2. **DB Service Name** Click the dropdown button and select the database whose information you want to view.
3. Check the detailed information in the Operation Information, Instance, Version, and (DR/HA) Switch History Management tabs.

{% hint style="info" %}
**Note**

For the database operation status, **Overall Status Summary Information** please refer to the page.
{% endhint %}

{% tabs %}
{% tab title="Operation Information" %}
<figure>
<img src="../../.gitbook/assets/image-dffc3a8e.png" alt="">
<figcaption>Figure 1. Operation Information</figcaption>
</figure>

You can check detailed database information such as account information that can access the database, control files, logs, and checkpoints, and you can visually check the database configuration as a diagram.

{% hint style="info" %}
**Note**

All items that display a date and time are shown based on the database timezone.
{% endhint %}
{% endtab %}
{% tab title="Instance" %}
<figure>
<img src="../../.gitbook/assets/image-e38cc454.png" alt="">
<figcaption>Figure 2. Instance</figcaption>
</figure>

Check the list and information of the configured instances.

- **Instance alias**Clicking it takes you to the "[Instance management](#dF57s45IXBUgU7RX1UvL)" page.
- After first selecting one or more instances with the ☑️ icon, **Restart** click the button, or without selecting any, **Restart** click the button directly to open a modal where you can select the instances to restart and the restart options. For details, please refer to "[Restarting an Instance](#undefined-2)".

{% hint style="info" %}
**Note**

When using a DR configuration, the Primary (Leader) DB and the Standby (Replica) DB are looked up separately.
{% endhint %}
{% endtab %}
{% tab title="Installation Information" %}
Check the database version information, system and compile information, and patch (or Extension) information.

The system and compile information displays binary OS information and the like as a list, regardless of the engine. The Basic Info and patch information differ depending on the engine as follows.

<table><thead><tr><th>Item</th><th>Tibero</th><th>OpenSQL</th></tr></thead><tbody><tr><td>Basic Info</td><td><ul><li>Major version</li><li>Minor version</li><li>Patch set version</li></ul></td><td><ul><li>OpenSQL version (e.g., 3.0)</li><li>PostgreSQL version (e.g., 17.5)</li></ul></td></tr><tr><td>System and Compile Information</td><td>Displays a list of binary OS information and the like</td><td>Displays a list of binary OS information and the like</td></tr><tr><td>Patch Information / Extensions</td><td><ul><li>Displays a list of the applied patch status</li><li>If there are none, displays the message "There are no applied patches"</li></ul></td><td><ul><li>Displays a list of the currently installed Extensions</li><li>Extensions added via DDL during operation are also reflected as of the time of lookup</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Items whose values cannot be looked up are `-`.
{% endhint %}
{% endtab %}
{% tab title="(DR/HA) Switch History Management" %}
This tab is provided for Tibero DR configurations or OpenSQL HA configurations.

Check the history of database role switchover events that have occurred.

{% hint style="info" %}
**Note**

Role switchover in OpenSQL is performed by Patroni, and OwlDB reflects it in the history by detecting that a node's Role has changed. At this point, since it does not distinguish whether the switchover is a Switchover performed by the user or a Failover performed by Patroni, the type is all recorded as `Failover`.
{% endhint %}

<table><thead><tr><th>Column Name</th><th>Description</th><th>Data Format</th><th>Default</th><th>Required</th></tr></thead><tbody><tr><td>ID</td><td><ul><li>A number that uniquely identifies the switchover event</li><li>Format: {event type-random string 16 bytes}</li><li>Event type: FO / SO / FB</li></ul></td><td><ul><li>FO-3f9a7c1e2d8b45f0</li><li>SO-b17e4a93d2c68f5e</li><li>FB-7a2d9e14c6b83f05</li></ul></td><td>O</td><td>X</td></tr><tr><td>Start Time</td><td>The time at which the switchover event occurred</td><td>yyyy.mm.dd HH\:mm:ss</td><td>O</td><td>O</td></tr><tr><td>Completion Time</td><td>The time at which the switchover event was completed</td><td>yyyy.mm.dd HH\:mm:ss</td><td>X</td><td>X</td></tr><tr><td>Type</td><td>Switchover event type</td><td><ul><li>Failover</li><li>Switchover</li><li>Failback</li></ul></td><td>O</td><td>O</td></tr><tr><td>Target</td><td>The target that performed the event</td><td><ul><li>Switchover: {user ID}</li><li>Failover: {user ID} / system (Auto Failover)</li><li>Failback: {user ID}</li></ul></td><td>O</td><td>X</td></tr><tr><td>Result</td><td>Displays the status of the event</td><td><ul><li>Success</li><li>Failure</li></ul></td><td>O</td><td>X</td></tr><tr><td>Cause/Remarks</td><td>Displays the cause of the event, the value entered by the user, or the reason for failure</td><td><ul><li>A value optionally entered by the user during role switchover (up to 200 characters, blank if not entered)</li><li>Trigger condition for Auto Failover</li><li>On Failback failure: Standby/Replica Reboot Failed or Switchover Failed</li><li>On Failover failure: Standby/Replica Promotion Failed</li><li>On post-processing failure after successful Failover: Cluster Normalization Failed (Primary scale out failed / New Standby/Replica creation failed, displayed with commas if multiple failures)</li></ul></td><td>O</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

---

## Edit DB Service Information <a href="#edit-db-service" id="edit-db-service"></a>

1. Next to the DB Service alias **Pencil icon**.
2. Edit the DB Service alias and description.
3. **Save** Click the button.
