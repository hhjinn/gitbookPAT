On the backup settings page, configure the automatic backup scheduler and check its operational status.

## Backup settings <a href="#backup-settings" id="backup-settings"></a>

Set whether automatic backup is used, the execution cycle, retention period, and start time, and check backup stability through the scheduler's most recent execution result, 7-day success rate, consecutive failure count, and the 30-day execution history chart.

**Management > Backup Settings** Clicking the menu displays the currently configured automatic backup configuration information.

{% hint style="info" %}
**Note**

In the Cloud environment, the CSP Snapshot feature is used to manage backups as a single automatic backup without distinguishing between Full/Incremental.
{% endhint %}

1. **Management > Backup Settings**Click.
2. **Edit**Click.
3. Set the automatic backup toggle to **On**.
4. Enter the automatic backup cycle, retention period, and start time.
5. **Save**Click.

The following items are displayed on the page.

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Automatic backup</td><td>Whether automatic backup is enabled (On/Off)</td><td>Default: Off</td></tr><tr><td>Automatic backup interval</td><td>The interval at which automatic backup runs</td><td><ul><li>By hour: 1–23</li><li>By day: 1–7</li></ul></td></tr><tr><td>Retention period</td><td>The period for which the created backup image is retained</td><td><ul><li>By hour: 1–23</li><li>By day: 1–35</li></ul></td></tr><tr><td>Start time</td><td>The date and time when automatic backup first starts</td><td>A date and time earlier than the present cannot be selected</td></tr><tr><td>Latest backup date</td><td>The date and time when automatic backup was most recently completed</td><td>Display only</td></tr><tr><td>Next backup date</td><td>The date and time when the next automatic backup is scheduled</td><td>Display only</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

If you set automatic backup to On, storage capacity increases according to the storage interval and retention period, and additional charges apply.
{% endhint %}

---

## Backup scheduler operation status <a href="#backup-scheduler-status" id="backup-scheduler-status"></a>

In the **Backup scheduler operation status** section at the bottom of the backup settings page, check the recent execution history and stability metrics of automatic backup. The last execution information is displayed even when automatic backup is off.

| Item | Description | Displayed when not configured |
| --- | --- | --- |
| Latest execution result | Whether the most recently completed automatic backup succeeded or failed | `-` |
| Success rate over the last 7 days | The success rate of automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive failure count | The number of consecutive failures starting from the most recently completed run (0 if all succeeded) | `-` |
| Execution result chart for the last 30 days | Displays the number of successful/failed automatic backups per day over the last 30 days as a stacked bar chart | No Data status display |

In the chart, success and failure are distinguished by color. Hover over a bar to check the detailed information on the number of successful and failed automatic backups for that day.

1. **Management > Backup Settings**Click.
2. In the **Backup scheduler operation status** section at the bottom of the page, check the latest execution result, the success rate over the last 7 days, and the consecutive failure count.
3. **Execution result chart for the last 30 days**Hover over a bar in to check the detailed execution results by day.
