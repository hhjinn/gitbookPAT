**Management > Backup/Recovery > Backup**Query the database backup list and perform creation, modification, deletion, and recovery here. It supports Full/Incremental Backup, and backups can be created automatically through the scheduler or manually by the user.

{% hint style="info" %}
**Note**

In On-Premise environments, Full/Incremental Backup is supported, but the Archive feature is not provided.
{% endhint %}

## Backup List Query <a href="#backup-list" id="backup-list"></a>

**Management > Backup/Recovery > Backup** When you enter the menu, the backup list of the current database appears. With Full Backup as the root, Incremental Backups are displayed nested in a tree structure.

The columns displayed in the list are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup Path</td><td>Backup path name</td></tr><tr><td>Backup method</td><td>Full Backup or Incremental Backup</td></tr><tr><td>Creation date</td><td>Date and time the backup image was created (yyyy.mm.dd HH\:mm:ss)</td></tr><tr><td>Expiration Date</td><td><ul><li>Expiration date and time calculated from the retention period (yyyy.mm.dd HH\:mm:ss)<ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00\:00:00 of the day after the selected end date</li><li>Permanent retention: <code>-</code> displayed as</li></ul></li><li>Incremental Backup: Displays the expiration date of the associated Full Backup</li></ul></td></tr><tr><td>Size(MB)</td><td><ul><li>Displays the total capacity of the backup image</li><li>Full Backup displays the sum of the capacities of subsequently created Incremental Backups</li><li>Incremental Backup displays only the capacity of that backup</li></ul></td></tr><tr><td>Creation Method</td><td>Automatic/Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

### Backup status history <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation started</li><li>Recoverable</li><li>Creation failed</li><li>Recovery started</li><li>Recovery failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence Date</td><td>Displays the date and time the status changed (yyyy.mm.dd HH\:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you switch the use of OpenBackup in OpenSQL **Not Used**, the status is displayed as Deleted.
{% endhint %}

## Backup Creation <a href="#create-backup" id="create-backup"></a>

When you enter a name and retention period, a single backup image at the desired point in time is created. Tibero manages Full Backup units based on RMGR, and for OpenSQL, the backup unit varies depending on whether the rsync or postgres method is selected.

1. **Backup** On the page, **Create** Click the button.
2. **Type**is selected.
3. **Name**Enter.
4. In the calendar **Retention Period**Select the end date. It is retained until the selected date and expires at 00\:00:00 the next day.
5. **Create** Click the button.

{% hint style="info" %}
**Note**

- When the database operation status is `Running`, it is only possible.
- Incremental Backup can only be created when there is an available Full Backup.
{% endhint %}

## Recovery <a href="#restore" id="restore"></a>

Select one backup from the backup list and **Recovery** Click the button and the recovery modal appears. Select a recovery type to recover the database to a specific point in time.

<table><thead><tr><th>Recovery Type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Full data recovery up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovery up to a specified date and time (minute unit)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

When you execute recovery, existing backups are deleted and the behavior of the DR-configured Standby changes depending on the recovery type and DB engine. Retain data that requires long-term retention in advance before recovery.
{% endhint %}

**Backup/DR Impact by Recovery Type**

<table><thead><tr><th>Recovery Type</th><th>Existing backup</th><th>DR configuration Standby handling</th></tr></thead><tbody><tr><td>Full Restore (Tibero)</td><td><ul><li>Retain existing backups</li><li>When selectively recovering an Incremental Backup, backups created after that backup cannot be used due to the backup chain change</li></ul></td><td>Automatic resynchronization upon normal Standby restart (registered DB and installed DB are the same)</td></tr><tr><td>Full Restore (OpenSQL)</td><td>Delete backups created after the selected backup</td><td>Automatic resynchronization upon normal Standby restart</td></tr><tr><td>PITR (Tibero)</td><td>Delete all currently stored backups</td><td><ul><li>Installed DB: Automatic Standby rebuild</li><li>Registered DB: Keeps Standby Down, administrator manually rebuilds after recovery</li></ul></td></tr><tr><td>PITR (OpenSQL)</td><td>Deletes backups and WAL created after the recovery point, retains backups prior to the specified time</td><td><ul><li>Installed DB: Automatic Standby rebuild</li><li>Registered DB: Keeps Standby Down, administrator manually rebuilds after recovery</li></ul></td></tr></tbody></table>

Recovery can take up to several hours, and you can check its progress in the recovery history.

1. Select one backup to recover.
2. **Recovery** Click the button.
3. **Recovery Type**is selected.
4. **PITR**If you select it, choose the date and time to recover. The selectable range is between that backup's recoverable point and the current time.
5. **Recovery** Click the button.

{% hint style="info" %}
**Note**

- When the database operation status is `Running`, `Down`, `Degraded`, it is only possible.
- OpenSQL behaves differently in creating an Incremental Backup after recovery depending on the backup method. `rsync` : Even after recovery, you can continue the existing backup chain and create an Incremental Backup. `postgres` : Upon recovery, it branches into a new timeline and cannot continue the existing chain. You must perform a Full Backup once after recovery, and that backup becomes the new baseline. `postgres` The method can only be selected on PostgreSQL 17 or later.
- If recovery fails, please select a point earlier than the previously selected one and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}

## Edit backup <a href="#modify-backup" id="modify-backup"></a>

Edit the name and retention period of the backup.

1. Select one backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention Period**Change it.
4. **Save** Click the button.

## Delete backup <a href="#delete-backup" id="delete-backup"></a>

Permanently delete the selected backup image.

1. Select one or more backups to delete.
2. **Delete** Click the button.
3. Confirm the deletion notice modal.
4. **Delete** Click the button.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be recovered, so double-check that you have taken the necessary measures before deletion.

- **Tibero(RMGR)**: An Incremental Backup alone cannot be deleted on its own. It must be selected by Full Backup unit, in which case all subordinate Incremental Backups are deleted together.
- **OpenSQL(rsync)**: Full Backups and Incremental Backups can be selected individually without distinction, and only the selected backup is deleted without affecting other backups.
- **OpenSQL(postgres)**: An Incremental Backup can be selected on its own, and Incremental Backups created after the selected backup are also deleted together.
{% endhint %}
