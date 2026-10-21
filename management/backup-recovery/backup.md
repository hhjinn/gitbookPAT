# Backup

Under **Management > Backup/Recovery > Backup**, view the database backup list and perform creation, modification, deletion, and recovery. It supports Full/Incremental Backup, which can be generated automatically through the scheduler or created manually by the user.

{% hint style="info" %}
**Note**

In an On-Premise environment, Full/Incremental Backup is supported, and the Archive feature is not provided.
{% endhint %}

### Viewing the Backup List <a href="#backup-list" id="backup-list"></a>

When you enter the **Management > Backup/Recovery > Backup** menu, the backup list of the current database appears. Incremental Backups are displayed nested in a tree structure with Full Backup as the root.

The columns displayed in the list are as follows.

| Item            | Description                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name            | Backup image name                                                                                                                                                                                                                                                                                                                                                                                                 |
| Backup Path     | Backup path name                                                                                                                                                                                                                                                                                                                                                                                                  |
| Backup method   | Full Backup or Incremental Backup                                                                                                                                                                                                                                                                                                                                                                                 |
| Creation date   | Date and time the backup image was created (yyyy.mm.dd HH\\:mm:ss)                                                                                                                                                                                                                                                                                                                                                |
| Expiration date | <ul><li><p>Expiration date and time calculated from the retention period (yyyy.mm.dd HH\:mm:ss)</p><ul><li>Automatic backup: backup execution time + retention period</li><li>Manual backup: 00\:00:00 on the day after the selected end date</li><li>Permanent retention: displayed as <code>-</code></li></ul></li><li>Incremental Backup: displays the expiration date of the associated Full Backup</li></ul> |
| Size(MB)        | <ul><li>Displays the total size of the backup image</li><li>Full Backup displays the sum of the sizes of subsequently created Incremental Backups</li><li>Incremental Backup displays only the size of that backup</li></ul>                                                                                                                                                                                      |
| Creation method | Automatic/Manual                                                                                                                                                                                                                                                                                                                                                                                                  |
| Status          | Current backup status                                                                                                                                                                                                                                                                                                                                                                                             |

#### Backup status history <a href="#undefined" id="undefined"></a>

| Item            | Description                                                                                                                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status          | <ul><li>Creation started</li><li>Recoverable</li><li>Creation failed</li><li>Recovery started</li><li>Recovery failed</li><li>Deletion started</li><li>Deleted</li><li>Unavailable</li></ul> |
| Occurrence date | Displays the date and time the status changed (yyyy.mm.dd HH\\:mm:ss)                                                                                                                        |

{% hint style="info" %}
**Note**

If you switch OpenBackup usage to **disabled** in OpenSQL, the status is displayed as deleted.
{% endhint %}

### Create backup <a href="#create-backup" id="create-backup"></a>

When you enter a name and retention period, a single backup image for the desired point in time is created. Tibero manages Full Backup units based on RMGR, and for OpenSQL the backup unit varies depending on whether the rsync or postgres method is selected.

1. Click the **Create** button on the **Backup** page.
2. Select the **Type**.
3. Enter the **Name**.
4. Select the end date of the **retention period** in the calendar. It is retained until the selected date and expires at 00:00:00 the next day.
5. Click the **Create** button.

{% hint style="info" %}
**Note**

* This is only possible when the database operation status is `Running`.
* An Incremental Backup can only be created when there is an available Full Backup.
{% endhint %}

### Recovery <a href="#restore" id="restore"></a>

When you select one backup from the backup list and click the **Recover** button, the recovery modal appears. Select a recovery type to recover the database to a specific point in time.

| Recovery type                 | Description                                                                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Full Restore                  | Recover all data up to the last commit point                                                                                  |
| PITR (Point-in-Time Recovery) | <ul><li>Recover up to a specified date and time (by the minute)</li><li>Data loss after the specified point in time</li></ul> |

{% hint style="warning" %}
**Caution**

When you perform a recovery, existing backups are deleted and the behavior of the Standby in the DR configuration varies depending on the recovery type and DB engine. Archive any data that requires long-term retention before recovery.
{% endhint %}

**Backup and DR impact by recovery type**

| Recovery type          | Existing backups                                                                                                                                                               | DR configuration Standby handling                                                                                                                                       |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Full Restore (Tibero)  | <ul><li>Retain existing backups</li><li>When recovering a selected Incremental Backup, the backup chain changes, so backups created after that backup cannot be used</li></ul> | Automatic resynchronization when the Standby restarts normally (registered DB and installed DB are the same)                                                            |
| Full Restore (OpenSQL) | Delete backups created after the selected backup                                                                                                                               | Automatic resynchronization when the Standby restarts normally                                                                                                          |
| PITR (Tibero)          | Delete all currently stored backups                                                                                                                                            | <ul><li>Installed DB: Standby is automatically rebuilt</li><li>Registered DB: Standby remains Down, and the administrator rebuilds it manually after recovery</li></ul> |
| PITR (OpenSQL)         | Delete backups and WAL created after the recovery point, retain backups prior to the specified time                                                                            | <ul><li>Installed DB: Standby is automatically rebuilt</li><li>Registered DB: Standby remains Down, and the administrator rebuilds it manually after recovery</li></ul> |

Recovery can take up to several hours, and you can check the progress in the recovery history.

1. Select one backup to recover.
2. Click the **Recover** button.
3. Select the **recovery type**.
4. If you select **PITR**, select the date and time to recover to. The selectable range is between the recoverable point of that backup and the current time.
5. Click the **Recover** button.

{% hint style="info" %}
**Note**

* This is only possible when the database operation status is `Running`, `Down`, or `Degraded`.
* For OpenSQL, the Incremental Backup creation behavior after recovery differs depending on the backup method. `rsync`: You can continue to create Incremental Backups on the existing backup chain even after recovery. `postgres`: Recovery branches into a new timeline, so the existing chain cannot be continued. You must perform a Full Backup once after recovery, and that backup becomes the new reference point. The `postgres` method can only be selected on PostgreSQL 17 or later.
* If recovery fails, please select a point in time earlier than the previously selected point and try again. If recovery does not succeed after multiple attempts, please request technical support.
{% endhint %}

### Edit backup <a href="#modify-backup" id="modify-backup"></a>

Edit the name and retention period of the backup.

1. Select one backup to edit.
2. Click the **Edit** button.
3. Change the **name** or **retention period**.
4. Click the **Save** button.

### Delete backup <a href="#delete-backup" id="delete-backup"></a>

Permanently delete the selected backup image.

1. Select one or more backups to delete.
2. Click the **Delete** button.
3. Confirm the deletion notice modal.
4. Click the **Delete** button.

{% hint style="warning" %}
**Caution**

Deleted backups cannot be recovered, so confirm once more that you have taken the necessary measures before deletion.

* **Tibero(RMGR)**: An Incremental Backup alone cannot be deleted if selected by itself. You must select it by Full Backup unit, in which case all sub Incremental Backups are deleted together.
* **OpenSQL(rsync)**: Full Backups and Incremental Backups can be selected individually without distinction, and only the selected backup is deleted without affecting other backups.
* **OpenSQL(postgres)**: An Incremental Backup can be selected by itself, and the Incremental Backups created after the selected backup are deleted together.
{% endhint %}
