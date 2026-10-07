On the **Backup Settings** page, configure the automatic backup scheduler and check its operational status. In an On-Premise environment, Full Backup and Incremental Backup are configured independently.

{% hint style="warning" %}
**Caution**

OpenSQL must use OpenBackup in order to use the backup/recovery features.
{% endhint %}

## Backup Settings <a href="#backup-settings" id="backup-settings"></a>

Clicking **Management > Backup Settings** displays the current automatic backup configuration information. In an On-Premise environment, Full Backup and Incremental Backup are displayed separately.

1. Click **Management > Backup Settings**.
2. Click **Edit**.
3. Enter the configuration items. For OpenSQL, additionally enter the OpenBackup configuration information.
4. Click **Save**.

### OpenBackup Settings (OpenSQL only) <a href="#openbackup-opensql" id="openbackup-opensql"></a>

In an OpenSQL On-Premise environment, OpenBackup configuration information is additionally displayed above the automatic backup items. If OpenBackup is disabled, an informational banner appears at the top of the page and the automatic backup items are not displayed.

| Item | Description |
| --- | --- |
| OpenBackup | Whether OpenBackup is used (Enabled/Disabled) |
| Health | Backup server connection status (Connected/Not Connected) |
| Backup server | Information about the OpenBackup (Barman) server in use |
| OpenBackup Agent Port | Port number used to communicate with the Agent of the OpenBackup server |
| WAL retention method | `archiver` / `streaming` / `archiver + streaming` |
| Backup method | `rsync` / `postgres` |
| Backup reuse method | Displayed only when the backup method is `rsync` (`link` / `copy`) |

### Edit mode input items <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Full Backup</td><td>Whether Full Backup is used</td><td>Default value: Off</td></tr><tr><td>Full Backup interval</td><td>Full Backup execution interval</td><td><ul><li>By hour: 1–23</li><li>By day: 1–7</li></ul></td></tr><tr><td>Retention period</td><td>Full Backup image retention period</td><td><ul><li>By hour: 1–23</li><li>By day: 1–35</li><li>Permanent retention</li></ul></td></tr><tr><td>Start Time</td><td>Full Backup start date and time</td><td>Cannot select a date and time earlier than the present</td></tr><tr><td>Incremental Backup</td><td>Whether Incremental Backup is used</td><td>Default value: Off</td></tr><tr><td>Incremental Backup interval</td><td>Incremental Backup execution interval</td><td><ul><li>By hour: 1–23</li><li>By day: 1–6</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

When configuring Incremental Backup, keep the following in mind.

- Incremental Backup cannot be turned on while Full Backup is off. Please turn on Full Backup first.
- The Incremental Backup interval must be smaller than the Full Backup interval.
{% endhint %}

{% hint style="info" %}
**Note**

In OpenSQL On-Premise, if the PostgreSQL version is 16 or lower and the WAL retention method is `streaming`, Incremental Backup cannot be used.
{% endhint %}

## Backup Scheduler Operational Status <a href="#backup-scheduler-status" id="backup-scheduler-status"></a>

In the **Backup Scheduler Operational Status** section at the bottom of the Backup Settings page, check the recent execution history and stability metrics of the automatic backup. The last execution information may be displayed even when automatic backup is off.

| Item | Description | When not configured |
| --- | --- | --- |
| Most recent execution result | The most recent completed execution result of each of Full Backup and Incremental Backup (Success/Failure) | `-` |
| Success rate over the last 7 days | The success ratio of all automatic backups completed within the last 7 days (e.g., 90% (9/10)) | `-` |
| Consecutive failure count | The number of consecutive failures starting from the most recent completed run (0 if all succeeded) | `-` |
| Execution result chart for the last 30 days | Displays the number of successful/failed automatic backups per day over the last 30 days as a stacked bar chart | No Data |

The success rate over the last 7 days and the consecutive failure count are calculated by summing all execution runs of Full Backup and Incremental Backup. The most recent execution result is displayed as follows depending on the configuration status.

- **Full Backup only configured**: Only the most recent Full Backup execution result is displayed.
- **Full + Incremental Backup configured together**: The execution results of both types are displayed separately.
- **Not configured**: `-` is displayed.

Hovering the mouse over a bar in the chart shows the number of successful and failed runs of Full Backup and Incremental Backup for that day. Backups scheduled to run that day but not yet completed are not included in the chart.

1. Click **Management > Backup Settings**.
2. In the **Backup Scheduler Operational Status** section at the bottom of the page, check the most recent execution result, the success rate over the last 7 days, and the consecutive failure count.
3. Hover the mouse over a bar in the chart to check the detailed execution result for each day.

## Backup Storage Usage <a href="#backup-storage-usage" id="backup-storage-usage"></a>

In the Backup Storage Usage section, check the current disk storage status used for backups as a horizontal bar chart. The usage ratio relative to the total storage is displayed in the center of the chart, and each item is distinguished by color and legend.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Total storage | Total capacity of the backup storage |
| Backup | Full/Incremental Backup storage usage |
| Archive Log | Archive Log usage retained to guarantee the recovery point |
| Others | Capacity in use at the same path other than the Backup/Archive Log of the current DB service |
| Free | Unused free capacity |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Total backup storage | Total backup storage capacity available to the current DB service |
| Backup | Full/Incremental Backup storage usage |
| WAL | WAL usage retained to guarantee the recovery point |
| Others | Capacity in use at the same path other than the Backup/WAL of the current DB service |
| Free | Unused free capacity |

{% hint style="info" %}
**Note**

The Others item may include usage from other DB services using the same Barman server, arbitrary files, and so on.
{% endhint %}
{% endtab %}
{% endtabs %}
