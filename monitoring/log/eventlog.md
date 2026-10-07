On the Eventlog page, you can view event logs that occurred according to the rules defined by OwlDB.

In the **Monitoring > Log Monitoring > Eventlog** menu, you can check logs that occurred in the DB Service, and filter out only the events you want by combining the query period and filters. Results are displayed in order of most recent received date.

{% hint style="info" %}
**Note**

If the DB Service is in the `Terminating` state, the Eventlog cannot be viewed. An informational banner is displayed at the top of the screen.
{% endhint %}

1. Click the **Monitoring > Log Monitoring > Eventlog** menu.
2. Select the query period. To specify a particular range, select **Direct Input** and then set the start date and end date.
3. Select a status or message filter to narrow the type of events to view.
4. To find a specific message, enter a keyword in the search box.
5. In the query results, click the **DB Service** or **instance** name to go to its detail page.

The types of event logs displayed in the message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {changed status}. ({status code}) |
| Warning | CPU usage exceeds 50% | CPU usage has exceeded the caution level (50%). (Current: {usage}%) |
|   | Memory usage exceeds 50% | Memory usage has exceeded the caution level (50%). (Current: {usage}%) |
| Error | CPU usage exceeds 90% | CPU usage has exceeded the warning level (90%). (Current: {usage}%) |
|   | Memory usage exceeds 90% | Memory usage has exceeded the warning level (90%). (Current: {usage}%) |
