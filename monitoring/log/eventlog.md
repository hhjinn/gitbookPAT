On the Eventlog page, you can query event logs that occurred according to the rules defined by OwlDB.

**Monitoring > Log Monitoring > Eventlog** In this menu, you can check logs that occurred in the DB Service, and by combining the query period and filters, you can filter out only the events you want. Results are displayed in order of the most recent received date.

{% hint style="info" %}
**Note**

If the DB Service is in `Terminating` If it is in state, Eventlog cannot be queried. A notice banner is displayed at the top of the screen.
{% endhint %}

1. **Monitoring > Log Monitoring > Eventlog** Click the menu.
2. Select the query period. To specify a particular range, **direct input**after selecting, set the start date and end date.
3. Select the status or message filter to narrow down the event types to query.
4. To find a specific message, enter a keyword in the search box.
5. In the query results, **DB Service** or **Instance** click the name to move to its detail page.

The event log types displayed in the message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {변경된 상태}. ({상태 코드}) |
| Warning | CPU usage exceeded 50% | CPU usage has exceeded the caution level (50%). (Current: {사용량}%) |
|   | Memory usage exceeded 50% | Memory usage has exceeded the caution level (50%). (Current: {사용량}%) |
| Error | CPU usage exceeded 90% | CPU usage has exceeded the warning level (90%). (Current: {사용량}%) |
|   | Memory usage exceeded 90% | Memory usage has exceeded the warning level (90%). (Current: {사용량}%) |
