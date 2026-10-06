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
| Info | Instance status change | 인스턴스 상태가 {변경된 상태}로 변경되었습니다. ({상태 코드}) |
| Warning | CPU usage exceeds 50% | CPU 사용량 주의 수준(50%)를 초과했습니다. (현재 : {사용량}%) |
|   | Memory usage exceeds 50% | Memory 사용량 주의 수준(50%)를 초과했습니다. (현재 : {사용량}%) |
| Error | CPU usage exceeds 90% | CPU 사용량 경고 수준(90%)를 초과했습니다. (현재 : {사용량}%) |
|   | Memory usage exceeds 90% | Memory 사용량 경고 수준(90%)를 초과했습니다. (현재 : {사용량}%) |
