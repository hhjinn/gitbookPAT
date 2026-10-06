**Bootlog** On this page, you can check the status and error information related to the DB boot procedure and diagnose the cause of problems. You can look up logs by boot/down event, and clicking each message shows the full detailed log in the right side panel.

{% hint style="info" %}
**Note**

When the instance is in the `Terminating` state, Bootlog cannot be looked up.
{% endhint %}

**Query period**

At the top of the screen, set the period to look up. Choose from the last 1 day, 3 days, 7 days, or 1 month, or specify the start and end dates by direct input.

1. **Monitoring > Log Monitoring > Bootlog**Click.
2. Select the period to look up.
3. Set the event, mode, and status filters to narrow the lookup scope.
4. Click the **Message** link of the log you want to check.
5. Check the detailed contents of the log in the right side panel of the screen.

{% hint style="info" %}
**Note**

The mode filter is provided only in the Tibero engine.
{% endhint %}

### Auto Refresh <a href="#undefined" id="undefined"></a>

To enable real-time updates of the monitoring screen, an auto refresh feature is supported.

1. Click the ⚙️ icon in the upper left.
2. Sets the refresh interval.
