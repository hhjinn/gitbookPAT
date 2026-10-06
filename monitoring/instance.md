On the instance monitoring page, you can check the key performance metrics of the database instance in real-time line charts.

Core metrics such as CPU usage, memory usage, and active session count are always displayed at the top of the screen. Select the Quick Insight, Load, I/O, Writes, and All tabs to focus on situational metrics, and when you switch between Tibero and OpenSQL using the DB Type toggle in the GNB, the metric configuration suited to that engine is applied.

{% hint style="info" %}
**Note**

The monitoring target resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Query Instance Monitoring <a href="#view-instance-monitoring" id="view-instance-monitoring"></a>

The instance monitoring page begins by selecting the DB Type and the resource to query at the top of the GNB. The selectable resource hierarchy and displayed metrics vary depending on the DB Type.

1. From the left menu **Monitoring > Instance Monitoring**.
2. In the DB Type toggle of the GNB **Tibero** or **OpenSQL**Select.
3. Select the Service and Instance to query in the DB Select tree. OpenSQL can select down to the Database level.
4. **(OpenSQL only)** As needed **Instance View** or **Database View** Click the button.
5. tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to query the desired metrics.
6. Click the ⚙️ icon to set up automatic refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection <a href="#db-type" id="db-type"></a>

In the DB Type toggle of the GNB **Tibero** or **OpenSQL**When selected, the DB Select tree and metrics suited to that DB Type are displayed. When you switch the DB Type, the DB Select is reset to Select All.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree is a **Service → Instance** two-level structure. You can select by All, Primary DB, or Standby DB.

- Instance Alias is displayed up to 30 characters; if exceeded, it is truncated with an ellipsis (…).
- Sort order: newest Service creation date → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending alias within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree is a **Service → Instance → Database** It is a three-level structure. You can select down to the Database level, and selecting a Database automatically includes its parent Instance.

- If you select only some Databases, the Instance View also aggregates metrics based only on the selected Databases.
- Instance Alias and Database name are each displayed up to 10 characters; if exceeded, they are truncated with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is shown in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Pinned Top Metrics <a href="#undefined-1" id="undefined-1"></a>

These are the key metrics always displayed at the top of the page regardless of DB Type. The overall instance Health status is `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`displayed together.

| Metric name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag between Primary and Standby (displayed only when a Standby exists) |

### Metric lookup by tab <a href="#undefined-2" id="undefined-2"></a>

When entering the page, the default tab is **Quick Insight**. When you switch tabs, only the metrics relevant to that situation are queried.

| Tab | Purpose of inspection |
| --- | --- |
| Quick Insight | Quickly identify overall anomalies |
| Load | Identify the cause of session/query load and CPU/Memory increases |
| I/O | Identify the cause of disk read/write activity volume and response latency |
| Writes | Identify bulk write operations and spikes in transaction rollbacks |
| All | Query all metrics at once to diagnose complex causes |

You can pin frequently used tabs. Hovering over a tab displays a pin icon, and clicking it sets that tab as the default entry tab. Only one tab can be pinned; pinning another tab automatically unpins the existing one.

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
| Logical Reads (CNT) | Number of reads of data from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Hard Parse Count (CNT) | Number of SQL executions that went through the full execution process without using the cache |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data served from disk read requests per unit of time |
| Disk Write Rate (MB/s) | Amount of data served from disk write requests per unit of time |
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
| Logical Reads (CNT) | Number of reads of data from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Transactions (CNT) | Number of transactions completed within a unit of time |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data served from disk read requests per unit of time |
| Disk Write Rate (MB/s) | Amount of data served from disk write requests per unit of time |
| Block Read Latency (ms/block) | Average time taken to read a single data block |
| Block Write Latency (ms/block) | Average time taken to write a single data block |

**Writes**

| Metric name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Transactions (CNT) | Number of transactions completed within a unit of time |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% endtabs %}

### OpenSQL View switching <a href="#opensql-view" id="opensql-view"></a>

When OpenSQL is selected, a **Instance View**and **Database View** switch button is additionally displayed on the screen.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays metrics at the Instance level by summing or averaging the selected Databases' metrics</li><li>The DB Select tree is displayed in two levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to three levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics provided only at the Instance level are displayed as-is on an Instance basis.
{% endhint %}
