Query the status of databases running in OwlDB and perform operations such as modify, stop, start, delete, role switch, license renewal, and spec change.

{% hint style="info" %}
**Note**

- If the instance status is not `Running`, some information may be missing.
- Because the database management page displays times based on the database timezone, there may be a difference from the local system time (browser time).
{% endhint %}

---

## View Database Information <a href="#database-info" id="database-info"></a>

1. Click the **Management > Overview** menu.
2. Click the **DB Service Name** dropdown button and select the database whose information you want to view.
3. Check the detailed information in the Operational Information, Instance, Version, and (DR/HA) Switch History Management tabs.

{% hint style="info" %}
**Note**

For the database operational status, please refer to the **Overall Status Summary Information** page.
{% endhint %}

{% tabs %}
{% tab title="Operational Information" %}
<figure>
<img src="../../.gitbook/assets/image-dffc3a8e.png" alt="">
<figcaption>Figure 1. Operational Information</figcaption>
</figure>

You can check detailed database information such as the account information that can access the database, control files, logs, and checkpoints, and you can visually check the database configuration as a diagram.

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

- Clicking an **instance alias** moves you to the "[Instance Management](#dF57s45IXBUgU7RX1UvL)" page.
- You can first select one or more instances with the ☑️ icon and then click the **Restart** button, or click the **Restart** button directly without a selection to open a modal where you can select the instances to restart and the restart options. For details, please refer to "[Restart Instance](#undefined-2)".

{% hint style="info" %}
**Note**

When using a DR configuration, the query distinguishes between the Primary (Leader) DB and the Standby (Replica) DB.
{% endhint %}
{% endtab %}
{% tab title="Installation Information" %}
Check the database version information, system and compile information, and patch (or Extension) information.

The system and compile information displays binary OS information and so on as a list, regardless of the engine. The Basic Info and patch information vary by engine as follows.

<table><thead><tr><th>Item</th><th>Tibero</th><th>OpenSQL</th></tr></thead><tbody><tr><td>Basic Info</td><td><ul><li>Major version</li><li>Minor version</li><li>Patchset version</li></ul></td><td><ul><li>OpenSQL version (e.g., 3.0)</li><li>PostgreSQL version (e.g., 17.5)</li></ul></td></tr><tr><td>System and Compile Information</td><td>Displays a list of binary OS information and so on</td><td>Displays a list of binary OS information and so on</td></tr><tr><td>Patch Information / Extensions</td><td><ul><li>Displays a list of the applied patch status</li><li>If there are none, the message "No patches have been applied" is displayed</li></ul></td><td><ul><li>Displays a list of the currently installed Extensions</li><li>Extensions added via DDL during operation are also reflected as of the time of the query</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Items whose value cannot be retrieved are displayed as `-`.
{% endhint %}
{% endtab %}
{% tab title="(DR/HA) Switch History Management" %}
This tab is provided for a Tibero DR configuration or an OpenSQL HA configuration.

Check the history of database role switch events that have occurred.

{% hint style="info" %}
**Note**

For OpenSQL, the role switch is performed by Patroni, and OwlDB checks that the node role has changed and reflects it in the history. At this time, since it does not distinguish whether the switch is a Switchover performed by the user or a Failover performed by Patroni, the type is all recorded as `Failover`.
{% endhint %}

<table><thead><tr><th>Column name</th><th>Description</th><th>Data type</th><th>Default value</th><th>Required value</th></tr></thead><tbody><tr><td>ID</td><td><ul><li>A number that uniquely identifies the switch event</li><li>Format: {event type-random string 16 bytes}</li><li>Event type: FO / SO / FB</li></ul></td><td><ul><li>FO-3f9a7c1e2d8b45f0</li><li>SO-b17e4a93d2c68f5e</li><li>FB-7a2d9e14c6b83f05</li></ul></td><td>O</td><td>X</td></tr><tr><td>Start Time</td><td>The time at which the switch event occurred</td><td>yyyy.mm.dd HH\:mm:ss</td><td>O</td><td>O</td></tr><tr><td>Completion Time</td><td>The time at which the switch event was completed</td><td>yyyy.mm.dd HH\:mm:ss</td><td>X</td><td>X</td></tr><tr><td>Type</td><td>Switch Event Type</td><td><ul><li>Failover</li><li>Switchover</li><li>Failback</li></ul></td><td>O</td><td>O</td></tr><tr><td>Execution Target</td><td>The target that performed the event</td><td><ul><li>Switchover: {user ID}</li><li>Failover: {user ID} / system (Auto Failover)</li><li>Failback: {user ID}</li></ul></td><td>O</td><td>X</td></tr><tr><td>Result</td><td>Displays the status of the event</td><td><ul><li>Success</li><li>Failure</li></ul></td><td>O</td><td>X</td></tr><tr><td>Cause/Remarks</td><td>Displays the cause of the event, the value entered by the user, or the failure reason</td><td><ul><li>A value optionally entered by the user during the role switch (up to 200 characters, blank if not entered)</li><li>The trigger condition for Auto Failover</li><li>On Failback failure: Standby/Replica Reboot Failed or Switchover Failed</li><li>On Failover failure: Standby/Replica Promotion Failed</li><li>On post-processing failure after a successful Failover: Cluster Normalization Failed (Primary scale out failed / New Standby/Replica creation failed; if multiple failures, displayed separated by commas)</li></ul></td><td>O</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

---

## Edit DB Service information <a href="#edit-db-service" id="edit-db-service"></a>

1. Click the **pencil icon** next to the DB Service alias.
2. Edit the DB Service alias and description.
3. Click the **Save** button.
