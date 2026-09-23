**Management > Backup/Recovery > Backup**allows you to view the database backup list and perform creation, modification, deletion, and recovery.

A backup saves the database state at a specific point in time as an image. Backups can be created automatically through the scheduler or manually by the user, and can be recovered to a desired point in time through Full Restore or Point-in-Time Recovery (PITR). Separate charges apply based on backup storage capacity.

The retention period can be set from a minimum of 1 hour up to a maximum of 35 days.

## Viewing the Backup List

**Management > Backup/Recovery > Backup** When you enter the menu, the backup list of the current DB Service appears. Each row represents a single backup image, displayed on a single-image basis without distinguishing between Full and Incremental.

The columns displayed in the list are as follows.

| Column | Description |
| --- | --- |
| Name | Backup image name |
| Backup Path | Backup path name |
| Created Date | Backup image creation date and time (yyyy.mm.dd HH:1f1f2-1f1f2:ss) |
| Expiration Date | Expiration date and time calculated from the retention period (yyyy.mm.dd HH:1f1f2-1f1f2:ss) |
| Creation Method | Automatic / Manual |
| Status | Current backup status |

{% hint style="info" %}
**Note**

- Click the 🔃 icon to manually refresh the list.
- If there is no data matching the search conditions, "No data available." is displayed.
{% endhint %}

### Backup Status History

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation Started</li><li>Recoverable</li><li>Creation Failed</li><li>Recovery Started</li><li>Recovery Failed</li><li>Deletion Started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time when the status changed (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When OpenBackup usage is switched to disabled in OpenSQL, the status is displayed as Deleted.
{% endhint %}

## Creating a Backup

When you enter a name and retention period, a single backup image at the current point in time is created.

1. **Backup** On the page, **Create** Click the button.
2. **Name**Enter it.
3. From the calendar, **Retention Period**Select the end date of.
4. **Create** Click the button.

{% hint style="info" %}
**Note**

The database operation status is `Running`Only possible when it is.
{% endhint %}

## Recovery

Select one backup from the backup list and **Recover** When you click the button, the recovery modal appears. Select the recovery type to recover the database to a specific point in time.

<table data-full-width="true"><thead><tr><th>Recovery Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Recovers all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovers up to the specified date and time (in minutes)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Existing backups may be deleted depending on the recovery type and DB engine.

- **Full Restore (Tibero)**: Existing backups are not deleted.
- **Full Restore (OpenSQL)**: Backups created after the selected backup are deleted.
- **PITR**: All currently stored backups are deleted. Please back up any data requiring long-term retention before recovery.
{% endhint %}

Recovery may take up to several hours, and the progress can be checked in **Recovery History**Check it in.

1. Select one backup to restore.
2. **Restore** Click the button.
3. **Select the recovery type.**Select it.
4. **PITR**If you selected it, select the date and time to restore. The selectable range is between the recoverable point of the backup and the current time.
5. **Restore** Click the button.

{% hint style="info" %}
**Note**

- The database operational status is `Running`, `Down`, `Degraded`This is only possible when it is.
- If the restore fails, please select an earlier point in time than the one previously selected and try again. If the restore does not succeed after several attempts, please request technical support.
{% endhint %}

## Edit Backup

Edit the backup's name and retention period.

1. Select one backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention Period**Change it.
4. **Save** Click the button.

## Delete Backup

Permanently delete the selected backup image.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be restored. Be sure to verify any necessary data before deleting.
{% endhint %}

1. Select one or more backups to delete.
2. **Delete** Click the button.
3. Review the deletion notice modal.
4. **Delete** Click the button.

## Archive Backup

Save a specific backup to long-term archive storage. Archived backups are not automatically deleted even after the retention period passes, and can be restored when needed.

{% hint style="info" %}
**Note**

- Backup archiving is only supported on Tibero in AWS environments, and the archive list is not displayed in Azure environments.
- A separate charge is incurred based on the archive storage capacity, and you will be billed for the minimum retention period of 90 days regardless of whether the data is deleted.
{% endhint %}

1. Select one backup to archive.
2. **Archive** Click the button.
3. **Name**Enter it.
4. **Retention Period**Select it.
5. **Archive** Click the button.

{% hint style="info" %}
**Note**

The database operational status is `Terminating`This is only possible when it is not.
{% endhint %}

### View Archive List

Within the Backup/Archive page **Archive** In the section, check the list of backups in long-term archive.

| Column | Description |
| --- | --- |
| Name | The name entered when archiving |
| ID | The ID automatically generated by the CSP |
| Creation Date | The creation date and time of the original backup image (not the archive request date) |
| Expiration Date | The expiration date and time calculated from the retention period |
| Size (MB) | The total size of the archived backup image |
| Status | Recoverable, Recovering, Deleted, Deleting |

For archived backups whose expiration date is within 30 days, a ⚠️ icon is displayed before the name. Hovering the mouse over the icon shows the scheduled deletion date and time. If long-term archiving is needed, before expiration **Edit**Click it to extend the retention period.

1. **Management > Backup/Restore > Backup**Navigate to it.
2. Scroll down the page to **Archive** Move to the section.
3. Find the archived backup you want using filters or name search.

### Modify Archive

Modify the name and retention period of an archived backup.

1. Select one archived backup to modify.
2. **Modify** Click the button.
3. **Name** or **Retention period**Change it.
4. **Save** Click the button.

### Restore Archive

1. Select one archived backup to restore.
2. **Restore** Click the button.
3. Enter the restore date and time.
4. **Restore** Click the button.
5. **Restore History**Check the progress status here.

{% hint style="warning" %}
**Caution**

When restoring an archive, database recovery starts immediately and all currently stored backups are deleted. Depending on the data size, completion can take up to 3 days.
{% endhint %}

### Delete Archive

1. Select one or more archived backups to delete.
2. **Delete** Click the button.

{% hint style="warning" %}
**Caution**

Deleted archives cannot be restored again, and all data is permanently deleted.
{% endhint %}
