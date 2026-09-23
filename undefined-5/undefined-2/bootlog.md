**Bootlog** Check the status and error information related to the DB boot process on this page, and diagnose the cause of problems. Query logs by boot·down event, and click each message to view the full detailed log in the right side panel.

## Bootlog Query

The Bootlog screen displays the boot·shutdown event history of the selected DB instance. After narrowing down the desired logs using the event·mode·status filters and message search, click a message link to view the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When the instance is `Terminating` in this state, the Bootlog cannot be queried.
{% endhint %}

**Query Period**

Set the period to query at the top of the screen. Choose from the last 1 day·3 days·7 days·1 month, or specify the start date·end date by direct input.

1. **Monitoring > Log Monitoring > Bootlog**Click.
2. Select the period to query.
3. Set the event, mode, and status filters to narrow the query scope.
4. Of the log to check, **message** click the link.
5. Check the details of the corresponding log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only by the Tibero engine.
{% endhint %}

### Auto Refresh

The auto refresh feature is supported for real-time updates of the monitoring screen.

1. Click the ⚙️ icon at the top left.
2. Set the refresh interval.
