On the **Management > Backup/Recovery > Recovery History** page, view and manage the history of past database recovery operations.

The recovery history provides detailed information such as the start and end date and time of recovery operations attempted by the user, and the progress status (success/failure). Recovery types are classified into Full Restore and Point-in-Time Recovery (PITR). It supports period-based queries from the last 1 day up to 3 months as well as direct input, and provides type and status filters and a name search function.

1. Go to the **OwlDB Console Screen > Management > Backup/Recovery > Recovery History** menu.
2. Click the **DB Alias** dropdown button to select the database whose recovery history you want to check.
3. View the recovery history.
4. Filter by query period, recovery type, and status.
5. Search by name, ID, creation date, or expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query Period</td><td><ul><li>Last 1 day to up to 3 months</li><li>Directly entered period</li></ul></td></tr><tr><td>Restore type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Direct input search</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the recovery fails, please select an earlier point in time than the one you previously selected and try again. If recovery still fails after several attempts, please request technical support.
{% endhint %}
