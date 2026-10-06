**My Page > Usage Management**allows you to view and manage the current month's usage status and past monthly usage history of databases (DB service licenses), excluding infrastructure resources.

{% hint style="warning" %}
**Caution**

The fees provided in Usage Management are amounts rounded to two decimal places, so they may differ from the amount actually billed by the CSP.
{% endhint %}

# Usage Status <a href="#usage-status" id="usage-status"></a>

You can view this month's database usage status based on DB service licenses. Data is automatically updated daily at 00:00 UTC.

1. **OwlDB Console Screen > My Page > Usage Management > Usage Status** Navigate to the menu.
2. Check the summary information at the top (usage period, number of DB services, total usage fee).
3. Check the detailed usage list in the table at the bottom. DB type filter (Tibero / OpenSQL) and name search are supported.

## Summary Information Items <a href="#summary-items" id="summary-items"></a>

<table><thead><tr><th>o4uJJV00Lfkj</th><th>2iz0hWZN3m2J</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>Usage Period</td><td><ul><li>Displays the period from the 1st of the current month to the day before the query date (UTC)</li><li>Notation:<code>YYYY.MM.DD \~ YYYY.MM.DD</code></li></ul></td></tr><tr><td>Number of DB Services</td><td><ul><li>Total number of DB services used within the query period</li><li>Also displays the count by DB type (Tibero / OpenSQL)</li></ul></td></tr><tr><td>Total Usage Fee</td><td><ul><li>Sum of DB service license fees used within the query period</li><li>Based on dollars ($), rounded to the second decimal place</li></ul></td></tr></tbody></table>

## Usage Status List <a href="#usage-status-list" id="usage-status-list"></a>

<table><thead><tr><th>4jOOAOxg2Wz3</th><th>ZoAoHSV3bCZe</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>DB Type</td><td>DB service type (<code>Tibero</code> / <code>OpenSQL</code>)</td></tr><tr><td>Name</td><td>Name of the DB service used within the query period<ul><li>Deleted services are marked before the name with<code>(Terminated)</code>Notation</li><li>Entire row displayed in red</li></ul></td></tr><tr><td>Instance alias</td><td>Instance alias belonging to the corresponding DB service</td></tr><tr><td>License Option</td><td>License application type (<code>LI</code> / <code>BYOL</code>)</td></tr><tr><td>Usage Time</td><td>Cumulative usage time within the query period (<code>nh nm</code>format)<ul><li>Seconds are rounded up to minutes and then converted to hours/minutes (e.g., 45 seconds →<code>1m</code>, 1 hour 24 minutes 1 second → <code>1h 25m</code>)</li></ul></td></tr><tr><td>Usage Fee</td><td>License usage fee incurred within the query period (in $)<ul><li>BYOL databases are also calculated with the specified license cost according to the BYOL billing policy</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- Databases in a deleted or stopped state are also included in the total count and usage list if metering (usage) records exist within the current month.
- For details on license fee calculation criteria, such as metering units and whether billing applies by state, refer to the License Billing Criteria.
{% endhint %}

---

# Usage History <a href="#usage-history" id="usage-history"></a>

You can view monthly database usage history reports and download them as files. The final usage history report for the current month is automatically generated at 00:00 UTC on the 1st of the following month and is retained for up to 5 years.

## Viewing and Downloading Method <a href="#view-and-download" id="view-and-download"></a>

1. **OwlDB Console Screen > My Page > Usage Management > Usage History** Navigate to the menu.
2. Use the calendar picker (query period selector) to set the desired range.
3. Check the list of monthly reports.
4. Select the report to download from the checkbox on the left side of the list. **When selecting a single report:** **PDF** Download as a file **When selecting multiple reports:** **ZIP** Download as a compressed file
5. **[Download]** Click the button.
6. Of the monthly usage history report **Name**Clicking it displays a Drawer screen on the right where you can check detailed usage history.

## Usage History List <a href="#usage-history-list" id="usage-history-list"></a>

<table><thead><tr><th>5xlZK5islr4K</th><th>x6LZcmOpZodX</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Name</strong></td><td>Monthly usage report name (format: <code>OwlDB Monthly Report YYYYMM</code>)</td></tr><tr><td><strong>Usage Month</strong></td><td>Target month of the report (format: <code>YYYYYear MMMonth</code>)</td></tr><tr><td><strong>Total Usage Fee</strong></td><td>Total license usage fee incurred in the corresponding month (rounded to the second decimal place)<ul><li>Months with no usage fee are<code>$0.00</code> Notation</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even if no DB service was used at all in a specific month or there are no registered DB services, a monthly report is automatically generated every month. In this case, the total usage fee is displayed as `$0.00`, and when entering the detailed history, *"No usage history available to view."* is displayed as a notice.
{% endhint %}

## Usage History Details <a href="#usage-history-details" id="usage-history-details"></a>

Clicking a report name in the usage history list allows you to view the detailed information of that report.

The items you can view are the same as in Usage Status.

- Top right **[Download]** Click the button to download the usage history as a PDF file.
