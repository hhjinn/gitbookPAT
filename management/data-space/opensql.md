**Management > Data Space Management** In this menu, you can view and manage databases belonging to an OpenSQL instance. From the database list, you can see each database's size, number of active sessions, and Tuple Health status at a glance, and you can create and delete new databases. Clicking a database alias lets you view detailed information such as Encoding, Connection Limit, and Bloat Ratio, along with trend indicators.

{% hint style="info" %}
**Note**

- OpenSQL database management is available only in Azure environments. It is not supported in AWS environments.
- If the DB engine is set to Tibero, the tablespace and data file management screen is displayed instead of this page.
{% endhint %}

<figure>
<img src="../../.gitbook/assets/image-7c6a12e3.png" alt="">
<figcaption>Figure 1. Data Space - OpenSQL</figcaption>
</figure>

## Viewing the Database List <a href="#database-list" id="database-list"></a>

**Management > Data Space Management** When you enter the menu, the list of databases belonging to the current instance is displayed. A status summary for the entire instance is shown at the top of the page, and the detailed status of each database is shown in the central table.

![Data Space Management List Page](Data space management screen showing both the top summary information and the database table)

### Top Summary Information <a href="#undefined" id="undefined"></a>

| Item | Description |
| --- | --- |
| Auto Vacuum | Auto Vacuum Status |
| Number of Databases | Total number of databases under the instance |
| Active Session Count | Total number of active sessions across all databases (bar chart) |
| Total DB Size | Total size of all databases |
| WAL Size | Write-Ahead Log Size |

### Table items <a href="#undefined-1" id="undefined-1"></a>

<table><thead><tr><th>Column</th><th>Description</th></tr></thead><tbody><tr><td>Alias</td><td><ul><li>Database Name</li><li>Navigates to the detailed information page when clicked</li></ul></td></tr><tr><td>Creation date</td><td>Database Creation Date and Time</td></tr><tr><td>Owner</td><td>Database Owner User</td></tr><tr><td>Encoding</td><td>Character Set configured for the database</td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td></tr><tr><td>Data Size</td><td>Actual capacity occupied by data (GB)</td></tr><tr><td>Number of active sessions</td><td>Number of currently active sessions (bar chart)</td></tr><tr><td>Tuple Health</td><td>Status indicator combining Dead Tuple ratio and Vacuum execution time</td></tr><tr><td>Bloat Ratio</td><td>Ratio of Dead Tuples relative to total size (%)</td></tr><tr><td>Live/Dead Tuple Rate</td><td>Ratio of Dead Tuples relative to Live Tuples (%)</td></tr><tr><td>Last Vacuum execution time</td><td>Date and time when the last Vacuum was executed</td></tr></tbody></table>

**Tuple Health** The status is determined based on the Bloat Ratio and the last Vacuum execution time.

| bIIG5p6cXo7A | Vacuum Normal (less than 24 hours) | Vacuum Warning (24–72 hours) | Vacuum Critical (72 hours or more) |
| --- | --- | --- | --- |
| Bloat Ratio Normal (less than 20%) | Healthy | Watch | Critical |
| Bloat Ratio Warning (20–40%) | Watch | Watch | Critical |
| Bloat Ratio Critical (40% or more) | Critical | Critical | Critical |

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
2. Check the current status in the top summary information and the database table.
3. To find a specific database, enter its alias in the search box.

## Create a database <a href="#create-database" id="create-database"></a>

Clicking the [Create] button opens the database creation drawer on the right side of the screen. After entering the required items and clicking the [Create] button, the creation request is sent and the drawer closes. Check the result of the creation request and its completion status via the toast message displayed at the top of the screen.

{% hint style="warning" %}
**Caution**

Clicking [Cancel] or closing the drawer resets all entered content. Changes are not saved until you click the [Create] button.
{% endhint %}

### Input items <a href="#undefined-2" id="undefined-2"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>Database Name *</td><td>Name of the database to create</td><td><ul><li>Only lowercase English letters (a-z), numbers (0-9), and underscores (<code>_</code>) up to 30 characters can be used</li><li>Duplicates not allowed within the same instance</li></ul></td></tr><tr><td>Owner *</td><td>Database Owner User</td><td><code>postgres</code> (fixed value)</td></tr><tr><td>Encoding</td><td>Database Character Set</td><td>Default value: <code>UTF8</code></td></tr><tr><td>Connection Limit</td><td>Maximum number of concurrent connections allowed</td><td><ul><li>Default value: Unlimited</li><li>Can be entered directly when Unlimited is unchecked (integer of 0 or greater)</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

**Unlimited** When checked, the instance's `max_connections` connects without limit within the range. To limit the number of connections, uncheck the checkbox and enter the desired value.

1. **Management > Data Space Management** Click the [Create] button on the page.
2. In the database creation drawer, **Database Name**and enter the required items.
3. Click the [Create] button.
4. Once the drawer closes, check the result of the creation request and its completion status via the toast message.

## View database details <a href="#database-details" id="database-details"></a>

Clicking a database alias in the list navigates to the detail page for that database. The page consists of three areas: basic information (Info), Database Activity, and Trend Metrics.

Clicking the 🔃 icon manually refreshes the page data. Clicking the parent menu name in the breadcrumb at the top of the page returns you to the data space management list.

1. **Management > Data Space Management** From the database list on the page, the **Alias**Click.
2. Check the status and current state in the basic information, Database Activity, and Trend Metrics areas.

### Displayed items <a href="#undefined-3" id="undefined-3"></a>

**Basic information (Info)**

| Item | Description |
| --- | --- |
| Creation date | Database Creation Date and Time |
| Tuple Health | Status indicator based on Dead Tuple ratio and Vacuum execution time (Healthy / Watch / Critical) |
| Last Vacuum execution time | Date and time when the last Vacuum was executed |
| Owner | Database Owner User |
| Encoding | Character Set configured for the database |
| Connection Limit | Maximum number of concurrent connections allowed |

**Database Activity**

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Size</td><td>Actual capacity occupied by data (GB)</td></tr><tr><td>Number of active sessions</td><td><ul><li>Number of currently active sessions (line chart)</li><li>Individual session lines displayed per node in an HA configuration</li></ul></td></tr></tbody></table>

**Trend Metrics**

| Item | Description |
| --- | --- |
| Bloat Ratio (%) | Ratio of Dead Tuples relative to total size |
| Live Tuple Count (CNT) | Number of Live Tuples in the database |
| Dead Tuple Count (CNT) | Number of Dead Tuples in the database |
| Live/Dead Tuple Rate (%) | Ratio of Dead Tuples relative to Live Tuples |

## Delete a database <a href="#delete-database" id="delete-database"></a>

A database can be deleted from two places: the list page and the detail page. Clicking the delete button brings up a confirmation modal, and deletion proceeds after final confirmation in the modal.

{% hint style="warning" %}
**Caution**

The delete operation cannot be undone. All data and objects contained in the database are permanently deleted, and connected applications may experience immediate failures.
{% endhint %}

1. **Management > Data Space Management** Navigate to the database to be deleted on the page. **[List page]** Select the database to delete using the radio button. **[Detail page]** The **Alias**to navigate to the detail page.
2. Click the [Delete] button.
3. Check the deletion target and scope of impact in the deletion confirmation modal.
4. Click the [Delete] button.
5. Check the deletion result via the toast message.
