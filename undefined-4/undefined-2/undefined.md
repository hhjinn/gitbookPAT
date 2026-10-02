**Management > Backup/Restore > Restore History** On this page, you can view and manage the history of database restore operations performed in the past.

The restore history provides detailed information such as the start and end times and progress status (success/failure) of restore operations attempted by the user. Restore types are categorized into Full Restore and Point-in-Time Recovery (PITR). It supports queries by period ranging from the last 1 day up to 3 months as well as direct input, and provides type and status filters along with a name search function.

1. **OwlDB Console Screen > Management > Backup/Restore > Restore History** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose restore history you want to view.
3. View the restore history.
4. Filter by query period, restore type, and status.
5. Search by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query Period</td><td><ul><li>Last 1 day to up to 3 months</li><li>Direct input period</li></ul></td></tr><tr><td>Recovery type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Direct input search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If recovery fails, please select a point earlier than the previously selected point and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}
