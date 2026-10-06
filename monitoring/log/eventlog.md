On the Eventlog page, you can view event logs that occurred according to the rules defined by OwlDB.

**Monitoring > Log Monitoring > Eventlog** In this menu, you can check the logs that occurred in the DB Service, and combine the query period and filters to narrow down to only the events you want. Results are displayed in order of most recent received date.

{% hint style="info" %}
**Note**

If the DB Service is `Terminating` in this state, the Eventlog cannot be queried. An information banner is displayed at the top of the screen.
{% endhint %}

1. **Monitoring > Log Monitoring > Eventlog** Click the menu.
2. Select the query period. To specify a particular range, **Direct Input**select this, then set the start date and end date.
3. Select the status or message filter to narrow down the event types to query.
4. To find a specific message, enter a keyword in the search box.
5. In the query results, **DB Service** or **Instance** click the name to move to the corresponding detail page.

The event log types displayed in the message column are as follows.

| Status | Occurrence Condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {변경된 상태}. ({상태 코드}) |
| Warning | CPU usage exceeds 50% | CPU usage has exceeded the caution level (50%). (Current: {사용량}%) |
|   | Memory usage exceeds 50% | Memory usage has exceeded the caution level (50%). (Current: {사용량}%) |
| Error | CPU usage exceeds 90% | CPU usage has exceeded the warning level (90%). (Current: {사용량}%) |
|   | Memory usage exceeds 90% | Memory usage has exceeded the warning level (90%). (Current: {사용량}%) |
