# Backup

In **Management > Backup/Restore > Backup**, you can view the database backup list and perform create, edit, delete, and restore operations.

A backup saves the database state at a specific point in time as an image. Backups can be created automatically through a scheduler or manually by the user, and you can restore to a desired point in time through a Full Restore or Point-in-Time Recovery (PITR). Separate charges apply based on backup storage capacity.

The retention period can be set from 1 hour up to a maximum of 35 days.

### Viewing the Backup List

When you enter the **Management > Backup/Restore > Backup** menu, the backup list of the current DB Service appears. Each row represents a single backup image, displayed on a single-image basis without distinguishing between Full and Incremental.

The columns displayed in the list are as follows.

<table data-full-width="true"><thead><tr><th width="212">Column</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup Path</td><td>Backup path name</td></tr><tr><td>Created</td><td>Backup image creation date and time (yyyy.mm.dd HH:mm:ss)</td></tr><tr><td>Expiration</td><td><p>Expiration date and time calculated from the retention period (yyyy.mm.dd HH:mm:ss)</p><ul><li>Automatic backup: Backup execution time + retention period</li><li>Manual backup: 00:00:00 on the day after the selected end date</li></ul></td></tr><tr><td>Creation Method</td><td>Automatic / Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

{% hint style="info" %}
**Note**

* Click the 🔃 icon to manually refresh the list.
* If there is no data matching the query conditions, "No data available." is displayed.
{% endhint %}

#### Backup Status History

<table data-full-width="true"><thead><tr><th width="198">Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation Started</li><li>Restorable</li><li>Creation Failed</li><li>Restore Started</li><li>Restore Failed</li><li>Deletion Started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time when the status changed (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you switch OpenBackup usage to disabled in OpenSQL, the status is displayed as Deleted.
{% endhint %}

### Creating a Backup

When you enter a name and retention period, a single backup image of the current point in time is created.

1. Click the **Create** button on the **Backup** page.
2. Enter a **Name**.
3. Select the end date of the **retention period** from the calendar. The backup is retained until the selected date and expires at 00:00:00 the next day.
4. Click the **Create** button.

{% hint style="info" %}
**Note**

This is only possible when the database operational status is `Running`.
{% endhint %}

### Restore

Select one backup from the backup list and click the **Restore** button to display the restore modal. Select a restore type to restore the database to a specific point in time.

<table data-full-width="true"><thead><tr><th width="293">Restore Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Restore all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Restore up to a specified date and time (in minutes)</li><li>Data after the specified point is lost</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Existing backups may be deleted depending on the restore type and DB engine.

* **Full Restore (Tibero)**: Existing backups are not deleted.
* **Full Restore (OpenSQL)**: Backups created after the selected backup are deleted.
* **PITR**: All currently stored backups are deleted. Preserve data that requires long-term storage in advance before restoring.
{% endhint %}

Restoration may take up to several hours, and you can check the progress in **Restore History**.

1. Select one backup to restore.
2. Click the **Restore** button.
3. Select the **restore type**.
4. If you selected **PITR**, select the date and time to restore. The selectable range is between the restorable point of that backup and the current time.
5. Click the **Restore** button.

{% hint style="info" %}
**Note**

* This is only possible when the database operational status is `Running`, `Down`, or `Degraded`.
* If the restore fails, please select an earlier point in time than the one previously selected and try again. If the restore does not succeed after several attempts, please request technical support.
{% endhint %}

### Editing a Backup

Edit the name and retention period of a backup.

1. Select one backup to edit.
2. Click the **Edit** button.
3. Change the **Name** or **Retention Period**.
4. Click the **Save** button.

### Deleting a Backup

Permanently delete the selected backup image.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be restored. Be sure to check for necessary data before deleting.
{% endhint %}

1. Select one or more backups to delete.
2. Click the **Delete** button.
3. Review the deletion notice modal.
4. Click the **Delete** button.

### Archiving a Backup

Store a specific backup in long-term archive storage. Archived backups are not automatically deleted even after the retention period passes, and can be restored when needed.

{% hint style="info" %}
**Note**

* Backup archiving is only supported for Tibero in the AWS environment, and the archive list is not displayed in the Azure environment.
* Separate charges apply based on archive storage capacity, and charges for the minimum retention period of 90 days are billed regardless of whether the data is deleted.
{% endhint %}

1. Select one backup to archive.
2. Click the **Archive** button.
3. Enter a **Name**.
4. Select the **Archive Period**.
5. Click the **Archive** button.

{% hint style="info" %}
**Note**

This is only possible when the database operational status is not `Terminating`.
{% endhint %}

#### Viewing the Archive List

Check the list of backups in long-term archive in the **Archive** section within the Backup/Archive page.

<table><thead><tr><th width="197">Column</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Name entered when archiving</td></tr><tr><td>ID</td><td>ID automatically generated by the CSP</td></tr><tr><td>Created</td><td>Creation date and time of the original backup image (not the archive request date)</td></tr><tr><td>Expiration</td><td>Expiration date and time calculated from the archive period</td></tr><tr><td>Size (MB)</td><td>Total size of the archived backup image</td></tr><tr><td>Status</td><td>Restorable, Restoring, Deleted, Deleting</td></tr></tbody></table>

Archived backups with an expiration date within 30 days display a ⚠️ icon before the name. Hover over the icon to check the scheduled deletion date and time. If long-term archiving is needed, click **Edit** before expiration to extend the archive period.

1. Go to **Management > Backup/Restore > Backup**.
2. Scroll down the page to the **Archive** section.
3. Find the desired archived backup using filters or name search.

#### Editing an Archive

Edit the name and retention period of an archived backup.

1. Select one archived backup to edit.
2. Click the **Edit** button.
3. Change the **Name** or **Archive Period**.
4. Click the **Save** button.

#### Restoring an Archive

1. Select one archived backup to restore.
2. Click the **Restore** button.
3. Enter the restore date and time.
4. Click the **Restore** button.
5. Check the progress status in **Restore History**.

{% hint style="warning" %}
**Caution**

When restoring an archive, database restoration begins immediately and all currently stored backups are deleted. Depending on the data size, completion may take up to 3 days.
{% endhint %}

#### Deleting an Archive

1. Select one or more archived backups to delete.
2. Click the **Delete** button.

{% hint style="warning" %}
**Caution**

A deleted archive cannot be restored again, and all data is completely deleted.
{% endhint %}
