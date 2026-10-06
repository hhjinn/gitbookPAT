The Eventlog page displays event logs generated according to rules defined by OwlDB.

**Monitoring > Log Monitoring > Eventlog** In this menu, you can check logs generated for the DB Service and filter out only the desired events by combining the query period and filters. Results are displayed in order of most recent received date.

{% hint style="info" %}
**Note**

If the DB Service is in the `Terminating` In this state, Eventlog cannot be queried. An information banner is displayed at the top of the screen.
{% endhint %}

1. **Monitoring > Log Monitoring > Eventlog** Click the menu.
2. Select the query period. To specify a particular range, **Direct input**after selecting it, set the start date and end date.
3. Select a status or message filter to narrow down the event types to query.
4. To find a specific message, enter a keyword in the search box.
5. In the query results, **DB Service** or **Instance** click the name to navigate to the corresponding detail page.

The event log types displayed in the message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {changed status}. ({status code}) |
| Warning | CPU usage exceeds 50% | CPU usage has exceeded the caution level (50%). (Current: {usage}%) |
|   | Memory usage exceeds 50% | Memory usage has exceeded the caution level (50%). (Current: {usage}%) |
| Error | CPU usage exceeds 90% | CPU usage has exceeded the warning level (90%). (Current: {usage}%) |
|   | Memory usage exceeds 90% | Memory usage has exceeded the warning level (90%). (Current: {usage}%) |
