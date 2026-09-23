# Backup

**Management > Backup/Recovery > Backup** allows you to view the database backup list and perform creation, modification, deletion, and recovery.

A backup saves the database state at a specific point in time as an image. Backups can be created automatically through the scheduler or manually by the user, and can be recovered to a desired point in time through Full Restore or Point-in-Time Recovery (PITR). Separate charges apply based on backup storage capacity.

The retention period can be set from a minimum of 1 hour up to a maximum of 35 days.

### Viewing the Backup List

When you open **Management > Backup/Recovery > Backup**, the backup list of the current DB Service appears. Each row represents a single backup image, displayed on a single-image basis without distinguishing between Full and Incremental.

The columns displayed in the list are as follows.

| Column          | Description                                                                               |
| --------------- | ----------------------------------------------------------------------------------------- |
| Name            | Backup image name                                                                         |
| Backup Path     | Backup path name                                                                          |
| Created Date    | Backup image creation date and time (yyyy.mm.dd HH:flag\_mm:ss)                           |
| Expiration Date | Expiration date and time calculated from the retention period (yyyy.mm.dd HH:flag\_mm:ss) |
| Creation Method | Automatic / Manual                                                                        |
| Status          | Current backup status                                                                     |

{% hint style="info" %}
**Note**

* Click the 🔃 icon to manually refresh the list.
* If there is no data matching the search conditions, "No data available." is displayed.
{% endhint %}

#### Backup Status History

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation Started</li><li>Recoverable</li><li>Creation Failed</li><li>Recovery Started</li><li>Recovery Failed</li><li>Deletion Started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time when the status changed (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When OpenBackup usage is switched to disabled in OpenSQL, the status is displayed as Deleted.
{% endhint %}

### Creating a Backup

When you enter a name and retention period, a single backup image at the current point in time is created.

1. On the **Backup** page, click **Create**.
2. Enter a **Name**.
3. From the calendar, select the **Retention Period** end date.
4. Click **Create**.

{% hint style="info" %}
**Note**

Creating a backup is only possible when the database operational status is `Running`.
{% endhint %}

### Recovery

Select a backup from the backup list and click **Recover**. The recovery modal appears. Select the recovery type to recover the database to a specific point in time.

<table data-full-width="true"><thead><tr><th>Recovery Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Recovers all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovers up to the specified date and time (in minutes)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Existing backups may be deleted depending on the recovery type and DB engine.

* **Full Restore (Tibero)**: Existing backups are not deleted.
* **Full Restore (OpenSQL)**: Backups created after the selected backup are deleted.
* **PITR**: All currently stored backups are deleted. Please back up any data requiring long-term retention before recovery.
{% endhint %}

Recovery may take up to several hours. You can check its progress in **Recovery History**.

1. Select one backup to restore.
2. Click **Restore**.
3. Select the recovery type.
4. If you selected **PITR**, select the date and time to restore. The selectable range is between the recoverable point of the backup and the current time.
5. Click **Restore**.

{% hint style="info" %}
**Note**

* Restoration is only possible when the database operational status is `Running`, `Down`, or `Degraded`.
* If the restore fails, please select an earlier point in time than the one previously selected and try again. If the restore does not succeed after several attempts, please request technical support.
{% endhint %}

### Edit Backup

Edit the backup's name and retention period.

1. Select one backup to edit.
2. Click **Edit**.
3. Change the **Name** or **Retention Period**.
4. Click **Save**.

### Delete Backup

Permanently delete the selected backup image.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be restored. Be sure to verify any necessary data before deleting.
{% endhint %}

1. Select one or more backups to delete.
2. Click **Delete**.
3. Review the deletion notice modal.
4. Click **Delete**.

### Archive Backup

Save a specific backup to long-term archive storage. Archived backups are not automatically deleted even after the retention period passes, and can be restored when needed.

{% hint style="info" %}
**Note**

* Backup archiving is only supported on Tibero in AWS environments, and the archive list is not displayed in Azure environments.
* A separate charge is incurred based on the archive storage capacity, and you will be billed for the minimum retention period of 90 days regardless of whether the data is deleted.
{% endhint %}

1. Select one backup to archive.
2. Click **Archive**.
3. Enter a **Name**.
4. Select a **Retention Period**.
5. Click **Archive**.

{% hint style="info" %}
**Note**

Archiving is only possible when the database operational status is not `Terminating`.
{% endhint %}

#### View Archive List

In the **Archive** section of the Backup/Archive page, check the list of long-term archived backups.

| Column          | Description                                                                            |
| --------------- | -------------------------------------------------------------------------------------- |
| Name            | The name entered when archiving                                                        |
| ID              | The ID automatically generated by the CSP                                              |
| Creation Date   | The creation date and time of the original backup image (not the archive request date) |
| Expiration Date | The expiration date and time calculated from the retention period                      |
| Size (MB)       | The total size of the archived backup image                                            |
| Status          | Recoverable, Recovering, Deleted, Deleting                                             |

For archived backups whose expiration date is within 30 days, a ⚠️ icon is displayed before the name. Hovering the mouse over the icon shows the scheduled deletion date and time. If long-term archiving is needed, click **Edit** before expiration to extend the retention period.

1. Navigate to **Management > Backup/Restore > Backup**.
2. Scroll down to the **Archive** section.
3. Find the archived backup you want using filters or name search.

#### Modify Archive

Modify the name and retention period of an archived backup.

1. Select one archived backup to modify.
2. Click **Modify**.
3. Change the **Name** or **Retention Period**.
4. Click **Save**.

#### Restore Archive

1. Select one archived backup to restore.
2. Click **Restore**.
3. Enter the restore date and time.
4. Click **Restore**.
5. Check the progress in **Restore History**.

{% hint style="warning" %}
**Caution**

When restoring an archive, database recovery starts immediately and all currently stored backups are deleted. Depending on the data size, completion can take up to 3 days.
{% endhint %}

#### Delete Archive

1. Select one or more archived backups to delete.
2. Click **Delete**.

{% hint style="warning" %}
**Caution**

Deleted archives cannot be restored again, and all data is permanently deleted.
{% endhint %}
