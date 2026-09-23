On the Instance Monitoring page, you can view the key performance metrics of a database instance as real-time line charts.

Core metrics such as CPU Usage, Memory Usage, and Active Session Count are always displayed at the top of the screen. You can focus on situation-specific metrics by selecting the Quick Insight, Load, I/O, Writes, or All tab, and switching between Tibero and OpenSQL using the DB Type toggle in the GNB applies the metric configuration suited to each engine.

{% hint style="info" %}
**Note**

The resource tier being monitored differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Viewing Instance Monitoring

The Instance Monitoring page starts with selecting the DB Type and the target resource to view from the top of the GNB. The selectable resource tiers and the displayed metrics vary depending on the DB Type.

1. From the left menu, **Monitoring > Instance Monitoring**click.
2. In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**select.
3. Select the Service and Instance to view from the DB Select tree. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** As needed, **Instance View** or **Database View** click the button.
5. tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to view the desired metrics.
6. Click the ⚙️ icon to configure auto-refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection

In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**When you select, the DB Select tree and metrics suited to that DB Type are displayed. Switching the DB Type resets the DB Select to select all.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree has a **Service → Instance** two-level structure. You can select by select all, Primary DB, or Standby DB units.

- The Instance Alias is displayed up to 30 characters, and if it exceeds this, it is shortened with an ellipsis (…).
- Sort order: Service creation date (newest first) → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → alias ascending within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree has a **Service → Instance → Database** three-level structure. You can select down to the Database level, and selecting a Database automatically includes its parent Instance.

- If you select only some Databases, the Instance View aggregates metrics based only on the selected Databases.
- The Instance Alias and Database name are each displayed up to 10 characters, and if they exceed this, they are shortened with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If the selected DB Type has no resources or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Top Pinned Metrics

These are the core metrics that are always displayed at the top of the page regardless of the DB Type. The overall instance Health status is displayed together as `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`.

| Metric Name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag time between Primary and Standby (displayed only when a Standby exists) |

### Viewing Metrics by Tab

When entering the page, the default tab is **Quick Insight**. Switching tabs displays only the metrics relevant to that situation.

| Tab | Purpose of checking |
| --- | --- |
| Quick Insight | Quickly identify overall anomalies |
| Load | Check session/query load and the causes of CPU/Memory increases |
| I/O | Check disk read/write activity volume and the causes of response latency |
| Writes | Checking for bulk write operations and surges in transaction rollbacks |
| All | Diagnosing compound causes by querying all metrics at once |

Frequently used tabs can be pinned. Hovering over a tab displays a pin icon, and clicking it sets that tab as the default entry tab. Only one tab can be pinned at most, and pinning another tab automatically unpins the existing one.

The metrics displayed per tab vary depending on the DB engine.

{% tabs %}
{% tab title="Tibero" %}
**Quick Insight**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | The total number of currently connected sessions |
| Execute Count (CNT) | The number of queries executed within a unit of time |
| Buffer Cache Hit (%) | The ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | The number of pages read directly from disk |
| Redo Log Rate (MB/s) | The amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | The number of transactions rolled back within a unit of time |

**Load**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | The total number of currently connected sessions |
| Execute Count (CNT) | The number of queries executed within a unit of time |
| Logical Reads (CNT) | The number of times data was read from the buffer cache |
| Physical Reads (CNT) | The number of pages read directly from disk |
| Buffer Cache Hit (%) | The ratio of Logical Reads to Physical Reads |
| Hard Parse Count (CNT) | The number of SQL statements that went through the entire execution process without using the cache |

**I/O**

| Metric name | Description |
| --- | --- |
| Physical Reads (CNT) | The number of pages read directly from disk |
| Disk Read Rate (MB/s) | The amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | The amount of data processed for write requests to disk per unit of time |
| Multi Block Disk Read (block/s) | The number of multi-block disk read blocks within a unit of time (per second) |
| Physical Write (block/s) | The number of DBWR write blocks within a unit of time (per second) |

**Writes**

| Metric name | Description |
| --- | --- |
| Execute Count (CNT) | The number of queries executed within a unit of time |
| Redo Entries (CNT) | The number of Redo records written to the log buffer |
| Redo Log Rate (MB/s) | The amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | The number of transactions rolled back within a unit of time |
{% endtab %}
{% tab title="OpenSQL" %}
**Quick Insight**

| Metric name | Description |
| --- | --- |
| Total Session Count (CNT) | The total number of currently connected sessions |
| Execute Count (CNT) | The number of queries executed within a unit of time |
| Buffer Cache Hit (%) | The ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | The number of pages read directly from disk |
| WAL Rate (MB/s) | The amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | The number of transactions rolled back within a unit of time |

**Load**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of times data was read from the buffer cache |
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

<table data-full-width="true"><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays metrics of the selected Databases summed or averaged at the Instance level</li><li>The DB Select tree is displayed in 2 levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to 3 levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics provided only at the Instance level are still displayed based on the Instance.
{% endhint %}
