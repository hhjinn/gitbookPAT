On the Instance Monitoring page, you can view key performance indicators of database instances in real-time line charts.

Core metrics such as CPU usage, memory usage, and active session count are always displayed at the top of the screen. You can focus on situation-specific metrics by selecting the Quick Insight, Load, I/O, Writes, or All tab, and when you switch between Tibero and OpenSQL using the DB Type toggle in the GNB, the metric configuration appropriate for the corresponding engine is applied.

{% hint style="info" %}
**Note**

The monitored resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Viewing Instance Monitoring

The Instance Monitoring page begins by selecting the DB Type and the resource to view at the top of the GNB. The selectable resource hierarchy and displayed metrics vary depending on the DB Type.

1. From the left menu, **Monitoring > Instance Monitoring**click.
2. In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**select.
3. In the DB Select tree, select the Service and Instance to view. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** As needed, **Instance View** or **Database View** click the button.
5. Tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to view the desired metrics.
6. Click the ⚙️ icon to configure auto-refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection

In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**When you select it, the DB Select tree and metrics appropriate for the corresponding DB Type are displayed. When you switch the DB Type, the DB Select is reset to Select All.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree is a **Service → Instance** two-level structure. You can select at the Select All, Primary DB, or Standby DB level.

- The Instance Alias is displayed up to 30 characters, and if exceeded, it is truncated with an ellipsis (…).
- Sort order: Service creation date, most recent first → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → alias ascending within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree is a **Service → Instance → Database** three-level structure. You can select down to the Database level, and when you select a Database, its parent Instance is automatically included.

- If you select only some Databases, the Instance View also aggregates metrics based only on the selected Databases.
- The Instance Alias and Database name are each displayed up to 10 characters, and if exceeded, they are truncated with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Top Fixed Metrics

These are core metrics that are always displayed at the top of the page regardless of the DB Type. The overall instance Health status is displayed together as `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`.

| Metric name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication delay time between Primary-Standby (displayed only when a Standby exists) |

### Viewing Metrics by Tab

The default tab when entering the page is **Quick Insight**. When you switch tabs, only the metrics appropriate for the corresponding situation are viewed.

| Tab | Purpose of check |
| --- | --- |
| Quick Insight | Quickly identify overall anomaly signs |
| Load | Identify session/query load and the cause of CPU/Memory increases |
| I/O | Identify disk read/write activity volume and the cause of response latency |
| Writes | Checks for surges in bulk write operations and transaction rollbacks |
| All | Diagnoses compound causes by viewing all metrics at once |

Frequently used tabs can be pinned. When you hover over a tab, a pin icon appears, and clicking it sets that tab as the default entry tab. Only one tab can be pinned at a time; pinning another tab automatically unpins the previous one.

The metrics displayed for each tab vary depending on the DB engine.

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
| Hard Parse Count (CNT) | Number of SQL statements that went through the full execution process without using the cache |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
| Multi Block Disk Read (block/s) | Number of multi-block disk read blocks within a unit of time (per second) |
| Physical Write (block/s) | Number of blocks written by DBWR within a unit of time (per second) |

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

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of data reads from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Transactions (CNT) | Number of transactions completed within a unit of time |

**I/O**

| Metric Name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
| Block Read Latency (ms/block) | Average time taken to read a single data block |
| Block Write Latency (ms/block) | Average time taken to write a single data block |

**Writes**

| Metric Name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Transactions (CNT) | Number of transactions completed within a unit of time |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% endtabs %}

### Switching the OpenSQL View

When OpenSQL is selected, on the screen **Instance View**and **Database View** switch buttons are additionally displayed.

<table data-full-width="true"><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays the metrics of the selected Database aggregated or averaged at the Instance level</li><li>The DB Select tree is displayed in 2 levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separated by each selected Database</li><li>The DB Select tree switches to 3 levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics provided only at the Instance level are displayed as-is on an Instance basis.
{% endhint %}
