**Management > Backup/Recovery > Backup**Queries the database backup list and performs create, modify, delete, and recovery operations. It supports Full/Incremental Backup, which can be generated automatically through the scheduler or created manually by the user.

{% hint style="info" %}
**Note**

In On-Premise environments, Full/Incremental Backup is supported, but the archive feature is not provided.
{% endhint %}

## Querying the backup list

**Management > Backup/Recovery > Backup** When you enter the menu, the backup list of the current database appears. With the Full Backup as the root, Incremental Backups are displayed nested in a tree structure.

The columns displayed in the list are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Backup image name</td></tr><tr><td>Backup Path</td><td>Backup path name</td></tr><tr><td>Backup method</td><td>Full Backup or Incremental Backup</td></tr><tr><td>Creation date</td><td>Date and time the backup image was created (yyyy.mm.dd HH\:mm:ss)</td></tr><tr><td>Expiration date</td><td><ul><li>Expiration date and time calculated from the retention period (yyyy.mm.dd HH\:mm:ss)<ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00\:00:00 on the day after the selected end date</li><li>Permanent retention: <code>-</code> Displayed as</li></ul></li><li>Incremental Backup: displays the expiration date of the associated Full Backup</li></ul></td></tr><tr><td>Size(MB)</td><td><ul><li>Displays the total capacity of the backup image</li><li>Full Backup displays the sum including the capacity of subsequently created Incremental Backups</li><li>Incremental Backup displays only the capacity of that backup</li></ul></td></tr><tr><td>Creation method</td><td>Automatic/Manual</td></tr><tr><td>Status</td><td>Current backup status</td></tr></tbody></table>

### Backup status history

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Status</td><td><ul><li>Creation started</li><li>Recoverable</li><li>Creation failed</li><li>Recovery started</li><li>Recovery failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul></td></tr><tr><td>Occurrence date</td><td>Displays the date and time the status changed (yyyy.mm.dd HH\:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**Note**

Using OpenBackup in OpenSQL **Disabled**When switched to, the status is displayed as Deleted.
{% endhint %}

## Creating a backup

When you enter a name and a retention period, a single backup image at the desired point in time is created. Tibero manages Full Backup units based on RMGR, and for OpenSQL the backup unit varies depending on whether the rsync or postgres method is selected.

1. **Backup** On the page, **Create** Click the button.
2. **Type**.
3. **Name**Enter.
4. In the calendar, **Retention period**Select the end date. It is retained until the selected date and expires at 00\:00:00 the next day.
5. **Create** Click the button.

{% hint style="info" %}
**Note**

- Only possible when the database operating status is `Running`.
- An Incremental Backup can only be created when an available Full Backup exists.
{% endhint %}

## Recovery

Select one backup from the backup list and **Recovery** When you click the button, the recovery modal appears. Select a recovery type to recover the database to a specific point in time.

<table><thead><tr><th>Recovery type</th><th>Description</th></tr></thead><tbody><tr><td>Full Restore</td><td>Full data recovery up to the last commit point</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>Recovery up to a specified date and time (by the minute)</li><li>Data loss after the specified point in time</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

When you run a recovery, depending on the recovery type and DB engine, existing backups are deleted and the behavior of the DR-configured Standby changes. Archive any data that requires long-term retention before recovery.
{% endhint %}

**Backup/DR impact by recovery type**

<table><thead><tr><th>Recovery type</th><th>Existing backups</th><th>DR-configured Standby handling</th></tr></thead><tbody><tr><td>Full Restore (Tibero)</td><td><ul><li>Retain existing backups</li><li>When recovering a selected Incremental Backup, backups created after that backup become unavailable due to the backup chain change</li></ul></td><td>Automatic resynchronization when the Standby restarts normally (same for registered DB and installed DB)</td></tr><tr><td>Full Restore (OpenSQL)</td><td>Delete backups created after the selected backup</td><td>Automatic resynchronization when the Standby restarts normally</td></tr><tr><td>PITR (Tibero)</td><td>Delete all currently stored backups</td><td><ul><li>Installed DB: Standby is automatically rebuilt</li><li>Registered DB: Standby remains Down; the administrator manually rebuilds it after recovery</li></ul></td></tr><tr><td>PITR (OpenSQL)</td><td>Delete backups/WAL created after the recovery point, retain backups prior to the specified time</td><td><ul><li>Installed DB: Standby is automatically rebuilt</li><li>Registered DB: Standby remains Down; the administrator manually rebuilds it after recovery</li></ul></td></tr></tbody></table>

Recovery may take up to several hours, and you can check the progress in the recovery history.

1. Select one backup to recover.
2. **Recovery** Click the button.
3. **Recovery type**.
4. **PITR**If you selected, select the date and time to recover. The selectable range is between the recoverable point of that backup and the current time.
5. **Recovery** Click the button.

{% hint style="info" %}
**Note**

- Only possible when the database operating status is `Running`, `Down`, `Degraded`.
- In OpenSQL, the behavior of creating an Incremental Backup after recovery differs depending on the backup method. `rsync` : Even after recovery, you can continue the existing backup chain to create Incremental Backups. `postgres` : Recovery branches into a new timeline, so the existing chain cannot be continued. A Full Backup must be performed once after recovery, and that backup becomes the new reference point. `postgres` This method can only be selected on PostgreSQL 17 or later.
- If recovery fails, please select a point earlier than the previously selected point and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}

## Edit Backup

Edit the name and retention period of a backup.

1. Select one backup to edit.
2. **Edit** Click the button.
3. **Name** or **Retention period**Change the .
4. **Save** Click the button.

## Delete Backup

Permanently delete the selected backup image.

1. Select one or more backups to delete.
2. **Delete** Click the button.
3. Confirm the deletion notice modal.
4. **Delete** Click the button.

{% hint style="warning" %}
**Caution**

Since deleted backups cannot be recovered, verify once again that you have taken any necessary measures before deletion.

- **Tibero(RMGR)**: An Incremental Backup cannot be deleted if selected on its own. It must be selected at the Full Backup level, in which case all child Incremental Backups are deleted together.
- **OpenSQL(rsync)**: Full Backups and Incremental Backups can be selected individually without distinction, and only the selected backups are deleted without affecting other backups.
- **OpenSQL(postgres)**: An Incremental Backup can be selected on its own, and Incremental Backups created after the selected backup are deleted together with it.
{% endhint %}
