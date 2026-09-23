**Management > Backup/Restore > Restore History** On this page, you can view and manage the history of database restore operations performed in the past.

The restore history provides detailed information such as the start and end times of restore operations attempted by the user and the progress status (success/failure). Restore types are classified into Full Restore and Point-in-Time Recovery (PITR). It supports queries by period, ranging from the most recent 1 day up to a maximum of 3 months as well as direct input, and provides type/status filters and name search functionality.

1. **OwlDB console screen > Management > Backup/Restore > Restore History** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose restore history you want to view.
3. View the restore history.
4. Filter by query period, restore type, and status.
5. Search by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query Period</td><td><ul><li>Most recent 1 day ~ up to 3 months</li><li>Directly entered period</li></ul></td></tr><tr><td>Restore Type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Direct input search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the restore fails, please select an earlier point in time than the one previously selected and try again. If the restore still does not succeed after multiple attempts, please request technical support.
{% endhint %}
