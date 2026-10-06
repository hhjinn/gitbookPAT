**Bootlog** On this page, you can check the status and error information related to the DB boot process and diagnose the cause of issues. Logs are viewed by boot·down event, and clicking each message shows the full detailed log in the right-side panel.

## View Bootlog <a href="#view-bootlog" id="view-bootlog"></a>

The Bootlog screen displays the boot and shutdown event history of the selected DB instance. After narrowing down the logs you want using the event, mode, and status filters and the message search, click a message link to view the full detailed log in the right-side panel.

{% hint style="info" %}
**Note**

When the instance is in the `Terminating` state, the Bootlog cannot be viewed.
{% endhint %}

**Query Period**

Set the period to view at the top of the screen. Choose from the last 1 day, 3 days, 7 days, or 1 month, or specify a start date and end date by direct input.

1. **Monitoring > Log Monitoring > Bootlog**Click it.
2. Select the period to view.
3. Set the event, mode, and status filters to narrow the search scope.
4. Of the log you want to check, click the **Message** link.
5. Check the detailed contents of the log in the right-side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is only provided by the Tibero engine.
{% endhint %}

### Auto refresh <a href="#undefined" id="undefined"></a>

To provide real-time updates on the monitoring screen, an auto-refresh feature is supported.

1. Click the ⚙️ icon at the top left.
2. Set the refresh interval.
