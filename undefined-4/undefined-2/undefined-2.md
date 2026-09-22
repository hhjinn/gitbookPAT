**Backup Settings** Configure the automatic backup scheduler and check its operational status on this page. In an On-Premise environment, Full Backup and Incremental Backup are configured independently of each other.

{% hint style="warning" %}
**Caution**

OpenSQL must use OpenBackup in order to use the backup/recovery features.
{% endhint %}

## Backup Settings

**Management > Backup Settings**Click to view the current automatic backup configuration information. In an On-Premise environment, Full Backup and Incremental Backup are displayed separately.

1. **Management > Backup Settings**Click it.
2. **Edit**Click it.
3. Enter the configuration items. For OpenSQL, also enter the OpenBackup configuration information.
4. **Save**Click it.

### OpenBackup Settings (OpenSQL only)

In an OpenSQL On-Premise environment, OpenBackup configuration information is additionally displayed above the automatic backup items. If OpenBackup is disabled, a notice banner appears at the top of the page and the automatic backup items are not displayed.

| Item | Description |
| --- | --- |
| OpenBackup | Whether OpenBackup is used (Enabled/Disabled) |
| Health | Backup server connection status (Connected/Disconnected) |
| Backup Server | Information about the OpenBackup(Barman) server in use |
| OpenBackup Agent Port | The port number used to communicate with the Agent of the OpenBackup server |
| WAL Retention Method | `archiver` / `streaming` / `archiver + streaming` |
| Backup Method | `rsync` / `postgres` |
| Backup Reuse Method | Only displayed when the backup method is `rsync`(`link` / `copy`) |

### Edit Mode Input Items

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td>Full Backup</td><td>Whether Full Backup is used</td><td>Default value: Off</td></tr><tr><td>Full Backup Interval</td><td>Full Backup execution interval</td><td><ul><li>Hourly: 1~23</li><li>Daily: 1~7</li></ul></td></tr><tr><td>Retention Period</td><td>Full Backup image retention period</td><td><ul><li>Hourly: 1~23</li><li>Daily: 1~35</li><li>Permanent retention</li></ul></td></tr><tr><td>Start Time</td><td>Full Backup start date and time</td><td>Cannot select a date and time earlier than the current time</td></tr><tr><td>Incremental Backup</td><td>Whether Incremental Backup is used</td><td>Default value: Off</td></tr><tr><td>Incremental Backup Interval</td><td>Incremental Backup execution interval</td><td><ul><li>Hourly: 1~23</li><li>Daily: 1~6</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Note the following when configuring Incremental Backup.

- Incremental Backup cannot be enabled while Full Backup is Off. Please enable Full Backup first.
- The Incremental Backup interval must be smaller than the Full Backup interval.
{% endhint %}

{% hint style="info" %}
**Note**

On OpenSQL On-Premise, if the PostgreSQL version is 16 or lower and the WAL retention method is `streaming`Incremental Backup cannot be used.
{% endhint %}

## Backup Scheduler Operational Status

At the bottom of the Backup Settings page, **Backup Scheduler Operational Status** In this section, check the recent execution history and reliability metrics of automatic backups. The last execution information may be displayed even when automatic backup is Off.

| Item | Description | When not configured |
| --- | --- | --- |
| Recent Execution Result | The most recent completed execution result for each of Full Backup and Incremental Backup (Success/Failure) | `-` |
| Last 7-Day Success Rate | The success rate of all automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive Failure Count | The number of consecutive failures starting from the most recent completed run (0 if all succeeded) | `-` |
| Last 30-Day Execution Result Chart | Displays the daily automatic backup success/failure counts over the last 30 days as a stacked bar chart | No Data |

The 7-day success rate and consecutive failure count are calculated by summing all Full Backup and Incremental Backup executions. The most recent execution result is displayed as follows depending on the configuration state.

- **Full Backup only configured**: Displays only the most recent Full Backup execution result.
- **Full + Incremental Backup configured together**: Displays the execution results of both types separately.
- **Not configured**: `-`is displayed.

Hovering the mouse over a bar in the chart shows the respective success and failure counts of Full Backup and Incremental Backup for that day. Backups scheduled to run that day or not yet completed are not included in the chart.

1. **Management > Backup Settings**Click.
2. At the bottom of the page **Backup Scheduler Operation Status** In the section, check the most recent execution result, the 7-day success rate, and the consecutive failure count.
3. Hover the mouse over a bar in the chart to check the detailed execution result for each day.

## Backup Storage Usage

In the Backup Storage Usage section, check the current disk storage status used for backups as a horizontal bar chart. The center of the chart displays the usage ratio relative to total storage, and each item is distinguished by color and legend.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Total Storage | Total capacity of the backup storage |
| Backup | Full/Incremental Backup storage usage |
| Archive Log | Archive Log usage retained to guarantee the recovery point |
| Others | Capacity used in the same path other than the current DB service's Backup/Archive Log |
| Free | Unused free capacity |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Total backup storage | Total capacity of the backup storage available to the current DB service |
| Backup | Full/Incremental Backup storage usage |
| WAL | WAL usage retained to guarantee the recovery point |
| Others | Capacity used in the same path other than the current DB service's Backup/WAL |
| Free | Unused free capacity |

{% hint style="info" %}
**Note**

The Others item may include usage from other DB services using the same Barman server, or arbitrary files.
{% endhint %}
{% endtab %}
{% endtabs %}
