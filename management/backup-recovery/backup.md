**Management > Backup/Restore > Backup**In, query the database backup list and perform creation, modification, deletion, and restoration.

A backup saves the database state at a specific point in time as an image. Backups can be created automatically through the scheduler or manually by the user, and can be restored to a desired point in time through Full Restore or Point-in-Time Recovery (PITR). Additional charges apply based on backup storage capacity.

The retention period can be set from 1 hour up to a maximum of 35 days.

## Querying the backup list <a href="#backup-list" id="backup-list"></a>

**Management > Backup/Restore > Backup** When you enter the menu, the backup list of the current DB Service appears. Each row represents one backup image and is displayed on a single-image basis, without distinguishing between Full and Incremental.

The columns displayed in the list are as follows.

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup path</td><td>Backup path name</td></tr><tr><td>Creation date</td><td>Backup image creation date and time (yyyy.mm.dd HH\:mm:ss)</td></tr><tr><td>Expiration date</td><td>The expiration date and time calculated from the retention period (yyyy.mm.dd HH\:mm:ss)<ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00\:00:00 on the day after the selected end date</li></ul></td></tr><tr><td>Creation method</td><td>Automatic / Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

{% hint style="info" %}
**Note**

- Click the 🔃 icon to manually refresh the list.
- If there is no data matching the query conditions, "There is no data available." is displayed.
{% endhint %}

### Backup status history <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation started</li><li>Restorable</li><li>Creation failed</li><li>Restoration started</li><li>Restoration failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence date</td><td>Displays the date and time when the status changed (yyyy.mm.dd HH\:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you switch OpenBackup usage to disabled in OpenSQL, the status is displayed as Deleted.
{% endhint %}

## Creating a backup <a href="#create-backup" id="create-backup"></a>

When you enter a name and retention period, a single backup image of the current point in time is created.

1. **Backup** On the page, **Create** Click the button.
2. **Name**Enter.
3. In the calendar, **Retention period**Select the end date of. It is retained until the selected date and expires at 00\:00:00 the next day.
4. **Create** Click the button.

{% hint style="info" %}
**Note**

The database operation status is `Running`Only possible when.
{% endhint %}

## Restore <a href="#restore" id="restore"></a>

Select one backup from the backup list and **Restore** When you click the button, the restore modal appears. Select a restore type to restore the database to a specific point in time.

<table><thead><tr><th>Restore type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Restore all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Restore up to a specified date and time (by the minute)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Depending on the recovery type and DB engine, existing backups may be deleted.

- **Full Restore (Tibero)**: Existing backups are not deleted.
- **Full Restore (OpenSQL)**: Backups created after the selected backup are deleted.
- **PITR**: All currently stored backups are deleted. Archive any data requiring long-term retention before recovery.
{% endhint %}

Recovery can take up to several hours, and you can check the progress in **Recovery History**Check in.

1. Select one backup to recover.
2. **Restore** Click the button.
3. **Restore type**Select
4. **PITR**If you selected, select the date and time to recover to. The selectable range is between the recoverable point of that backup and the current time.
5. **Restore** Click the button.

{% hint style="info" %}
**Note**

- The database operation status is `Running`, `Down`, `Degraded`Only possible when.
- If recovery fails, please select a point earlier than the previously selected point and try again. If recovery does not succeed after multiple attempts, please request technical support.
{% endhint %}

## Edit Backup <a href="#modify-backup" id="modify-backup"></a>

Edit the backup's name and retention period.

1. Select one backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention period**Change
4. **Save** Click the button.

## Delete Backup <a href="#delete-backup" id="delete-backup"></a>

Permanently delete the selected backup image.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be recovered. Be sure to check for any needed data before deleting.
{% endhint %}

1. Select one or more backups to delete.
2. **Delete** Click the button.
3. Confirm the deletion notice modal.
4. **Delete** Click the button.

## Archive Backup <a href="#backup-retention" id="backup-retention"></a>

Save a specific backup to long-term archive storage. Archived backups are not automatically deleted even after the retention period passes, and can be recovered when needed.

{% hint style="info" %}
**Note**

- Backup archiving is supported only on Tibero in AWS environments; in Azure environments, the archive list is not displayed.
- Separate charges apply based on archive storage capacity, and you are billed for the minimum retention period of 90 days regardless of whether the data is deleted.
{% endhint %}

1. Select one backup to archive.
2. **Archive** Click the button.
3. **Name**Enter.
4. **Retention Period**Select
5. **Archive** Click the button.

{% hint style="info" %}
**Note**

The database operation status is `Terminating`Available only when it is not.
{% endhint %}

### View Archive List <a href="#undefined-1" id="undefined-1"></a>

Within the Backup/Archive page, **Archive** Check the list of backups in long-term archive in the section.

| Column | Description |
| --- | --- |
| Name | Name entered at the time of archiving |
| ID | ID automatically generated by the CSP |
| Creation date | Creation date and time of the original backup image (not the archive request date) |
| Expiration date | Expiration date and time calculated from the retention period |
| Size (MB) | Total size of the archived backup image |
| Status | Recoverable, Recovering, Deleted, Deleting |

Archived backups with an expiration date within 30 days display a ⚠️ icon before the name. Hover over the icon to check the scheduled deletion date and time. If long-term retention is needed, before expiration **Edit**Click to extend the retention period.

1. **Management > Backup/Restore > Backup**Navigate to.
2. Scroll down the page to move to the **Archive** section.
3. Find the desired archived backup using filters or name search.

### Edit Archive <a href="#undefined-2" id="undefined-2"></a>

Edit the name and retention period of the archived backup.

1. Select one archived backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention Period**Change
4. **Save** Click the button.

### Recover from Archive <a href="#undefined-3" id="undefined-3"></a>

1. Select one archived backup to recover.
2. **Restore** Click the button.
3. Enter the recovery date and time.
4. **Restore** Click the button.
5. **Recovery History**Check the progress status in

{% hint style="warning" %}
**Caution**

When recovering from an archive, database recovery starts immediately and all currently stored backups are deleted. Depending on the data size, it may take up to 3 days to complete.
{% endhint %}

### Delete Archive <a href="#undefined-4" id="undefined-4"></a>

1. Select one or more archived backups to delete.
2. **Delete** Click the button.

{% hint style="warning" %}
**Caution**

Deleted archives cannot be recovered, and all data is permanently deleted.
{% endhint %}
