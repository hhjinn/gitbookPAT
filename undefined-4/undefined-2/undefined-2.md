**Backup Settings** Configure the automatic backup scheduler and check the operational status on the page. In an On-Premise environment, Full Backup and Incremental Backup are each configured independently.

{% hint style="warning" %}
**Caution**

OpenSQL must use OpenBackup to use the backup/recovery feature.
{% endhint %}

## Backup Settings

**Management > Backup Settings**Click to check the current automatic backup configuration information. In an On-Premise environment, Full Backup and Incremental Backup are displayed separately.

1. **Management > Backup Settings**Click it.
2. **Edit**Click it.
3. Enter the configuration items. For OpenSQL, additionally enter the OpenBackup configuration information.
4. **Save**Click it.

### OpenBackup Settings (OpenSQL only)

In the OpenSQL On-Premise environment, OpenBackup configuration information is displayed additionally above the automatic backup items. If OpenBackup is disabled, an informational banner appears at the top of the page and the automatic backup items are not displayed.

| Item | Description |
| --- | --- |
| OpenBackup | Whether OpenBackup is used (Used/Not Used) |
| Health | Backup server connection status (Connected/Not Connected) |
| Backup server | Information on the OpenBackup (Barman) server in use |
| OpenBackup Agent Port | Port number for communicating with the Agent of the OpenBackup server |
| WAL retention method | `archiver` / `streaming` / `archiver + streaming` |
| Backup method | `rsync` / `postgres` |
| Backup reuse method | The backup method is `rsync`Displayed only when it is (`link` / `copy`) |

### Edit mode input items

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Full Backup</td><td>Whether Full Backup is used</td><td>Default: Off</td></tr><tr><td>Full Backup interval</td><td>Full Backup execution interval</td><td><ul><li>Per hour: 1–23</li><li>Per day: 1–7</li></ul></td></tr><tr><td>Retention period</td><td>Full Backup image retention period</td><td><ul><li>Per hour: 1–23</li><li>Per day: 1–35</li><li>Permanent retention</li></ul></td></tr><tr><td>Start Time</td><td>Full Backup start date and time</td><td>Cannot select a date and time earlier than the present</td></tr><tr><td>Incremental Backup</td><td>Whether Incremental Backup is used</td><td>Default: Off</td></tr><tr><td>Incremental Backup interval</td><td>Incremental Backup execution interval</td><td><ul><li>Per hour: 1–23</li><li>Per day: 1–6</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Note the following when configuring Incremental Backup.

- Incremental Backup cannot be enabled while Full Backup is off. Please enable Full Backup first.
- The Incremental Backup interval must be smaller than the Full Backup interval.
{% endhint %}

{% hint style="info" %}
**Note**

In OpenSQL On-Premise, if the PostgreSQL version is 16 or lower and the WAL retention method is `streaming`Incremental Backup cannot be used in this case.
{% endhint %}

## Backup Scheduler Operational Status

At the bottom of the Backup Settings page, **Backup Scheduler Operational Status** In the section, check the recent execution history and stability metrics of automatic backups. The last execution information may be displayed even when automatic backup is off.

| Item | Description | When not configured |
| --- | --- | --- |
| Recent execution result | The most recent completed execution result of Full Backup and Incremental Backup respectively (Success/Failure) | `-` |
| Success rate over the last 7 days | The success ratio of all automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive failure count | The number of consecutive failures starting from the most recent completed instance (0 if all succeeded) | `-` |
| Execution result chart for the last 30 days | Displays the daily number of successful/failed automatic backups over the last 30 days as a stacked bar chart | No Data |

The success rate over the last 7 days and the consecutive failure count are calculated by summing all execution instances of Full Backup and Incremental Backup. The recent execution result is displayed as follows depending on the configuration status.

- **Full Backup only configured**: Displays only the recent execution result of Full Backup.
- **Full + Incremental Backup configured simultaneously**: Displays the execution results of both types separately.
- **Not configured**: `-`is displayed.

Hovering the mouse over a bar in the chart shows the number of successful/failed instances of Full Backup and Incremental Backup for that day. Backups scheduled to run that day or not yet completed are not included in the chart.

1. **Management > Backup Settings**Click it.
2. Bottom of the page **Backup Scheduler Operational Status** In the section, check the recent execution result, success rate over the last 7 days, and consecutive failure count.
3. Hover the mouse over a bar in the chart to check the detailed execution results by date.

## Backup Storage Usage

In the Backup Storage Usage section, check the current disk storage status used for backups as a horizontal bar chart. The usage ratio relative to the total storage is displayed in the center of the chart, and each item is distinguished by color and legend.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Total storage | Total backup storage capacity |
| Backup | Full/Incremental Backup storage usage |
| Archive Log | Archive Log usage retained to guarantee the recovery point |
| Others | Capacity in use on the same path besides the current DB service's Backup/Archive Log |
| Free | Unused free capacity |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Total backup storage | Total backup storage capacity available to the current DB service |
| Backup | Full/Incremental Backup storage usage |
| WAL | WAL usage retained to guarantee the recovery point |
| Others | Capacity in use on the same path besides the current DB service's Backup/WAL |
| Free | Unused free capacity |

{% hint style="info" %}
**Note**

The Others item may include usage from other DB services using the same Barman server or arbitrary files.
{% endhint %}
{% endtab %}
{% endtabs %}
