On the Eventlog page, you can view event logs generated according to the rules defined by OwlDB.

In the **Monitoring > Log Monitoring > Eventlog** menu, you can check logs generated for a DB Service, and filter out only the events you want by combining the query period and filters. Results are displayed in descending order of received date.

{% hint style="info" %}
**Note**

If the DB Service is in the `Terminating` state, you cannot query Eventlog. A notice banner is displayed at the top of the screen.
{% endhint %}

1. Click the **Monitoring > Log Monitoring > Eventlog** menu.
2. Select the query period. To specify a particular range, select **Direct input** and then set the start and end dates.
3. Select a status or message filter to narrow down the event types to view.
4. To find a specific message, enter a keyword in the search box.
5. In the search results, click the **DB Service** or **instance** name to go to its detail page.

The event log types shown in the Message column are as follows.

| Status | Trigger condition | Message |
| --- | --- | --- |
| Info | Instance status change | The instance status has changed to {changed status}. ({status code}) |
| Warning | CPU usage exceeded 50% | CPU usage has exceeded the caution level (50%). (Current: {usage}%) |
|   | Memory usage exceeded 50% | Memory usage has exceeded the caution level (50%). (Current: {usage}%) |
| Error | CPU usage exceeded 90% | CPU usage has exceeded the warning level (90%). (Current: {usage}%) |
|   | Memory usage exceeded 90% | Memory usage has exceeded the warning level (90%). (Current: {usage}%) |
