**Bootlog** On this page, you can check the status and error information related to the DB boot procedure and diagnose the cause of problems. You can view logs for each boot/down event, and clicking each message lets you view the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When the instance is in the `Terminating` state, Bootlog cannot be queried.
{% endhint %}

**Query period**

Set the period to query at the top of the screen. Choose from the last 1 day, 3 days, 7 days, or 1 month, or specify a start date and end date by manual entry.

1. **Monitoring > Log Monitoring > Bootlog**Click.
2. Select the period to query.
3. Set the event, mode, and status filters to narrow the query scope.
4. Of the log you want to check, click the **Message** link.
5. Check the detailed content of that log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only in the Tibero engine.
{% endhint %}

# Auto Refresh <a href="#auto-refresh" id="auto-refresh"></a>

To enable real-time updates of the monitoring screen, an auto-refresh feature is supported.

1. Click the ⚙️ icon at the top left.
2. Sets the refresh interval.
