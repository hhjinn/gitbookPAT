**Management > Backup/Restore > Restore History** On this page, you can view and manage the history of database restore operations performed in the past.

The restore history provides detailed information such as the start and end date/time of restore operations attempted by the user and the progress status (success/failure). Restore types are classified into Full Restore and Point-in-Time Recovery (PITR). It supports period-based queries ranging from the last 1 day up to a maximum of 3 months as well as direct input, and provides type/status filters and name search functionality.

1. **OwlDB console screen > Management > Backup/Restore > Restore History** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose restore history you want to view.
3. View the restore history.
4. Filter by query period, restore type, and status.
5. Search by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query Period</td><td><ul><li>Last 1 day ~ maximum 3 months</li><li>Direct Input Period</li></ul></td></tr><tr><td>Restore Type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, Creation Date, Expiration Date</td><td>Direct Input Search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the restore fails, please select a point in time earlier than the previously selected point and try again. If the restore does not succeed after multiple attempts, please request technical support.
{% endhint %}
