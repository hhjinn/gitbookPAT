Under **My Page > Usage Management**, you can view and manage the current month's usage status and past monthly usage history for databases (DB Service licenses), excluding infrastructure resources.

{% hint style="warning" %}
**Caution**

The charges provided in Usage Management are amounts rounded to two decimal places, so they may differ from the amount actually billed by the CSP.
{% endhint %}

# Usage Status <a href="#usage-status" id="usage-status"></a>

You can view this month's database usage status based on the DB service license. Data is automatically updated daily at 00:00 UTC.

1. **Go to the OwlDB console screen > My Page > Usage Management > Usage Status** menu.
2. Check the summary information at the top (usage period, number of DB services, total usage charge).
3. Check the detailed usage list in the table at the bottom. It supports a DB type filter (Tibero / OpenSQL) and name search.

## Summary Information Items <a href="#summary-items" id="summary-items"></a>

<table><thead><tr><th>o4uJJV00Lfkj</th><th>2iz0hWZN3m2J</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>Usage Period</td><td><ul><li>Displays the period from the 1st of the current month to the day before the query date in UTC</li><li>Notation: <code>YYYY.MM.DD \~ YYYY.MM.DD</code></li></ul></td></tr><tr><td>Number of DB Services</td><td><ul><li>Total number of DB services used within the query period</li><li>Also displays the count by DB type (Tibero / OpenSQL)</li></ul></td></tr><tr><td>Total Usage Charge</td><td><ul><li>Sum of DB service license charges used within the query period</li><li>Based on US dollars ($), rounded to two decimal places</li></ul></td></tr></tbody></table>

## Usage Status List <a href="#usage-status-list" id="usage-status-list"></a>

<table><thead><tr><th>4jOOAOxg2Wz3</th><th>ZoAoHSV3bCZe</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>DB Type</td><td>DB service type (<code>Tibero</code> / <code>OpenSQL</code>)</td></tr><tr><td>Name</td><td>Name of the DB service used within the query period<ul><li>Deleted services are marked with <code>(Terminated)</code> before the name</li><li>The entire row is displayed in red</li></ul></td></tr><tr><td>Instance alias</td><td>Alias of the instance belonging to the corresponding DB service</td></tr><tr><td>License Option</td><td>License application type (<code>LI</code> / <code>BYOL</code>)</td></tr><tr><td>Usage Time</td><td>Cumulative usage time within the query period (in <code>nh nm</code> format)<ul><li>Seconds are rounded up to minutes and then converted to hours/minutes (e.g., 45 seconds → <code>1m</code>, 1 hour 24 minutes 1 second → <code>1h 25m</code>)</li></ul></td></tr><tr><td>Usage Charge</td><td>License usage charge incurred within the query period (in $)<ul><li>BYOL databases are also calculated with the designated license cost according to the BYOL billing policy</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- Databases in a deleted or stopped state are also included in the total count and usage list if there is metering (usage) history within the current month.
- For details on the license charge calculation criteria, such as metering units and whether billing applies by status, refer to the License Billing Criteria.
{% endhint %}

---

# Usage History <a href="#usage-history" id="usage-history"></a>

You can view the monthly database usage history report and download it as a file. The final usage history report for the current month is automatically generated at 00:00 UTC on the 1st of the following month and is retained for up to 5 years.

## How to View and Download <a href="#view-and-download" id="view-and-download"></a>

1. **Go to the OwlDB console screen > My Page > Usage Management > Usage History** menu.
2. Use the calendar picker (query period selector) to set the desired range.
3. Check the monthly report list.
4. Select the report to download from the checkbox on the left of the list. **When selecting a single report:** download as a **PDF** file **When selecting multiple reports:** download as a **ZIP** compressed file
5. Click the **[Download]** button.
6. When you click the **name** of a monthly usage history report, a Drawer screen appears on the right where you can check the detailed usage history.

## Usage History List <a href="#usage-history-list" id="usage-history-list"></a>

<table><thead><tr><th>5xlZK5islr4K</th><th>x6LZcmOpZodX</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Name</strong></td><td>Monthly usage report name (format: <code>OwlDB Monthly Report YYYYMM</code>)</td></tr><tr><td><strong>Usage Month</strong></td><td>Report target month (format: <code>MM YYYY</code>)</td></tr><tr><td><strong>Total Usage Charge</strong></td><td>Total license usage charge incurred in the corresponding month (rounded to two decimal places)<ul><li>Months with no usage charge are marked as <code>$0.00</code></li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even if no DB services were used at all in a specific month or there are no registered DB services, a monthly report is automatically generated every month. In this case, the total usage charge is displayed as `$0.00`, and when entering the detailed history, the message *"There is no usage history available."* is displayed.
{% endhint %}

## Usage History Details <a href="#usage-history-details" id="usage-history-details"></a>

When you click a report name in the usage history list, you can view the detailed information of that report.

The items that can be viewed are the same as in Usage Status.

- You can download the usage history as a PDF file by clicking the **[Download]** button in the upper right.
