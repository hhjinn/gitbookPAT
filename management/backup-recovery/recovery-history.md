**Management > Backup/Recovery > Recovery History** On this page, you can view and manage the history of database recovery operations performed in the past.

The recovery history provides detailed information such as the start and end date/time of recovery operations attempted by the user and their progress status (success/failure). Recovery types are classified into Full Restore and Point-in-Time Recovery (PITR). It supports period-based queries from the last 1 day up to 3 months as well as manual entry, and provides type/status filters and a name search function.

1. **OwlDB console screen > Management > Backup/Recovery > Recovery History** Go to the menu.
2. **DB Alias** Click the dropdown button to select the database for which to check the recovery history.
3. View the recovery history.
4. Filter by query period, recovery type, and status.
5. Search by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query period</td><td><ul><li>Last 1 day ~ up to 3 months</li><li>Manually entered period</li></ul></td></tr><tr><td>Recovery Type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Manual entry search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If recovery fails, please select a point earlier than the previously selected one and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}
