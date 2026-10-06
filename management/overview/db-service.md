Query the status of databases running in OwlDB, and perform operations such as modify, stop, start, delete, role switchover, license renewal, and spec change.

{% hint style="info" %}
**Note**

- If the instance status is `Running`not, some information may be missing.
- The database management page displays times based on the database timezone, so there may be a difference from your local system time (browser time).
{% endhint %}

---

## Querying database information <a href="#database-info" id="database-info"></a>

1. **Management > Overview** Click the menu.
2. **DB Service Alias** Click the dropdown button to select the database whose information you want to query.
3. Check detailed information in the Operation Info, Instance, Installation Info, (DR/HA) Switchover History Management, and (BYOL) License tabs.

{% hint style="info" %}
**Note**

The database operation status can be checked on the **Overall Status Summary** page.
{% endhint %}

{% hint style="info" %}
**Note**

If you are using the BYOL license model and the license expiration date is within 3 months, an expiration notice banner is displayed at the top of the screen. In the banner, **Go to the renewal page** clicking the link allows you to navigate to the license renewal screen.
{% endhint %}

{% tabs %}
{% tab title="Operation Info" %}
<figure>
<img src="../../.gitbook/assets/image-87ac079b.png" alt="">
<figcaption>Figure 1. Operation Info</figcaption>
</figure>

You can check detailed database information such as account information that can access the relevant database, control files, logs, and checkpoints, and you can visually check the database configuration as a diagram.

{% hint style="info" %}
**Note**

All items that display a date and time are displayed based on the database timezone.
{% endhint %}
{% endtab %}
{% tab title="Instance" %}
<figure>
<img src="../../.gitbook/assets/image-121f711d.png" alt="">
<figcaption>Figure 2. Instance</figcaption>
</figure>

Check the list and information of the configured instances.

- **Instance alias**Clicking[Instance management](#dF57s45IXBUgU7RX1UvL)navigates to the "" page.
- After first selecting one or more instances with the ☑️ icon, **Restart** click the button, or without any selection, **Restart** click the button directly, and in the modal that opens you can select the instances to restart and the restart options. For details, refer to "[Restarting an instance](#undefined-2)".

{% hint style="info" %}
**Note**

When using DR configuration, the Primary (Leader) DB and Standby (Replica) DB are queried separately.
{% endhint %}
{% endtab %}
{% tab title="Installation Info" %}
Check the database version information, system and compile information, and patch (or Extension) information.

The system and compile information displays binary OS information and so on as a list, independent of the engine. Basic Info and patch information differ depending on the engine as follows.

<table><thead><tr><th>Item</th><th>Tibero</th><th>OpenSQL</th></tr></thead><tbody><tr><td>Basic Info</td><td><ul><li>Major version</li><li>Minor version</li><li>Patch set version</li></ul></td><td><ul><li>OpenSQL version (e.g., 3.0)</li><li>PostgreSQL version (e.g., 17.5)</li></ul></td></tr><tr><td>System and compile information</td><td>Displays binary OS information and so on as a list</td><td>Displays binary OS information and so on as a list</td></tr><tr><td>Patch information / Extensions</td><td><ul><li>Displays a list of the applied patch status</li><li>If there are none, the message "There are no applied patches" is displayed</li></ul></td><td><ul><li>Displays a list of the currently installed Extensions</li><li>Extensions added via DDL during operation are also reflected as of the query time</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Items whose values cannot be queried are displayed as `-`.
{% endhint %}
{% endtab %}
{% tab title="(BYOL) License" %}
This is a tab provided in the BYOL license model.

- Check the license in use and the information of the allocated instances.
- You can also check information about expired licenses.

{% hint style="info" %}
**Note**

If an idle license exists, **Rebuild** through the button, you can create instances and allocate licenses according to the existing license configuration.
{% endhint %}
{% endtab %}
{% tab title="(DR/HA) Switchover History Management" %}
This is a tab provided when using a Tibero DR configuration or an OpenSQL HA configuration.

Check the history of database role switchover events that have occurred. When a switchover event completes, the history is added.

<table><thead><tr><th>Column name</th><th>Description</th><th>Data format</th><th>Default value</th><th>Required value</th></tr></thead><tbody><tr><td>ID</td><td><ul><li>A number that uniquely identifies the switchover event</li><li>Format: {event type-random string 16 bytes}</li><li>Event type: FO / SO / FB</li></ul></td><td><ul><li>FO-3f9a7c1e2d8b45f0</li><li>SO-b17e4a93d2c68f5e</li><li>FB-7a2d9e14c6b83f05</li></ul></td><td>O</td><td>X</td></tr><tr><td>Start time</td><td>The time at which the switchover event occurred</td><td>yyyy.mm.dd HH\:mm:ss</td><td>O</td><td>O</td></tr><tr><td>Completion time</td><td>The time at which the switchover event completed</td><td>yyyy.mm.dd HH\:mm:ss</td><td>X</td><td>X</td></tr><tr><td>Type</td><td>Switchover event type</td><td><ul><li>Failover</li><li>Switchover</li><li>Failback</li></ul></td><td>O</td><td>O</td></tr><tr><td>Target</td><td>The target that performed the event</td><td><ul><li>Switchover: {user ID}</li><li>Failover: {user ID} / system(Auto Failover)</li><li>Failback: {user ID}</li></ul></td><td>O</td><td>X</td></tr><tr><td>Result</td><td>Displays the status of the event</td><td><ul><li>Success</li><li>Failure</li></ul></td><td>O</td><td>X</td></tr><tr><td>Cause/Remarks</td><td>Displays the cause of the event, the value entered by the user, or the failure reason</td><td><ul><li>A value optionally entered by the user during role switching (up to 200 characters, blank if not entered)</li><li>Trigger conditions for Auto Failover</li><li>On Failback failure: Standby/Replica Reboot Failed or Switchover Failed</li><li>On Failover failure: Standby/Replica Promotion Failed</li><li>On post-processing failure after a successful Failover: Cluster Normalization Failed (Primary scale out failed / New Standby/Replica creation failed, displayed with commas if multiple failures occur)</li></ul></td><td>O</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

---

## Editing DB Service information <a href="#edit-db-service" id="edit-db-service"></a>

1. Next to the DB Service alias **Pencil icon**Click.
2. Edits the DB Service alias and description.
3. **Save** Click the button.
