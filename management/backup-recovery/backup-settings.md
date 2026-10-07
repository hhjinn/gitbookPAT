**Backup Settings** On the page, configure the automatic backup scheduler and check its operational status. In an On-Premise environment, Full Backup and Incremental Backup are each configured independently.

{% hint style="warning" %}
**Caution**

OpenSQL must use OpenBackup to use the backup/recovery features.
{% endhint %}

## Backup Settings <a href="#backup-settings" id="backup-settings"></a>

**Management > Backup Settings**Clicking this shows the current automatic backup configuration information. In an On-Premise environment, Full Backup and Incremental Backup are displayed separately.

1. **Management > Backup Settings**Click.
2. **Edit**Click.
3. Enter the setting items. For OpenSQL, additionally enter the OpenBackup setting information.
4. **Save**Click.

### OpenBackup Settings (OpenSQL only) <a href="#openbackup-opensql" id="openbackup-opensql"></a>

In an OpenSQL On-Premise environment, OpenBackup setting information is additionally displayed above the automatic backup items. If OpenBackup is disabled, a notification banner appears at the top of the page and the automatic backup items are not displayed.

| Item | Description |
| --- | --- |
| OpenBackup | Whether OpenBackup is used (Enabled/Disabled) |
| Health | Backup server connection status (Connected/Not Connected) |
| Backup server | Information on the OpenBackup (Barman) server in use |
| OpenBackup Agent Port | Port number used to communicate with the OpenBackup server's Agent |
| WAL retention method | `archiver` / `streaming` / `archiver + streaming` |
| Backup method | `rsync` / `postgres` |
| Backup reuse method | The backup method is `rsync`Displayed only when it is (`link` / `copy`) |

### Edit Mode Input Items <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td>Full Backup</td><td>Whether Full Backup is used</td><td>Default: Off</td></tr><tr><td>Full Backup Interval</td><td>Full Backup execution interval</td><td><ul><li>By hour: 1–23</li><li>By day: 1–7</li></ul></td></tr><tr><td>Retention Period</td><td>Full Backup image retention period</td><td><ul><li>By hour: 1–23</li><li>By day: 1–35</li><li>Permanent retention</li></ul></td></tr><tr><td>Start Time</td><td>Full Backup Start Date/Time</td><td>A date/time earlier than the present cannot be selected</td></tr><tr><td>Incremental Backup</td><td>Whether Incremental Backup is used</td><td>Default: Off</td></tr><tr><td>Incremental Backup Interval</td><td>Incremental Backup execution interval</td><td><ul><li>By hour: 1–23</li><li>By day: 1–6</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Note the following when configuring Incremental Backup.

- Incremental Backup cannot be turned on while Full Backup is off. Please turn on Full Backup first.
- The Incremental Backup interval must be smaller than the Full Backup interval.
{% endhint %}

{% hint style="info" %}
**Note**

In OpenSQL On-Premise, if the PostgreSQL version is 16 or lower and the WAL retention method is `streaming`Incremental Backup cannot be used in this case.
{% endhint %}

## Backup Scheduler Operational Status <a href="#backup-scheduler-status" id="backup-scheduler-status"></a>

At the bottom of the Backup Settings page, in the **Backup Scheduler Operational Status** section, check the recent execution history and stability metrics of automatic backups. Even when automatic backup is off, the last execution information may be displayed.

| Item | Description | When not configured |
| --- | --- | --- |
| Most Recent Execution Result | The most recently completed execution result for each of Full Backup and Incremental Backup (Success/Failure) | `-` |
| Success Rate Over the Last 7 Days | The success rate of all automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive Failure Count | The number of consecutive failures starting from the most recently completed run (0 if all succeeded) | `-` |
| Execution Result Chart for the Last 30 Days | Displays the daily success/failure counts of automatic backups over the last 30 days as a stacked bar chart | No Data |

The success rate over the last 7 days and the consecutive failure count are calculated by summing all execution runs of both Full Backup and Incremental Backup. The most recent execution result is displayed as follows depending on the configuration status.

- **Only Full Backup configured**: Displays only the most recent Full Backup execution result.
- **Full + Incremental Backup Simultaneous Configuration**: Displays the execution results of the two types separately.
- **Not Configured**: `-`is displayed.

Hover the mouse over a bar in the chart to check the number of successes and failures for each of Full Backup and Incremental Backup on that date. Backups scheduled to run on the current day or not yet completed are not included in the chart.

1. **Management > Backup Settings**Click.
2. At the bottom of the page **Backup Scheduler Operational Status** In this section, check the latest execution result, the success rate over the past 7 days, and the number of consecutive failures.
3. Hover the mouse over a bar in the chart to check the detailed execution results by date.

## Backup Storage Usage <a href="#backup-storage-usage" id="backup-storage-usage"></a>

In the Backup Storage Usage section, check the current disk storage status in use for backups with a horizontal bar chart. The usage ratio relative to the total storage is displayed in the center of the chart, and each item is distinguished by color and legend.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Total Storage | Total capacity of backup storage |
| Backup | Full/Incremental Backup storage usage |
| Archive Log | Archive Log usage retained to guarantee the recovery point |
| Others | Capacity in use on the same path other than the Backup/Archive Log of the current DB service |
| Free | Unused free capacity |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Total Backup Storage | Total backup storage capacity available to the current DB service |
| Backup | Full/Incremental Backup storage usage |
| WAL | WAL usage retained to guarantee the recovery point |
| Others | Capacity in use on the same path other than the Backup/WAL of the current DB service |
| Free | Unused free capacity |

{% hint style="info" %}
**Note**

The Others item may include usage from other DB services using the same Barman server or arbitrary files.
{% endhint %}
{% endtab %}
{% endtabs %}
