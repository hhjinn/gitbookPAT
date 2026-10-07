On the backup settings page, you configure the automatic backup scheduler and check its operational status.

## Backup settings <a href="#backup-settings" id="backup-settings"></a>

Set whether to use automatic backup, the execution cycle, the retention period, and the start time, and check backup stability through the scheduler's latest execution result, 7-day success rate, consecutive failure count, and 30-day execution history chart.

Clicking the **Management > Backup Settings** menu lets you check the currently configured automatic backup configuration information.

{% hint style="info" %}
**Note**

In a Cloud environment, the CSP Snapshot feature is used to manage backups as a single automatic backup without distinguishing between Full/Incremental.
{% endhint %}

1. Click **Management > Backup Settings**.
2. Click **Edit**.
3. Set the automatic backup toggle to **On**.
4. Enter the automatic backup cycle, retention period, and start time.
5. Click **Save**.

The following items are displayed on the page.

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Automatic backup</td><td>Whether automatic backup is enabled (On/Off)</td><td>Default value: Off</td></tr><tr><td>Automatic backup cycle</td><td>The cycle at which automatic backup runs</td><td><ul><li>Hourly: 1–23</li><li>Daily: 1–7</li></ul></td></tr><tr><td>Retention period</td><td>The period for which the created backup image is retained</td><td><ul><li>Hourly: 1–23</li><li>Daily: 1–35</li></ul></td></tr><tr><td>Start time</td><td>The date and time when automatic backup first starts</td><td>Dates and times earlier than the present cannot be selected</td></tr><tr><td>Latest backup date</td><td>The date and time when the most recent automatic backup was completed</td><td>Display only</td></tr><tr><td>Next backup date</td><td>The scheduled date and time for the next automatic backup</td><td>Display only</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

If you set automatic backup to On, storage usage increases according to the storage interval and retention period, and additional charges apply.
{% endhint %}

---

## Backup scheduler operation status <a href="#backup-scheduler-status" id="backup-scheduler-status"></a>

In the **Backup scheduler operation status** section at the bottom of the backup settings page, you can check the recent execution history and stability metrics of automatic backups. The last execution information is displayed even when automatic backup is off.

| Item | Description | Displayed when not configured |
| --- | --- | --- |
| Latest execution result | Whether the most recently completed automatic backup succeeded or failed | `-` |
| Success rate over the last 7 days | The success rate of automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive failure count | The number of consecutive failures starting from the most recently completed run (0 if all succeeded) | `-` |
| Execution results chart for the last 30 days | Displays the number of successful/failed automatic backups per day over the last 30 days as a stacked bar chart | No Data status display |

In the chart, success and failure are distinguished by color. Hover over a bar to view detailed information on the number of successful and failed automatic backups for that date.

1. Click **Management > Backup Settings**.
2. In the **Backup scheduler operation status** section at the bottom of the page, check the latest execution result, success rate over the last 7 days, and consecutive failure count.
3. In the **Execution results chart for the last 30 days**, hover over a bar to check the detailed execution results by date.
