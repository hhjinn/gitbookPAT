**Bootlog** On this page, you can check status and error information related to the DB boot procedure and diagnose the cause of problems. You can query logs by boot·down event, and clicking each message lets you view the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When the instance is in `Terminating` state, Bootlog cannot be queried.
{% endhint %}

**Query Period**

Set the query period at the top of the screen. Select from the last 1 day, 3 days, 7 days, or 1 month, or specify the start and end dates by direct input.

1. **Monitoring > Log Monitoring > Bootlog**Click.
2. Select the query period.
3. Set the event, mode, and status filters to narrow down the query scope.
4. Of the log you want to check, the **Message** click the link.
5. Check the detailed content of the log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only in the Tibero engine.
{% endhint %}

### Auto refresh

For real-time updates of the monitoring screen, an auto refresh function is supported.

1. Click the ⚙️ icon at the top left.
2. Set the refresh interval.
