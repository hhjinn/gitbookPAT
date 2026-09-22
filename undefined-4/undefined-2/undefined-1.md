**Management > Backup/Recovery > Backup**Query the database backup list and perform creation, modification, deletion, and recovery. Full/Incremental Backup is supported, and backups can be generated automatically through the scheduler or created manually by the user.

{% hint style="info" %}
**Note**

In On-Premise environments, Full/Incremental Backup is supported, but the Archive feature is not provided.
{% endhint %}

## Viewing the Backup List

**Management > Backup/Recovery > Backup** When you enter the menu, the backup list of the current database appears. With Full Backup as the root, Incremental Backups are displayed nested in a tree structure.

The columns displayed in the list are as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup Path</td><td>Backup path name</td></tr><tr><td>Backup Method</td><td>Full Backup or Incremental Backup</td></tr><tr><td>Creation Date</td><td>Date and time the backup image was created (yyyy.mm.dd HH:mm:ss)</td></tr><tr><td>Expiration Date</td><td><ul><li>Expiration date and time calculated from the retention period (yyyy.mm.dd HH:mm:ss)</li><li>Incremental Backup displays the expiration date of its associated Full Backup as is</li></ul></td></tr><tr><td>Size(MB)</td><td><ul><li>Displays the total capacity of the backup image</li><li>Full Backup displays the sum of the capacities of subsequently created Incremental Backups</li><li>Incremental Backup displays only the capacity of that backup</li></ul></td></tr><tr><td>Creation Method</td><td>Automatic/Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

### Backup Status History

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation started</li><li>Recoverable</li><li>Creation failed</li><li>Recovery started</li><li>Recovery failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time the status changed (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When you switch OpenBackup usage in OpenSQL to **Disabled**, the status is displayed as Deleted.
{% endhint %}

## Creating a Backup

When you enter a name and retention period, a single backup image at the desired point in time is created. Tibero manages Full Backup units based on RMGR, and for OpenSQL the backup unit varies depending on whether the rsync or postgres method is selected.

1. **Backup** On the page, **Create** Click the button.
2. **Type**Select.
3. **Name**Enter.
4. From the calendar, **Retention Period**Select the end date of.
5. **Create** Click the button.

{% hint style="info" %}
**Note**

- Only available when the database operation status is `Running`.
- Incremental Backup can only be created when there is an available Full Backup.
{% endhint %}

## Recovery

Select one backup from the backup list and **Recover** When you click the button, the recovery modal appears. Select a recovery type to recover the database to a specific point in time.

<table data-full-width="true"><thead><tr><th>Recovery Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Recovers all data up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovers up to a specified date and time (in minutes)</li><li>Data after the specified point in time is lost</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

When you run recovery, existing backups are deleted and the behavior of the DR-configured Standby changes depending on the recovery type and DB engine. Archive data that requires long-term retention in advance before recovery.
{% endhint %}

**Backup/DR Impact by Recovery Type**

<table data-full-width="true"><thead><tr><th>Restore type</th><th>Existing backups</th><th>DR configuration Standby handling</th></tr></thead><tbody><tr><td>Full Restore (Tibero)</td><td><ul><li>Retain existing backups</li><li>For Incremental Backup selective restore, changes to the backup chain make backups created after that backup unusable</li></ul></td><td>Automatic resynchronization on normal Standby restart (registered DB and installed DB identical)</td></tr><tr><td>Full Restore (OpenSQL)</td><td>Delete backups created after the selected backup</td><td>Automatic resynchronization on normal Standby restart</td></tr><tr><td>PITR (Tibero)</td><td>Delete all currently stored backups</td><td><ul><li>Installed DB: Standby automatic rebuild</li><li>Registered DB: Standby remains Down, administrator manually rebuilds after restore</li></ul></td></tr><tr><td>PITR (OpenSQL)</td><td>Delete backups and WAL created after the restore point, retain backups prior to the specified time</td><td><ul><li>Installed DB: Standby automatic rebuild</li><li>Registered DB: Standby remains Down, administrator manually rebuilds after restore</li></ul></td></tr></tbody></table>

A restore may take up to several hours, and you can check its progress in the restore history.

1. Select one backup to restore.
2. **Restore** Click the button.
3. **Restore type**Select it.
4. **PITR**If you selected it, select the date and time to restore to. The selectable range is between the recoverable point of that backup and the current time.
5. **Restore** Click the button.

{% hint style="info" %}
**Note**

- The database operation status is `Running`, `Down`, `Degraded`It is only possible when it is.
- For OpenSQL, the Incremental Backup creation behavior after restore differs depending on the backup method. `rsync` : Even after a restore, you can continue the existing backup chain to create Incremental Backups. `postgres` : On restore, it branches to a new timeline and the existing chain cannot be continued. You must perform a Full Backup once after restore, and that backup becomes the new baseline. `postgres` This method can only be selected on PostgreSQL 17 or later.
- If the restore fails, please select a point earlier than the previously selected point and try again. If the restore still fails after several attempts, please request technical support.
{% endhint %}

## Edit backup

Edit the name and retention period of the backup.

1. Select one backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention period**Change it.
4. **Save** Click the button.

## Delete backup

Permanently delete the selected backup image.

1. Select one or more backups to delete.
2. **Delete** Click the button.
3. Confirm the deletion notice modal.
4. **Delete** Click the button.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be restored, so verify once more that you have taken the necessary measures before deleting.

- **Tibero(RMGR)**: An Incremental Backup cannot be deleted if selected alone. It must be selected at the Full Backup level, in which case all subordinate Incremental Backups are deleted together.
- **OpenSQL(rsync)**: Full Backups and Incremental Backups can be selected individually without distinction, and only the selected backup is deleted without affecting other backups.
- **OpenSQL(postgres)**: An Incremental Backup can be selected alone, and Incremental Backups created after the selected backup are also deleted together.
{% endhint %}
