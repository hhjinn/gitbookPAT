# Backup Settings

The Backup Settings page is where you configure the automatic backup scheduler and check its operational status.

### Backup Settings

You set whether to enable automatic backup, the execution interval, the retention period, and the start time, and you check backup stability through the scheduler's most recent execution result, 7-day success rate, consecutive failure count, and the 30-day execution history chart.

Click **Management > Backup Settings** to check the currently configured automatic backup settings.

{% hint style="info" %}
**Note**

In the Cloud environment, the CSP Snapshot feature is used to manage backups as a single automatic backup without distinguishing between Full and Incremental.
{% endhint %}

1. Click **Management > Backup Settings**.
2. Click **Edit**.
3. Set the automatic backup toggle to **On**.
4. Enter the automatic backup interval, retention period, and start time.
5. Click **Save**.

The page displays the following items.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td>Automatic Backup</td><td>Whether automatic backup is enabled (On/Off)</td><td>Default: Off</td></tr><tr><td>Automatic Backup Interval</td><td>The interval at which automatic backup runs</td><td><ul><li>Hourly: 1–23</li><li>Daily: 1–7</li></ul></td></tr><tr><td>Retention Period</td><td>The period for which created backup images are retained</td><td><ul><li>Hourly: 1–23</li><li>Daily: 1–35</li></ul></td></tr><tr><td>Start Time</td><td>The date and time when automatic backup first starts</td><td>Dates and times earlier than the current time cannot be selected</td></tr><tr><td>Last Backup Date</td><td>The date and time when automatic backup was most recently completed</td><td>Display only</td></tr><tr><td>Next Backup Date</td><td>The scheduled date and time of the next automatic backup</td><td>Display only</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

If you set automatic backup to On, storage capacity increases depending on the backup interval and retention period, and additional charges apply.
{% endhint %}

***

### Backup Scheduler Operational Status

At the bottom of the Backup Settings page, the **Backup Scheduler Operational Status** section lets you check the recent execution history and stability metrics of automatic backup. The last execution information is displayed even when automatic backup is off.

| Item                          | Description                                                                                                     | Displayed when not configured |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| Most Recent Execution Result  | Whether the most recently completed automatic backup succeeded or failed                                        | `-`                           |
| 7-Day Success Rate            | The success rate of automatic backups completed within the last 7 days (e.g., 90% (9/10))                       | `-`                           |
| Consecutive Failure Count     | The number of consecutive failures starting from the most recently completed run (0 if all succeeded)           | `-`                           |
| 30-Day Execution Result Chart | Displays the number of successful/failed automatic backups per day over the last 30 days as a stacked bar chart | No Data status display        |

In the chart, success and failure are distinguished by color. Hovering the mouse over a bar shows detailed information on the number of successful and failed automatic backups for that date.

1. Click **Management > Backup Settings**.
2. At the bottom of the page, check the most recent execution result, 7-day success rate, and consecutive failure count in **Backup Scheduler Operational Status**.
3. Hover over a bar in **30-Day Execution Result Chart** to check detailed execution results for each date.
