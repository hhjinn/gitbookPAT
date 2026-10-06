On the Instance Monitoring page, you can view key performance metrics of the database instance in real-time line charts.

Core metrics such as CPU usage, memory usage, and the number of active sessions are always displayed at the top of the screen. By selecting the Quick Insight, Load, I/O, Writes, or All tabs, you can focus on situation-specific metrics, and by switching between Tibero and OpenSQL using the GNB DB Type toggle, a metric configuration suited to the respective engine is applied.

{% hint style="info" %}
**Note**

The monitored resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Viewing Instance Monitoring <a href="#view-instance-monitoring" id="view-instance-monitoring"></a>

The Instance Monitoring page begins by selecting the DB Type and the resource to view at the top of the GNB. The selectable resource hierarchy and the displayed metrics vary depending on the DB Type.

1. In the left menu, **Monitoring > Instance Monitoring**Click.
2. In the GNB DB Type toggle, **Tibero** or **OpenSQL**Select
3. Select the Service and Instance to view in the DB Select tree. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** As needed, **Instance View** or **Database View** Click the button.
5. tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to view the desired metrics.
6. Click the ⚙️ icon to set up automatic refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection <a href="#db-type" id="db-type"></a>

In the GNB DB Type toggle, **Tibero** or **OpenSQL**When selected, the DB Select tree and metrics suited to the respective DB Type are displayed. Switching the DB Type resets DB Select to select all.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree is **Service → Instance** a 2-level structure. You can select at the All, Primary DB, and Standby DB levels.

- Instance Alias is displayed up to a maximum of 30 characters, and if exceeded, it is shortened with an ellipsis (…).
- Sort order: Service creation date, newest first → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending alias order within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree is **Service → Instance → Database** a 3-level structure. You can select down to the Database level, and when a Database is selected, its parent Instance is automatically included.

- If only some Databases are selected, the Instance View also aggregates metrics based only on the selected Databases.
- Instance Alias and Database names are each displayed up to a maximum of 10 characters, and if exceeded, they are shortened with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If the instance Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Top Fixed Metrics <a href="#undefined-1" id="undefined-1"></a>

These are core metrics always displayed at the top of the page regardless of the DB Type. The overall instance Health status is `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`displayed together.

| Metric name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag time between Primary and Standby (displayed only when a Standby exists) |

### Viewing Metrics by Tab <a href="#undefined-2" id="undefined-2"></a>

The default tab when entering the page is **Quick Insight**. When you switch tabs, only the metrics suited to the respective situation are viewed.

| Tab | Purpose of Checking |
| --- | --- |
| Quick Insight | Quickly identify overall anomaly signs |
| Load | Check session/query load and the cause of CPU/Memory increases |
| I/O | Check disk read/write activity volume and the cause of response delays |
| Writes | Check bulk write operations and surges in transaction rollbacks |
| All | Diagnose complex causes by viewing all metrics at once |

Frequently used tabs can be pinned. When you hover over a tab, a pin icon appears, and clicking it sets that tab as the default entry tab. Only up to 1 tab can be pinned, and pinning another tab automatically unpins the existing one.

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
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
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
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
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

When OpenSQL is selected, on the screen **Instance View**and **Database View** a switch button is additionally displayed.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays metrics of the selected Databases summed or averaged at the Instance level</li><li>The DB Select tree is displayed in 2 levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to 3 levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to the Database View, metrics provided only at the Instance level are displayed on an Instance basis as-is.
{% endhint %}
