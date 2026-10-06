**Management > Data Space Management** In this menu, you can query and manage databases belonging to an OpenSQL instance. From the database list, you can see each database's size, active session count, and Tuple Health status at a glance, and you can create and delete databases. Clicking a database alias lets you view detailed information such as Encoding, Connection Limit, and Bloat Ratio, along with trend indicators.

{% hint style="info" %}
**Note**

- OpenSQL database management is available only in Azure environments. It is not supported in AWS environments.
- When the DB engine is set to Tibero, the tablespace and data file management screen is displayed instead of this page.
{% endhint %}

<figure>
<img src="../../.gitbook/assets/image-7c6a12e3.png" alt="">
<figcaption>Figure 1. Data Space - OpenSQL</figcaption>
</figure>

## Querying the Database List

**Management > Data Space Management** When you enter the menu, the list of databases belonging to the current instance is displayed. A status summary for the entire instance is shown at the top of the page, and detailed status for each database can be checked in the central table.

![Data Space Management list page](The Data Space Management screen showing the top summary information together with the database table)

### Top Summary Information

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is enabled |
| Number of Databases | Total number of databases under the instance |
| Active Session Count | Sum of active sessions across all databases (bar chart) |
| Total DB Size | Sum of the sizes of all databases |
| WAL Size | Write-Ahead Log size |

### Table Items

<table data-full-width="true"><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Alias</td><td><ul><li>Database name</li><li>Clicking navigates to the detail information page</li></ul></td></tr><tr><td>Creation Date</td><td>Date and time the database was created</td></tr><tr><td>Owner</td><td>User who owns the database</td></tr><tr><td>Encoding</td><td>Character Set configured for the database</td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td></tr><tr><td>Data Size</td><td>Capacity actually occupied by data (GB)</td></tr><tr><td>Active Session Count</td><td>Number of currently active sessions (bar chart)</td></tr><tr><td>Tuple Health</td><td>A status indicator combining the Dead Tuple ratio and Vacuum execution time</td></tr><tr><td>Bloat Ratio</td><td>The proportion of Dead Tuple relative to total size (%)</td></tr><tr><td>Live/Dead Tuple Rate</td><td>Ratio of Dead Tuple to Live Tuple (%)</td></tr><tr><td>Last Vacuum Execution Time</td><td>Date and time the last Vacuum was executed</td></tr></tbody></table>

**Tuple Health** The status is determined based on the Bloat Ratio and the last Vacuum execution time.

| bIIG5p6cXo7A | Vacuum Normal (under 24 hours) | Vacuum Warning (24–72 hours) | Vacuum Danger (72 hours or more) |
| --- | --- | --- | --- |
| Bloat Ratio Normal (under 20%) | Healthy | Watch | Critical |
| Bloat Ratio Warning (20–40%) | Watch | Watch | Critical |
| Bloat Ratio Danger (40% or more) | Critical | Critical | Critical |

{% hint style="info" %}
**Note**

Tuple Health is a guide based on estimated statistics, and the actual performance impact may differ depending on traffic patterns.
{% endhint %}

You can find a specific database by entering its alias in the search box. Clicking a column header sorts in ascending/descending order, and the default sort criterion is alias ascending. Clicking the 🔃 icon manually refreshes the page data.

{% hint style="info" %}
**Note**

`postgres`, `template0`, `template1`are system default databases; they appear in the list but cannot be selected or deleted.
{% endhint %}

1. **Management > Data Space Management** Click the menu.
2. Check the status in the top summary information and the database table.
3. To find a specific database, enter its alias in the search box.

## Creating a Database

Clicking the [Create] button opens the database creation drawer on the right side of the screen. After entering the required fields, clicking the [Create] button sends the creation request and closes the drawer. The result of the creation request and whether it completed are confirmed via a toast message displayed at the top of the screen.

{% hint style="warning" %}
**Caution**

Clicking [Cancel] or closing the drawer resets all entered content. Changes are not saved until you click the [Create] button.
{% endhint %}

### Input Items

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td>Database Name *</td><td>The name of the database to create</td><td><ul><li>Up to 30 characters, using only lowercase English letters (a-z), numbers (0-9), and underscores (<code>_</code>) are allowed</li><li>Duplicates are not allowed within the same instance</li></ul></td></tr><tr><td>Owner *</td><td>The user who owns the database</td><td><code>postgres</code> (fixed value)</td></tr><tr><td>Encoding</td><td>Database Character Set</td><td>Default value: <code>UTF8</code></td></tr><tr><td>Connection Limit</td><td>The maximum number of connections that can access concurrently</td><td><ul><li>Default value: Unlimited</li><li>Can be entered directly when Unlimited is unchecked (integer of 0 or greater)</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

**Unlimited** When checked, within the instance's `max_connections` range, connections are unlimited. To limit the number of connections, uncheck the checkbox and enter the desired value.

1. **Management > Data Space Management** Click the [Create] button on the page.
2. In the database creation drawer, **Database Name**and enter the required items.
3. Click the [Create] button.
4. When the drawer closes, check the creation request result and completion status via the toast message.

## Viewing Database Details

Clicking a database alias in the list navigates to the detail page for that database. The page consists of three areas: basic information (Info), Database Activity, and Trend Metrics.

Clicking the 🔃 icon manually refreshes the page data. Clicking the parent menu name in the breadcrumb at the top of the page returns you to the Data Space Management list.

1. **Management > Data Space Management** From the database list on the page, for the database you want to view, its **alias**Click it.
2. Check the status and current state in the basic information, Database Activity, and Trend Metrics areas.

### Displayed Items

**Basic Information (Info)**

| Item | Description |
| --- | --- |
| Creation Date | The date and time the database was created |
| Tuple Health | A status indicator based on the Dead Tuple ratio and Vacuum execution time (Healthy / Watch / Critical) |
| Last Vacuum Execution Time | The date and time the last Vacuum was executed |
| Owner | The user who owns the database |
| Encoding | The Character Set configured for the database |
| Connection Limit | The maximum number of connections that can access concurrently |

**Database Activity**

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Size</td><td>The capacity occupied by actual data (GB)</td></tr><tr><td>Active Session Count</td><td><ul><li>The number of currently active sessions (line chart)</li><li>In an HA configuration, sessions are displayed as individual lines per node</li></ul></td></tr></tbody></table>

**Trend Metrics**

| Item | Description |
| --- | --- |
| Bloat Ratio (%) | The ratio of Dead Tuples relative to the total size |
| Live Tuple Count (CNT) | The number of Live Tuples in the database |
| Dead Tuple Count (CNT) | The number of Dead Tuples in the database |
| Live/Dead Tuple Rate (%) | The ratio of Dead Tuples relative to Live Tuples |

## Deleting a Database

A database can be deleted from two locations: the list page and the detail page. Clicking the delete button displays a confirmation modal, and deletion proceeds after final confirmation in the modal.

{% hint style="warning" %}
**Caution**

The deletion operation cannot be undone. All data and objects contained in the database are permanently deleted, and connected applications may experience an immediate failure.
{% endhint %}

1. **Management > Data Space Management** Enter the database you want to delete on the page. **[List Page]** Select the database to delete using the radio button. **[Details Page]** Click the **alias**of the database to delete to navigate to the details page.
2. Click the [Delete] button.
3. In the deletion confirmation modal, verify the deletion target and scope of impact.
4. Click the [Delete] button.
5. Verify the deletion result via the toast message.
