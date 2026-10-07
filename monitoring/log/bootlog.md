On the **Bootlog** page, you can check status and error information related to the DB boot procedure and diagnose the causes of problems. You can view logs by boot and down event, and clicking each message lets you check the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When an instance is in the `Terminating` state, you cannot query Bootlog.
{% endhint %}

**Query period**

At the top of the screen, set the period to query. Select from the last 1 day, 3 days, 7 days, or 1 month, or specify the start and end dates by direct input.

1. Click **Monitoring > Log Monitoring > Bootlog**.
2. Select the period to query.
3. Set the event, mode, and status filters to narrow the query range.
4. Click the **message** link of the log you want to check.
5. Check the detailed content of the log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only in the Tibero engine.
{% endhint %}

# Auto refresh <a href="#auto-refresh" id="auto-refresh"></a>

To provide real-time updates on the monitoring screen, an auto refresh feature is supported.

1. Click the ⚙️ icon at the top left.
2. Set the refresh interval.
