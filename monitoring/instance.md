On the instance monitoring page, you can check the key performance metrics of the database instance in real-time line charts.

Core metrics such as CPU utilization, memory utilization, and the number of active sessions are always displayed at the top of the screen. Select the Quick Insight, Load, I/O, Writes, or All tab to focus on situation-specific metrics, and when you switch between Tibero and OpenSQL using the DB Type toggle in the GNB, the metric configuration suited to that engine is applied.

{% hint style="info" %}
**Note**

The monitored resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## View Instance Monitoring <a href="#view-instance-monitoring" id="view-instance-monitoring"></a>

The instance monitoring page begins with selecting the DB Type and the resource to view at the top of the GNB. The selectable resource hierarchy and the displayed metrics vary depending on the DB Type.

1. In the left menu **Monitoring > Instance Monitoring**Click.
2. In the DB Type toggle of the GNB **Tibero** or **OpenSQL**is selected.
3. Select the Service and Instance to view in the DB Select tree. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** As needed **Instance View** or **Database View** Click the button.
5. Tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) by clicking it to view the desired metrics.
6. Click the ⚙️ icon to configure auto-refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection <a href="#db-type" id="db-type"></a>

In the DB Type toggle of the GNB **Tibero** or **OpenSQL**When you select it, the DB Select tree and metrics suited to that DB Type are displayed. When you switch the DB Type, DB Select is reset to the full selection.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree has a **Service → Instance** two-level structure. You can make selections at the All, Primary DB, and Standby DB levels.

- The Instance Alias is displayed up to a maximum of 30 characters, and if it exceeds this, it is abbreviated with an ellipsis (…).
- Sort order: Service creation date, newest first → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending alias order within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree has a **Service → Instance → Database** It has a three-level structure. You can make selections down to the Database level, and when you select a Database, its parent Instance is automatically included.

- If you select only some Databases, the Instance View also aggregates metrics based only on the selected Databases.
- The Instance Alias and Database name are each displayed up to a maximum of 10 characters, and if they exceed this, they are abbreviated with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Pinned Top Metrics <a href="#undefined-1" id="undefined-1"></a>

These are the key metrics that are always displayed at the top of the page regardless of DB Type. The overall instance Health status is `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`displayed together as well.

| Metric name | Description |
| --- | --- |
| CPU Usage (%) | CPU Usage |
| Memory Usage (%) | Memory Usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag between Primary and Standby (displayed only when a Standby exists) |

### Viewing Metrics by Tab <a href="#undefined-2" id="undefined-2"></a>

The default tab when entering the page is **Quick Insight**. When you switch tabs, only the metrics relevant to that situation are queried.

| Tab | Purpose of review |
| --- | --- |
| Quick Insight | Quickly identify overall anomaly signs |
| Load | Check session/query load and the cause of CPU/Memory increases |
| I/O | Check disk read/write activity levels and the cause of response delays |
| Writes | Check bulk write operations and surges in transaction rollbacks |
| All | Query all metrics at once to diagnose compound causes |

You can pin frequently used tabs. When you hover over a tab, a pin icon appears, and clicking it sets that tab as the default entry tab. You can pin only one at most, and pinning another tab automatically unpins the existing one.

The metrics displayed per tab vary depending on the DB engine.

{% tabs %}
{% tab title="Tibero" %}
**Quick Insight**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Redo Log Rate (MB/s) | Amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | Number of transactions rolled back within a unit of time |

**Load**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of data reads from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Hard Parse Count (CNT) | Number of SQL executions that went through the entire execution process without using the cache |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data for which disk read requests were processed per unit of time |
| Disk Write Rate (MB/s) | Amount of data for which disk write requests were processed per unit of time |
| Multi Block Disk Read (block/s) | Number of multi-block disk read blocks within a unit of time (per second) |
| Physical Write (block/s) | Number of DBWR written blocks within a unit of time (per second) |

**Writes**

| Metric name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Redo Entries (CNT) | Number of Redo records written to the log buffer |
| Redo Log Rate (MB/s) | Amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% tab title="OpenSQL" %}
**Quick Insight**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | Number of pages read directly from disk |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |

**Load**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of data reads from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Transactions (CNT) | Number of transactions completed within a unit of time |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data for which disk read requests were processed per unit of time |
| Disk Write Rate (MB/s) | Amount of data for which disk write requests were processed per unit of time |
| Block Read Latency (ms/block) | Average time taken to read one data block |
| Block Write Latency (ms/block) | Average time taken to write one data block |

**Writes**

| Metric name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Transactions (CNT) | Number of transactions completed within a unit of time |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% endtabs %}

### OpenSQL View Switching <a href="#opensql-view" id="opensql-view"></a>

When OpenSQL is selected, a **Instance View**and **Database View** switch button is additionally displayed on the screen.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays the metrics of the selected Database at the Instance level by summing or averaging them</li><li>The DB Select tree is displayed in two levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to three levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when you switch to Database View, metrics provided only at the Instance level are displayed on an Instance basis as is.
{% endhint %}
