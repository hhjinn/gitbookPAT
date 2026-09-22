**Management > Backup/Recovery > Recovery History** On this page, you can view and manage the history of database recovery operations performed in the past.

The recovery history provides detailed information such as the start and end date/time of recovery operations attempted by the user and the progress status (success/failure). Recovery types are classified as Full Restore and Point-in-Time Recovery (PITR). It supports queries by period from the last 1 day up to 3 months as well as through direct input, and provides type/status filters and a name search function.

1. **OwlDB console screen > Management > Backup/Recovery > Recovery History** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose recovery history you want to view.
3. View the recovery history.
4. Filter by query period, recovery type, and status.
5. Search by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query period</td><td><ul><li>Last 1 day to up to 3 months</li><li>Direct input period</li></ul></td></tr><tr><td>Recovery type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Direct input search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If recovery fails, please select a point in time earlier than the previously selected one and try again. If recovery still does not succeed after multiple attempts, please request technical support.
{% endhint %}
