**My Page > Usage Management**allows you to check and manage the current month's usage status and past monthly usage history of databases (DB service licenses), excluding infrastructure resources.

{% hint style="warning" %}
**Caution**

The charges provided in Usage Management are amounts rounded to two decimal places, so they may differ from the amounts actually billed by the CSP.
{% endhint %}

# Usage Status

You can view the current month's database usage status based on DB service licenses. Data is automatically updated daily at 00:00 UTC.

1. **OwlDB console screen > My Page > Usage Management > Usage Status** Navigate to the menu.
2. Check the summary information at the top (usage period, number of DB services, total usage charges).
3. Check the detailed usage list in the table at the bottom. DB type filter (Tibero / OpenSQL) and name search are supported.

## Summary Information Items

<table data-full-width="true"><thead><tr><th>o4uJJV00Lfkj</th><th>2iz0hWZN3m2J</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>Usage Period</td><td><ul><li>Displays the period from the 1st of the current month to the day before the query date (UTC)</li><li>Notation:<code>YYYY.MM.DD \~ YYYY.MM.DD</code></li></ul></td></tr><tr><td>Number of DB Services</td><td><ul><li>Total number of DB services used within the query period</li><li>Also displays the number by DB type (Tibero / OpenSQL)</li></ul></td></tr><tr><td>Total Usage Charges</td><td><ul><li>Sum of DB service license charges used within the query period</li><li>Based on dollars ($), rounded to two decimal places</li></ul></td></tr></tbody></table>

## Usage Status List

<table data-full-width="true"><thead><tr><th>4jOOAOxg2Wz3</th><th>ZoAoHSV3bCZe</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td>DB Type</td><td>DB service type (<code>Tibero</code> / <code>OpenSQL</code>)</td></tr><tr><td>Name</td><td>Name of the DB service used within the query period<ul><li>Deleted services are marked before the name<code>(Terminated)</code>Notation</li><li>Entire row displayed in red</li></ul></td></tr><tr><td>Instance Alias</td><td>Instance alias belonging to the DB service</td></tr><tr><td>License Option</td><td>License application type (<code>LI</code> / <code>BYOL</code>)</td></tr><tr><td>Usage Time</td><td>Cumulative usage time within the query period (<code>nh nm</code>format)<ul><li>Seconds are rounded up to minutes and then converted to hours/minutes (e.g., 45 seconds →<code>1m</code>, 1 hour 24 minutes 1 second → <code>1h 25m</code>)</li></ul></td></tr><tr><td>Usage Charges</td><td>License usage charges incurred within the query period (in $)<ul><li>BYOL databases are also calculated at the license cost specified according to the BYOL billing policy</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- Databases in a deleted or stopped state are also included in the total count and usage list if there is metering (usage) history within the current month.
- For details on license charge calculation criteria such as metering units and billing by status, refer to the License Billing Criteria.
{% endhint %}

---

# Usage History

You can check the monthly database usage history report and download it as a file. The final usage history report for the current month is automatically generated at 00:00 on the 1st of the following month (UTC) and is retained for up to 5 years.

## Query and Download Method

1. **OwlDB console screen > My Page > Usage Management > Usage History** Navigate to the menu.
2. Use the calendar picker (query period selector) to set the desired range.
3. Check the monthly report list.
4. Select the report to download from the checkbox on the left of the list. **When selecting a single report:** **PDF** Downloaded as a file **When selecting multiple reports:** **ZIP** Downloaded as a compressed file
5. **[Download]** Click the button.
6. Of the monthly usage history report **name**When you click it, a Drawer screen appears on the right where you can check detailed usage history.

## Usage History List

<table data-full-width="true"><thead><tr><th>5xlZK5islr4K</th><th>x6LZcmOpZodX</th></tr></thead><tbody><tr><td><strong>Item</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Name</strong></td><td>Monthly usage report name (format: <code>OwlDB Monthly Report YYYYMM</code>)</td></tr><tr><td><strong>Usage Month</strong></td><td>Report target month (format: <code>YYYY년 MM월</code>)</td></tr><tr><td><strong>Total Usage Fee</strong></td><td>Total license usage fee incurred during the month (rounded to the second decimal place)<ul><li>For months with no usage fee,<code>$0.00</code> displayed</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even if the DB service was not used at all during a specific month or there is no registered DB service, the monthly report is automatically generated every month. In this case, the total usage fee is displayed as `$0.00`and when entering the details, *"There is no usage history available for review."* this notice is displayed.
{% endhint %}

## Usage History Details

Clicking a report name in the usage history list allows you to view the detailed information of that report.

The items available for review are the same as in the usage status.

- In the upper right corner, **[Download]** click the button to download the usage history as a PDF file.
