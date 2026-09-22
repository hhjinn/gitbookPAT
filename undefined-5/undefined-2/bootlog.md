**Bootlog** On this page, you can check status and error information related to the DB boot process and diagnose the causes of issues. You can view logs by boot/down event, and clicking each message displays the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When the instance is in `Terminating` state, Bootlog cannot be queried.
{% endhint %}

**Query Period**

Set the query period at the top of the screen. Choose from the last 1 day, 3 days, 7 days, or 1 month, or manually specify a start date and end date.

1. **Monitoring > Log Monitoring > Bootlog**Click.
2. Select the period to query.
3. Set the event, mode, and status filters to narrow the query scope.
4. Click the **Message** link for the log you want to check.
5. Check the detailed content of the log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only by the Tibero engine.
{% endhint %}

### Auto Refresh

The auto refresh feature is supported for real-time updates of the monitoring screen.

1. Click the ⚙️ icon at the top left.
2. Set the refresh interval.
