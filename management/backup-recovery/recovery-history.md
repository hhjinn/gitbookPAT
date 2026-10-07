On the **Management > Backup/Recovery > Recovery History** page, you can view and manage the history of database recovery operations performed in the past.

The recovery history provides detailed information such as the start and end date and time of recovery operations attempted by the user and the progress status (success/failure). Recovery types are classified into Full Restore and Point-In-Time Recovery (PITR). It supports period-based queries from the last 1 day up to a maximum of 3 months as well as through direct input, and provides type and status filters and a name search function.

1. Navigate to the **OwlDB console screen > Management > Backup/Recovery > Recovery History** menu.
2. Click the **DB Alias** dropdown button to select the database whose recovery history you want to check.
3. Views the recovery history.
4. Filters by query period, recovery type, and status.
5. Searches by name, ID, creation date, and expiration date.

The filter and search items are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Query period</td><td><ul><li>Last 1 day to a maximum of 3 months</li><li>Manually entered period</li></ul></td></tr><tr><td>Recovery type</td><td><ul><li>Full Restore</li><li>Point-in-Time Recovery (PITR)</li></ul></td></tr><tr><td>Status</td><td>Success/Failure</td></tr><tr><td>Name, ID, creation date, expiration date</td><td>Manual search entry</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If recovery fails, please select a point in time earlier than the previously selected point and try again. If recovery does not succeed after multiple attempts, please request technical support.
{% endhint %}
