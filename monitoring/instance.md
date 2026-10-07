On the Instance Monitoring page, you can view key performance metrics of the database instance in real-time line charts.

Core metrics such as CPU usage, memory usage, and the number of active sessions are always displayed at the top of the screen. You can select the Quick Insight, Load, I/O, Writes, or All tabs to focus on situation-specific metrics, and switching between Tibero and OpenSQL using the DB Type toggle in the GNB applies a metric configuration suited to the respective engine.

{% hint style="info" %}
**Note**

The monitored resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Viewing Instance Monitoring <a href="#view-instance-monitoring" id="view-instance-monitoring"></a>

The Instance Monitoring page begins with selecting the DB Type and the resource to view at the top of the GNB. The selectable resource hierarchy and the displayed metrics vary depending on the DB Type.

1. Click **Monitoring > Instance Monitoring** in the left menu.
2. Select **Tibero** or **OpenSQL** from the DB Type toggle in the GNB.
3. Select the Service and Instance to view in the DB Select tree. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** Click the **Instance View** or **Database View** button as needed.
5. Click a tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to view the desired metrics.
6. Click the ⚙️ icon to set up auto-refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection <a href="#db-type" id="db-type"></a>

When you select **Tibero** or **OpenSQL** from the DB Type toggle in the GNB, the DB Select tree and metrics appropriate for that DB Type are displayed. When you switch the DB Type, the DB Select is reset to Select All.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree has a two-level **Service → Instance** structure. You can select by Select All, Primary DB, or Standby DB units.

- The Instance Alias is displayed up to a maximum of 30 characters, and if exceeded, it is shortened with an ellipsis (…).
- Sort order: Service creation date, most recent first → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending by alias within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree has a three-level **Service → Instance → Database** structure. You can select down to the Database level, and when you select a Database, the parent Instance is automatically included.

- If you select only some Databases, the Instance View aggregates metrics based solely on the selected Databases.
- The Instance Alias and Database name are each displayed up to a maximum of 10 characters, and if exceeded, they are shortened with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If the instance Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and if you select only that resource, it switches to the No Data screen.
{% endhint %}

### Top Pinned Metrics <a href="#undefined-1" id="undefined-1"></a>

These are the core metrics that are always displayed at the top of the page regardless of the DB Type. The overall instance Health status is displayed together as `Available(n)`, `Limited(n)`, `Unavailable(n)`, and `In Progress(n)`.

| Metric name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication delay time between Primary and Standby (displayed only when a Standby exists) |

### Viewing Metrics by Tab <a href="#undefined-2" id="undefined-2"></a>

When you enter the page, the default tab is **Quick Insight**. When you switch tabs, only the metrics suited to the respective situation are shown.

| Tab | Purpose of checking |
| --- | --- |
| Quick Insight | Quickly identify overall anomalies |
| Load | Check session/query load and the causes of CPU/Memory increases |
| I/O | Check disk read/write activity volume and the causes of response delays |
| Writes | Check bulk write operations and surges in transaction rollbacks |
| All | Diagnose complex causes by viewing all metrics at once |

You can pin frequently used tabs. When you hover over a tab, a pin icon appears, and clicking it sets that tab as the default entry tab. You can pin only one at most, and pinning another tab automatically unpins the existing one.

The metrics displayed per tab differ depending on the DB engine.

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
| Logical Reads (CNT) | Number of times data was read from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Hard Parse Count (CNT) | Number of SQL executions that went through the entire execution process without using the cache |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data that handled read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data that handled write requests from disk per unit of time |
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
| Logical Reads (CNT) | Number of times data was read from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Transactions (CNT) | Number of transactions completed within a unit of time |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data that handled read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data that handled write requests from disk per unit of time |
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

### Switching OpenSQL View <a href="#opensql-view" id="opensql-view"></a>

When OpenSQL is selected, the **Instance View** and **Database View** switch buttons are additionally displayed on the screen.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays the metrics of the selected Databases at the Instance level by summing or averaging them</li><li>The DB Select tree is displayed in two levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to three levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics provided only at the Instance level are still displayed on an Instance basis.
{% endhint %}
