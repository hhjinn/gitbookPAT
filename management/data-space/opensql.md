On the **Management > Data Space Management** menu, you can view and manage the databases belonging to the OpenSQL instance. From the database list, you can grasp at a glance each database's size, number of active sessions, and Tuple Health status, and you can create and delete databases. Clicking a database alias lets you also check detailed information such as Encoding, Connection Limit, and Bloat Ratio, along with trend indicators.

{% hint style="info" %}
**Note**

- OpenSQL database management is available only in the Azure environment. It is not supported in the AWS environment.
- If the DB engine is set to Tibero, the tablespace and data file management screen is displayed instead of this page.
{% endhint %}

<figure>
<img src="../../.gitbook/assets/image-61986e5d.png" alt="">
<figcaption>Figure 1. Data Space - OpenSQL</figcaption>
</figure>

## Viewing the database list

**Management > Data Space Management** When you enter the menu, the list of databases belonging to the current instance is displayed. A status summary for the entire instance is shown at the top of the page, and the detailed status of each database is shown in the central table.

### Top summary information

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is used |
| Number of databases | Total number of databases under the instance |
| Active Session Count | Sum of active sessions across all databases (bar chart) |
| Total DB Size | Total size of all databases |
| WAL Size | Write-Ahead Log size |

### Table items

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Alias</td><td><ul><li>Database name</li><li>Moves to the detailed information page when clicked</li></ul></td></tr><tr><td>Creation date</td><td>Database creation date and time</td></tr><tr><td>Owner</td><td>User who owns the database</td></tr><tr><td>Encoding</td><td>Character Set configured for the database</td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td></tr><tr><td>Data Size</td><td>Capacity occupied by the actual data (GB)</td></tr><tr><td>Number of active sessions</td><td>Number of currently active sessions (bar chart)</td></tr><tr><td>Tuple Health</td><td>A status indicator that combines the Dead Tuple ratio and Vacuum execution time</td></tr><tr><td>Bloat Ratio</td><td>Ratio of Dead Tuples relative to the total size (%)</td></tr><tr><td>Live/Dead Tuple Rate</td><td>Ratio of Dead Tuples relative to Live Tuples (%)</td></tr><tr><td>Last Vacuum execution time</td><td>Date and time when the last Vacuum was performed</td></tr></tbody></table>

**Tuple Health** The status is determined based on the Bloat Ratio and the last Vacuum execution time.

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

`postgres`, `template0`, `template1`is the system default database; it is shown in the list but cannot be selected or deleted.
{% endhint %}

1. **Management > Data Space Management** Click the menu.
2. Check the status in the top summary information and the database table.
3. To find a specific database, enter its alias in the search box.

## Create database

**Create** Clicking the button opens the database creation drawer on the right side of the screen. After entering the required items, **Create** clicking the button sends the creation request and closes the drawer. The result of the creation request and whether it has completed are confirmed via the toast message displayed at the top of the screen.

{% hint style="warning" %}
**Caution**

**Cancel**Clicking it or closing the drawer resets all the entered content. **Create** Changes are not saved until the button is clicked.
{% endhint %}

### Input items

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Database Name *</td><td>Name of the database to create</td><td><ul><li>Only lowercase English letters (a-z), numbers (0-9), and underscores (<code>_</code>) can be used, within 30 characters</li><li>Cannot be duplicated within the same instance</li></ul></td></tr><tr><td>Owner *</td><td>User who owns the database</td><td><code>postgres</code> (fixed value)</td></tr><tr><td>Encoding</td><td>Database Character Set</td><td>Default value: <code>UTF8</code></td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td><td><ul><li>Default value: Unlimited</li><li>Can be entered directly when Unlimited is unchecked (an integer of 0 or more)</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

**Unlimited** When checked, within the instance's `max_connections` connections are made without limit within the range. To limit the number of connections, uncheck the checkbox and enter the desired value.

1. **Management > Data Space Management** On the page, **Create** Click the button.
2. In the database creation drawer, **Database Name**and the required items.
3. **Create** Click the button.
4. When the drawer closes, check the result of the creation request and whether it has completed via the toast message.

## Viewing database detailed information

Clicking a database alias in the list moves you to the detailed information page for that database. The page consists of three areas: basic information (Info), Database Activity, and Trend Metrics.

Clicking the 🔃 icon manually refreshes the page data. Clicking the parent menu name in the breadcrumb at the top of the page returns you to the Data Space Management list.

1. **Management > Data Space Management** In the database list on the page, select the database you want to view. **Alias**Click it.
2. Check the status and current state in the basic information, Database Activity, and Trend Metrics areas.

### Displayed items

**Basic information (Info)**

| Item | Description |
| --- | --- |
| Creation date | Database creation date and time |
| Tuple Health | A status indicator based on the Dead Tuple ratio and Vacuum execution time (Healthy / Watch / Critical) |
| Last Vacuum execution time | Date and time when the last Vacuum was performed |
| Owner | User who owns the database |
| Encoding | Character Set configured for the database |
| Connection Limit | Maximum number of concurrent connections allowed |

**Database Activity**

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Size</td><td>Capacity occupied by the actual data (GB)</td></tr><tr><td>Number of active sessions</td><td><ul><li>Number of currently active sessions (line chart)</li><li>Display individual session lines per node in HA configuration</li></ul></td></tr></tbody></table>

**Trend Metrics**

| Item | Description |
| --- | --- |
| Bloat Ratio (%) | The ratio of Dead Tuples relative to the total size |
| Live Tuple Count (CNT) | Number of Live Tuples in the database |
| Dead Tuple Count (CNT) | Number of Dead Tuples in the database |
| Live/Dead Tuple Rate (%) | Ratio of Dead Tuples to Live Tuples |

## Delete Database

A database can be deleted from two locations: the list page and the detail page. Clicking the delete button opens a confirmation modal, and deletion proceeds after final confirmation in the modal.

{% hint style="warning" %}
**Caution**

The deletion operation cannot be undone. All data and objects contained in the database are permanently deleted, and connected applications may experience an immediate outage.
{% endhint %}

1. **Management > Data Space Management** Navigate to the database you want to delete on the page. **[List Page]** Select the database to delete using the radio button. **[Detail Page]** Of the database to delete, **Alias**Click to move to the detail page.
2. **Delete** Click the button.
3. Verify the deletion target and scope of impact in the deletion confirmation modal.
4. **Delete** Click the button.
5. Check the deletion result via the toast message.
