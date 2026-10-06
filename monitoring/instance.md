On the instance monitoring page, you can check the key performance indicators of the database instance in real-time line charts.

Core metrics such as CPU utilization, memory utilization, and active session count are always displayed at the top of the screen. Select the Quick Insight, Load, I/O, Writes, or All tabs to focus on situation-specific metrics, and switching between Tibero and OpenSQL using the DB Type toggle in the GNB applies the metric configuration suited to that engine.

{% hint style="info" %}
**Note**

The monitoring target resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## Instance Monitoring Query

The instance monitoring page begins by selecting the DB Type and the resource to query at the top of the GNB. The selectable resource hierarchy and the metrics displayed vary depending on the DB Type.

1. From the left menu, **Monitoring > Instance Monitoring**Click it.
2. In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**.
3. Select the Service and Instance to query in the DB Select tree. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** As needed, **Instance View** or **Database View** Click the button.
5. tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to query the desired metrics.
6. Click the ⚙️ icon to configure automatic refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection

In the DB Type toggle of the GNB, **Tibero** or **OpenSQL**When selected, the DB Select tree and metrics suited to that DB Type are displayed. When switching the DB Type, the DB Select is reset to Select All.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree is **Service → Instance** a 2-level structure. You can select at the Select All, Primary DB, or Standby DB level.

- Instance Alias is displayed up to a maximum of 30 characters, and if exceeded, it is shortened with an ellipsis (…).
- Sort order: Service creation date newest first → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending alias order within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree is **Service → Instance → Database** a 3-level structure. You can select down to the Database level, and when a Database is selected, its parent Instance is automatically included.

- If only some Databases are selected, the Instance View also aggregates metrics based only on the selected Databases.
- Instance Alias and Database names are each displayed up to a maximum of 10 characters, and if exceeded, they are shortened with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If there are no resources in the selected DB Type or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and selecting only that resource switches to the No Data screen.
{% endhint %}

### Top Fixed Metrics

These are the core metrics always displayed at the top of the page regardless of DB Type. The overall instance Health status is `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`is also displayed together.

| Metric Name | Description |
| --- | --- |
| CPU Usage (%) | CPU Utilization |
| Memory Usage (%) | Memory Usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag between Primary and Standby (displayed only when a Standby exists) |

### Metric Lookup by Tab

The default tab when entering the page is **Quick Insight**. When you switch tabs, only the metrics relevant to that situation are retrieved.

| Tab | Purpose |
| --- | --- |
| Quick Insight | Quickly identify overall anomalies |
| Load | Identify the cause of session/query load and CPU/Memory increases |
| I/O | Identify disk read/write activity and the cause of response latency |
| Writes | Identify bulk write operations and spikes in transaction rollbacks |
| All | Retrieve all metrics at once to diagnose complex causes |

You can pin frequently used tabs. When you hover over a tab, a pin icon appears, and clicking it sets that tab as the default entry tab. Only one tab can be pinned at a time, and pinning another tab automatically unpins the existing one.

The metrics displayed per tab vary depending on the DB engine.

{% tabs %}
{% tab title="Tibero" %}
**Quick Insight**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Redo Log Rate (MB/s) | Amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | Number of transactions rolled back within a unit of time |

**Load**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of currently connected sessions |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of reads of data from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Hard Parse Count (CNT) | Number of SQL executions that went through the full execution process without using the cache |

**I/O**

| Metric Name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
| Multi Block Disk Read (block/s) | Number of multi-block disk read blocks within a unit of time (per second) |
| Physical Write (block/s) | Number of DBWR written blocks within a unit of time (per second) |

**Writes**

| Metric Name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Redo Entries (CNT) | Number of Redo records written to the log buffer |
| Redo Log Rate (MB/s) | Amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% tab title="OpenSQL" %}
**Quick Insight**

| Metric Name | Description |
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
| Logical Reads (CNT) | Number of reads of data from the buffer cache |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Transactions (CNT) | Number of transactions completed within a unit of time |

**I/O**

| Metric Name | Description |
| --- | --- |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Disk Read Rate (MB/s) | Amount of data processed for read requests from disk per unit of time |
| Disk Write Rate (MB/s) | Amount of data processed for write requests to disk per unit of time |
| Block Read Latency (ms/block) | Average time taken to read one data block |
| Block Write Latency (ms/block) | Average time taken to write one data block |

**Writes**

| Metric Name | Description |
| --- | --- |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Transactions (CNT) | Number of transactions completed within a unit of time |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |
{% endtab %}
{% endtabs %}

### OpenSQL View Switching

When OpenSQL is selected, the screen additionally displays a **Instance View**and **Database View** toggle button.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays metrics for the selected Database aggregated or averaged at the Instance level</li><li>The DB Select tree is displayed in 2 levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separated by each selected Database</li><li>The DB Select tree switches to 3 levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics that are only provided at the Instance level are still displayed on an Instance basis.
{% endhint %}
