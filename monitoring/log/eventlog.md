On the Eventlog page, you can view event logs that occurred according to rules defined by OwlDB.

**Monitoring > Log Monitoring > Eventlog** In this menu, you can check logs that occurred in the DB Service, and combine the search period and filters to extract only the events you want. Results are displayed in order of most recent reception date.

{% hint style="info" %}
**Note**

If the DB Service is in `Terminating` In this state, the Eventlog cannot be viewed. A notice banner is displayed at the top of the screen.
{% endhint %}

1. **Monitoring > Log Monitoring > Eventlog** Click the menu.
2. Select the search period. To specify a particular range, select **Direct input**and then set the start date and end date.
3. Select the status or message filter to narrow the event types to view.
4. To find a specific message, enter a keyword in the search box.
5. In the search results, **DB Service** or **Instance** click the name to navigate to the corresponding detail page.

The event log types displayed in the message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {changed status}. ({status code}) |
| Warning | CPU usage exceeds 50% | CPU usage has exceeded the caution level (50%). (Current: {usage}%) |
|   | Memory usage exceeds 50% | Memory usage has exceeded the caution level (50%). (Current: {usage}%) |
| Error | CPU usage exceeds 90% | CPU usage has exceeded the warning level (90%). (Current: {usage}%) |
|   | Memory usage exceeds 90% | Memory usage has exceeded the warning level (90%). (Current: {usage}%) |
