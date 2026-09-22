The Eventlog page displays event logs generated according to the rules defined by OwlDB.

**Monitoring > Log Monitoring > Eventlog** In this menu, you can check the logs generated for a DB Service and filter out only the events you want by combining a query period and filters. Results are displayed in order of the most recent received date.

{% hint style="info" %}
**Note**

If a DB Service is in the `Terminating` state, Eventlog cannot be queried. A notification banner is displayed at the top of the screen.
{% endhint %}

1. **Monitoring > Log Monitoring > Eventlog** Click the menu.
2. Select a query period. To specify a particular range, **Enter manually**select this, then set the start date and end date.
3. Select a status or message filter to narrow down the type of events to query.
4. To find a specific message, enter a keyword in the search box.
5. In the query results, **DB Service** or **Instance** Click the name to navigate to the corresponding detail page.

The types of event logs displayed in the message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {변경된 상태}. ({상태 코드}) |
| Warning | CPU usage exceeded 50% | CPU usage exceeded the caution level (50%). (Current: {사용량}%) |
|   | Memory usage exceeded 50% | Memory usage exceeded the caution level (50%). (Current: {사용량}%) |
| Error | CPU usage exceeded 90% | CPU usage exceeded the warning level (90%). (Current: {사용량}%) |
|   | Memory usage exceeded 90% | Memory usage exceeded the warning level (90%). (Current: {사용량}%) |
