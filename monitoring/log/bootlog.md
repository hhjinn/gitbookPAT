On the **Bootlog** page, you can check status and error information related to the DB boot procedure and diagnose the cause of problems. You can view logs by boot/down event, and clicking each message shows the full detailed log in the right side panel.

## Viewing the Bootlog <a href="#view-bootlog" id="view-bootlog"></a>

The Bootlog screen displays the boot/shutdown event history of the selected DB instance. After narrowing down the logs you want with event, mode, and status filters and message search, click a message link to view the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

The Bootlog cannot be viewed when the instance is in the `Terminating` state.
{% endhint %}

**Query Period**

Set the period to view at the top of the screen. Choose from the last 1 day, 3 days, 7 days, or 1 month, or specify a start date and end date by direct input.

1. Click **Monitoring > Log Monitoring > Bootlog**.
2. Select the period to view.
3. Set the event, mode, and status filters to narrow the scope of the query.
4. Click the **message** link of the log you want to check.
5. Check the details of the log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only by the Tibero engine.
{% endhint %}

### Auto Refresh <a href="#undefined" id="undefined"></a>

To update the monitoring screen in real time, an auto refresh feature is supported.

1. Click the ⚙️ icon in the top left.
2. Set the refresh interval.
