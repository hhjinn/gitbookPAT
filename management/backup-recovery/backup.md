In **Management > Backup/Restore > Backup**, you can view the database backup list and perform creation, modification, deletion, and restoration.

A backup saves the state of the database at a specific point in time as an image. Backups can be created automatically through the scheduler or manually by the user, and you can restore to a desired point in time through Full Restore or Point-in-Time Recovery (PITR). Additional charges apply based on the backup storage usage.

The retention period can be set from 1 hour up to a maximum of 35 days.

## Viewing the backup list <a href="#backup-list" id="backup-list"></a>

When you enter the **Management > Backup/Restore > Backup** menu, the backup list of the current DB Service appears. Each row represents a single backup image, and backups are displayed on a single-image basis without distinguishing between Full and Incremental.

The columns displayed in the list are as follows.

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup path</td><td>Backup path name</td></tr><tr><td>Creation Date</td><td>Backup image creation date and time (yyyy.mm.dd HH\:mm:ss)</td></tr><tr><td>Expiration date</td><td>The expiration date and time calculated from the retention period (yyyy.mm.dd HH\:mm:ss)<ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00\:00:00 on the day after the selected end date</li></ul></td></tr><tr><td>Creation method</td><td>Automatic / Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

{% hint style="info" %}
**Note**

- Click the 🔃 icon to manually refresh the list.
- If there is no data matching the search conditions, "There is no data available to check." is displayed.
{% endhint %}

### Backup status history <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation started</li><li>Restorable</li><li>Creation failed</li><li>Restoration started</li><li>Restoration failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence date</td><td>Displays the date and time when the status changed (yyyy.mm.dd HH\:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you switch OpenBackup usage to disabled in OpenSQL, the status is displayed as Deleted.
{% endhint %}

## Creating a backup <a href="#create-backup" id="create-backup"></a>

When you enter a name and retention period, a single backup image of the current point in time is created.

1. On the **Backup** page, click the **Create** button.
2. Enter a **name**.
3. Select the end date of the **retention period** in the calendar. The backup is retained until the selected date and expires at 00\:00:00 the following day.
4. Click the **Create** button.

{% hint style="info" %}
**Note**

This is only possible when the database operation status is `Running`.
{% endhint %}

## Restoration <a href="#restore" id="restore"></a>

When you select one backup from the backup list and click the **Restore** button, the restore modal appears. Select a restore type to restore the database to a specific point in time.

<table><thead><tr><th>Restore type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Restore all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Restore up to a specified date and time (in minutes)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Depending on the restore type and DB engine, existing backups may be deleted.

- **Full Restore (Tibero)**: Existing backups are not deleted.
- **Full Restore (OpenSQL)**: Backups created after the selected backup are deleted.
- **PITR**: All currently stored backups are deleted. For data that needs long-term retention, back it up in advance before restoring.
{% endhint %}

Restoration can take up to several hours, and you can check the progress in **Restoration history**.

1. Select one backup to restore.
2. Click the **Restore** button.
3. Select a **restore type**.
4. If you select **PITR**, select the date and time to restore to. The selectable range is between the restorable point of that backup and the current time.
5. Click the **Restore** button.

{% hint style="info" %}
**Note**

- Available only when the database operational status is `Running`, `Down`, or `Degraded`.
- If the recovery fails, please select an earlier point in time than the one you previously selected and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}

## Edit Backup <a href="#modify-backup" id="modify-backup"></a>

Edits the name and retention period of a backup.

1. Select one backup to edit.
2. Click the **Edit** button.
3. Change the **name** or **retention period**.
4. Click the **Save** button.

## Delete Backup <a href="#delete-backup" id="delete-backup"></a>

Permanently deletes the selected backup image.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be recovered. Be sure to verify the data you need before deleting.
{% endhint %}

1. Select one or more backups to delete.
2. Click the **Delete** button.
3. Confirm the deletion notice modal.
4. Click the **Delete** button.

## Archive Backup <a href="#backup-retention" id="backup-retention"></a>

Stores a specific backup in long-term archive storage. Archived backups are not automatically deleted even after the retention period expires, and can be recovered when needed.

{% hint style="info" %}
**Note**

- Backup archiving is supported only on Tibero in AWS environments; the archive list is not displayed in Azure environments.
- A separate charge is incurred based on the archive storage capacity, and charges are billed for the minimum retention period of 90 days regardless of whether the data is deleted.
{% endhint %}

1. Select one backup to archive.
2. Click the **Archive** button.
3. Enter a **name**.
4. Select the **retention period**.
5. Click the **Archive** button.

{% hint style="info" %}
**Note**

Available only when the database operational status is not `Terminating`.
{% endhint %}

### View Archive List <a href="#undefined-1" id="undefined-1"></a>

Check the list of backups in long-term archive in the **Archive** section within the Backup/Archive page.

| Column | Description |
| --- | --- |
| Name | The name entered at the time of archiving |
| ID | ID automatically generated by the CSP |
| Creation Date | Creation date and time of the original backup image (not the archive request date) |
| Expiration date | Expiration date and time calculated from the retention period |
| Size (MB) | Total size of the archived backup image |
| Status | Recoverable, Recovering, Deleted, Deleting |

Archived backups with an expiration date within 30 days display a ⚠️ icon in front of the name. Hover over the icon to see the scheduled deletion date and time. If long-term archiving is needed, click **Edit** before expiration to extend the retention period.

1. Go to **Management > Backup/Recovery > Backup**.
2. Scroll down the page to the **Archive** section.
3. Find the desired archived backup using a filter or name search.

### Edit Archive <a href="#undefined-2" id="undefined-2"></a>

Edits the name and retention period of an archived backup.

1. Select one archived backup to edit.
2. Click the **Edit** button.
3. Change the **name** or **retention period**.
4. Click the **Save** button.

### Recover Archive <a href="#undefined-3" id="undefined-3"></a>

1. Select one archived backup to recover.
2. Click the **Restore** button.
3. Enter the recovery date and time.
4. Click the **Restore** button.
5. Check the progress status in **Recovery History**.

{% hint style="warning" %}
**Caution**

When recovering an archive, database recovery starts immediately and all currently stored backups are deleted. Completion may take up to 3 days depending on the data size.
{% endhint %}

### Delete Archive <a href="#undefined-4" id="undefined-4"></a>

1. Select one or more archived backups to delete.
2. Click the **Delete** button.

{% hint style="warning" %}
**Caution**

Deleted archives cannot be recovered again, and all data is permanently deleted.
{% endhint %}
