# Backup

Under **Management > Backup/Recovery > Backup**, you can view the database backup list and perform creation, modification, deletion, and recovery. It supports Full/Incremental Backup, which can be created automatically through a scheduler or manually by the user.

{% hint style="info" %}
**Note**

In On-Premise environments, Full/Incremental Backup is supported, but the Archive feature is not provided.
{% endhint %}

### Viewing the Backup List

When you enter the **Management > Backup/Recovery > Backup** menu, the backup list of the current database appears. With Full Backup as the root, Incremental Backups are displayed nested in a tree structure.

The columns displayed in the list are as follows.

<table data-full-width="true"><thead><tr><th width="210">Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup Path</td><td>Backup path name</td></tr><tr><td>Backup Method</td><td>Full Backup or Incremental Backup</td></tr><tr><td>Created Date</td><td>Date and time the backup image was created (yyyy.mm.dd HH:mm:ss)</td></tr><tr><td>Expiration Date</td><td><ul><li><p>Expiration date and time calculated from the retention period (yyyy.mm.dd HH:mm:ss)</p><ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00:00:00 of the day after the selected end date</li><li>Permanent retention: displayed as <code>-</code></li></ul></li><li>Incremental Backup: displays the expiration date of the associated Full Backup</li></ul></td></tr><tr><td>Size(MB)</td><td><ul><li>Displays the total size of the backup image</li><li>Full Backup displays the sum including the sizes of Incremental Backups created afterward</li><li>Incremental Backup displays only the size of that backup</li></ul></td></tr><tr><td>Creation Method</td><td>Automatic/Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

#### Backup Status History

<table data-full-width="true"><thead><tr><th width="264">Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation Started</li><li>Recoverable</li><li>Creation Failed</li><li>Recovery Started</li><li>Recovery Failed</li><li>Deletion Started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time the status changed (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you switch OpenBackup usage to **Disabled** in OpenSQL, the status is displayed as Deleted.
{% endhint %}

### Creating a Backup

When you enter a name and retention period, a single backup image of the desired point in time is created. Tibero manages Full Backup units based on RMGR, and for OpenSQL, the backup unit varies depending on whether the rsync or postgres method is selected.

1. On the **Backup** page, click the **Create** button.
2. Select the **Type**.
3. Enter the **Name**.
4. Select the end date of the **retention period** in the calendar. The backup is retained until the selected date and expires at 00:00:00 the next day.
5. Click the **Create** button.

{% hint style="info" %}
**Note**

* This is only possible when the database operation status is `Running`.
* Incremental Backup can only be created when there is an available Full Backup.
{% endhint %}

### Recovery

Select one backup from the backup list and click the **Recover** button to display the recovery modal. Select a recovery type to recover the database to a specific point in time.

<table data-full-width="true"><thead><tr><th>Recovery Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Recovers all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovers up to a specified date and time (in minutes)</li><li>Data after the specified point in time is lost</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

When you perform recovery, existing backups are deleted and the behavior of the DR-configured Standby changes depending on the recovery type and DB engine. Archive any data that requires long-term retention before recovery.
{% endhint %}

**Backup/DR Impact by Recovery Type**

<table data-full-width="true"><thead><tr><th>Recovery Type</th><th>Existing Backups</th><th>DR-Configured Standby Handling</th></tr></thead><tbody><tr><td>Full Restore (Tibero)</td><td><ul><li>Existing backups retained</li><li>When recovering a selected Incremental Backup, backups created after that backup become unavailable due to backup chain changes</li></ul></td><td>Automatically resynchronized when Standby restarts normally (registered DB and installed DB are identical)</td></tr><tr><td>Full Restore (OpenSQL)</td><td>Backups created after the selected backup are deleted</td><td>Automatically resynchronized when Standby restarts normally</td></tr><tr><td>PITR (Tibero)</td><td>All currently stored backups are deleted</td><td><ul><li>Installed DB: Standby automatically rebuilt</li><li>Registered DB: Standby remains Down, administrator rebuilds it directly after recovery</li></ul></td></tr><tr><td>PITR (OpenSQL)</td><td>Backups and WAL created after the recovery point are deleted, backups prior to the specified time are retained</td><td><ul><li>Installed DB: Standby automatically rebuilt</li><li>Registered DB: Standby remains Down, administrator rebuilds it directly after recovery</li></ul></td></tr></tbody></table>

Recovery can take up to several hours, and you can check the progress in the recovery history.

1. Select one backup to recover.
2. Click the **Recover** button.
3. Select the **Recovery Type**.
4. If you selected **PITR**, select the date and time to recover to. The selectable range is between the recoverable point of that backup and the current time.
5. Click the **Recover** button.

{% hint style="info" %}
**Note**

* This is only possible when the database operation status is `Running`, `Down`, or `Degraded`.
* For OpenSQL, the Incremental Backup creation behavior after recovery differs depending on the backup method. `rsync`: You can continue the existing backup chain to create Incremental Backups even after recovery. `postgres`: Recovery branches to a new timeline, so the existing chain cannot be continued. You must perform a Full Backup once after recovery, and that backup becomes the new reference point. The `postgres` method can only be selected on PostgreSQL 17 or later.
* If recovery fails, please select an earlier point in time than the one previously selected and try again. If recovery does not succeed after multiple attempts, please request technical support.
{% endhint %}

### Modifying a Backup

Modify the name and retention period of a backup.

1. Select one backup to modify.
2. Click the **Edit** button.
3. Change the **Name** or **retention period**.
4. Click the **Save** button.

### Deleting a Backup

Permanently delete the selected backup image.

1. Select one or more backups to delete.
2. Click the **Delete** button.
3. Confirm the deletion guide modal.
4. Click the **Delete** button.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be recovered, so verify once more that you have taken the necessary measures before deletion.

* **Tibero(RMGR)**: An Incremental Backup cannot be deleted if selected alone. It must be selected as a Full Backup unit, in which case all subordinate Incremental Backups are deleted together.
* **OpenSQL(rsync)**: Full Backups and Incremental Backups can be selected individually without distinction, and only the selected backup is deleted without affecting other backups.
* **OpenSQL(postgres)**: An Incremental Backup can be selected alone, and Incremental Backups created after the selected backup are also deleted together.
{% endhint %}
