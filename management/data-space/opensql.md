From the **Management > Data Space Management** menu, you can view and manage databases belonging to an OpenSQL instance. In the database list, you can see each database's size, number of active sessions, and Tuple Health status at a glance, and create or delete databases. Clicking a database alias lets you also review detailed information such as Encoding, Connection Limit, and Bloat Ratio, along with trend metrics.

{% hint style="info" %}
**Note**

- OpenSQL database management is available only in the Azure environment. It is not supported in the AWS environment.
- If the DB engine is set to Tibero, the tablespace and data file management screen is displayed instead of this page.
{% endhint %}

<figure>
<img src="../../.gitbook/assets/image-61986e5d.png" alt="">
<figcaption>Figure 1. Data Space - OpenSQL</figcaption>
</figure>

## View database list <a href="#database-list" id="database-list"></a>

When you enter the **Management > Data Space Management** menu, the list of databases belonging to the current instance is displayed. A status summary for the entire instance is shown at the top of the page, and you can review the detailed status of each database in the central table.

### Top summary information <a href="#undefined" id="undefined"></a>

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is enabled |
| Number of databases | Total number of databases under the instance |
| Active Session Count | Sum of active sessions across all databases (bar chart) |
| Total DB Size | Total size of all databases |
| WAL Size | Write-Ahead Log size |

### Table items <a href="#undefined-1" id="undefined-1"></a>

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Alias</td><td><ul><li>Database name</li><li>Clicking moves to the detailed information page</li></ul></td></tr><tr><td>Creation date</td><td>Database creation date and time</td></tr><tr><td>Owner</td><td>Database owner user</td></tr><tr><td>Encoding</td><td>Character Set configured for the database</td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td></tr><tr><td>Data Size</td><td>Capacity occupied by actual data (GB)</td></tr><tr><td>Number of active sessions</td><td>Number of currently active sessions (bar chart)</td></tr><tr><td>Tuple Health</td><td>A status indicator combining the Dead Tuple ratio and Vacuum execution time</td></tr><tr><td>Bloat Ratio</td><td>Ratio of Dead Tuples to the total size (%)</td></tr><tr><td>Live/Dead Tuple Rate</td><td>Ratio of Dead Tuples to Live Tuples (%)</td></tr><tr><td>Last Vacuum execution time</td><td>Date and time when the last Vacuum was performed</td></tr></tbody></table>

The **Tuple Health** status is determined based on the Bloat Ratio and the last Vacuum execution time.

| 3xCdwgyaRWkM | Vacuum normal (less than 24 hours) | Vacuum caution (24–72 hours) | Vacuum critical (72 hours or more) |
| --- | --- | --- | --- |
| Bloat Ratio normal (less than 20%) | Healthy | Watch | Critical |
| Bloat Ratio caution (20–40%) | Watch | Watch | Critical |
| Bloat Ratio critical (40% or more) | Critical | Critical | Critical |

{% hint style="info" %}
**Note**

Tuple Health is a guide based on estimated statistics, and the actual performance impact may vary depending on traffic patterns.
{% endhint %}

You can find a specific database by entering its alias in the search box. Clicking a column header sorts in ascending/descending order, and the default sort criterion is alias ascending. Clicking the 🔃 icon manually refreshes the page data.

{% hint style="info" %}
**Note**

`postgres`, `template0`, and `template1` are system default databases; they appear in the list but cannot be selected or deleted.
{% endhint %}

1. Click the **Management > Data Space Management** menu.
2. Check the status in the top summary information and the database table.
3. To find a specific database, enter its alias in the search box.

## Create database <a href="#create-database" id="create-database"></a>

Clicking the **Create** button opens the database creation drawer on the right side of the screen. After entering the required fields, clicking the **Create** button sends the creation request and closes the drawer. The result of the creation request and whether it is complete can be confirmed via the toast message displayed at the top of the screen.

{% hint style="warning" %}
**Caution**

Clicking **Cancel** or closing the drawer resets all entered content. Changes are not saved until you click the **Create** button.
{% endhint %}

### Input items <a href="#undefined-2" id="undefined-2"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Database Name *</td><td>Name of the database to create</td><td><ul><li>Only lowercase English letters (a-z), numbers (0-9), and underscores (<code>_</code>) within 30 characters are allowed</li><li>Cannot be duplicated within the same instance</li></ul></td></tr><tr><td>Owner *</td><td>Database owner user</td><td><code>postgres</code> (fixed value)</td></tr><tr><td>Encoding</td><td>Database Character Set</td><td>Default value: <code>UTF8</code></td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td><td><ul><li>Default value: Unlimited</li><li>Can be entered directly when Unlimited is unchecked (an integer of 0 or more)</li></ul></td></tr></tbody></table>

The * notation indicates a required input item.

When **Unlimited** is checked, connections are unlimited within the instance's `max_connections` range. To limit the number of connections, uncheck the checkbox and enter the desired value.

1. On the **Management > Data Space Management** page, click the **Create** button.
2. In the database creation drawer, enter the **Database Name** and the required fields.
3. Click the **Create** button.
4. When the drawer closes, check the creation request result and completion status via the toast message.

## View database detailed information <a href="#database-details" id="database-details"></a>

Clicking a database alias in the list moves you to that database's detailed information page. The page consists of three areas: basic information (Info), Database Activity, and Trend Metrics.

Clicking the 🔃 icon manually refreshes the page data. Clicking the parent menu name in the breadcrumb at the top of the page returns you to the Data Space Management list.

1. On the **Management > Data Space Management** page, click the **alias** of the database you want to view from the database list.
2. Check the status and current state in the basic information, Database Activity, and Trend Metrics areas.

### Displayed items <a href="#undefined-3" id="undefined-3"></a>

**Basic information (Info)**

| Item | Description |
| --- | --- |
| Creation date | Database creation date and time |
| Tuple Health | A status indicator based on the Dead Tuple ratio and Vacuum execution time (Healthy / Watch / Critical) |
| Last Vacuum execution time | Date and time when the last Vacuum was performed |
| Owner | Database owner user |
| Encoding | Character Set configured for the database |
| Connection Limit | Maximum number of concurrent connections allowed |

**Database Activity**

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Size</td><td>Capacity occupied by actual data (GB)</td></tr><tr><td>Number of active sessions</td><td><ul><li>Number of currently active sessions (line chart)</li><li>Displays a separate line for each node's sessions in an HA configuration</li></ul></td></tr></tbody></table>

**Trend Metrics**

| Item | Description |
| --- | --- |
| Bloat Ratio (%) | Ratio of Dead Tuples to the total size |
| Live Tuple Count (CNT) | Number of Live Tuples in the database |
| Dead Tuple Count (CNT) | Number of Dead Tuples in the database |
| Live/Dead Tuple Rate (%) | Ratio of Dead Tuples to Live Tuples |

## Deleting a Database <a href="#delete-database" id="delete-database"></a>

A database can be deleted from two places: the list page and the detail page. Clicking the delete button opens a confirmation modal, and the deletion proceeds after final confirmation in the modal.

{% hint style="warning" %}
**Caution**

The deletion operation cannot be undone. All data and objects contained in the database are permanently deleted, and connected applications may experience an immediate failure.
{% endhint %}

1. On the **Management > Data Space Management** page, go to the database you want to delete. **[List Page]** Select the database to delete using the radio button. **[Detail Page]** Click the **alias** of the database to delete to move to the detail page.
2. Click the **Delete** button.
3. In the delete confirmation modal, review the deletion target and the scope of impact.
4. Click the **Delete** button.
5. Check the deletion result in the toast message.
