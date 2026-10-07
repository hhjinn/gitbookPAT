On the instance monitoring page, you can check the key performance metrics of database instances in real-time line charts.

Core metrics such as CPU usage, memory usage, and the number of active sessions are always displayed at the top of the screen. Select the Quick Insight, Load, I/O, Writes, or All tab to focus on situation-specific metrics, and switch between Tibero and OpenSQL using the DB Type toggle in the GNB to apply the metric configuration suited to each engine.

{% hint style="info" %}
**Note**

The monitored resource hierarchy differs depending on the DB Type. Tibero displays metrics at the Service → Instance level, while OpenSQL allows selection down to the Service → Instance → Database level.
{% endhint %}

## View Instance Monitoring <a href="#view-instance-monitoring" id="view-instance-monitoring"></a>

The instance monitoring page starts by selecting the DB Type and the resource to view at the top of the GNB. The selectable resource hierarchy and the displayed metrics vary depending on the DB Type.

1. In the left menu, click **Monitoring > Instance Monitoring**.
2. In the GNB's DB Type toggle, select **Tibero** or **OpenSQL**.
3. In the DB Select tree, select the Service and Instance to view. OpenSQL allows selection down to the Database level.
4. **(OpenSQL only)** Click the **Instance View** or **Database View** button as needed.
5. Click a tab (**Quick Insight** / **Load** / **I/O** / **Writes** / **All**) to view the desired metrics.
6. Click the ⚙️ icon to set up automatic refresh.
7. Click the 🔃 icon to manually refresh the data.

### DB Type and Resource Selection <a href="#db-type" id="db-type"></a>

When you select **Tibero** or **OpenSQL** in the GNB's DB Type toggle, the DB Select tree and metrics corresponding to that DB Type are displayed. When you switch the DB Type, the DB Select is reset to Select All.

{% tabs %}
{% tab title="Tibero" %}
The DB Select tree has a 2-level **Service → Instance** structure. You can select by Select All, Primary DB, or Standby DB.

- Instance Alias is displayed up to a maximum of 30 characters, and if it exceeds this, it is shortened with an ellipsis (…).
- Sort order: newest Service creation date → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → ascending alias order within the same role
{% endtab %}
{% tab title="OpenSQL" %}
The DB Select tree has a 3-level **Service → Instance → Database** structure. You can select down to the Database level, and when you select a Database, its parent Instance is automatically included.

- If you select only some Databases, the Instance View also aggregates metrics based only on the selected Databases.
- Instance Alias and Database names are each displayed up to a maximum of 10 characters, and if they exceed this, they are shortened with an ellipsis (…).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

If there are no resources for the selected DB Type or no collected data, the No Data screen is displayed. If an instance's Health is abnormal, a ⚠️ icon is displayed in the DB Select tree, and if you select only that resource, it switches to the No Data screen.
{% endhint %}

### Top Fixed Metrics <a href="#undefined-1" id="undefined-1"></a>

These are the core metrics that are always displayed at the top of the page regardless of the DB Type. The overall instance Health status is displayed together as `Available(n)`, `Limited(n)`, `Unavailable(n)`, `In Progress(n)`.

| Metric Name | Description |
| --- | --- |
| CPU Usage (%) | CPU usage |
| Memory Usage (%) | Memory usage |
| Active Session Count (CNT) | Number of currently active sessions |
| Replication Lag (sec) | Replication lag time between Primary and Standby (displayed only when a Standby exists) |

### Metric Lookup by Tab <a href="#undefined-2" id="undefined-2"></a>

When entering the page, the default tab is **Quick Insight**. Switching tabs displays only the metrics relevant to that situation.

| Tab | Verification Purpose |
| --- | --- |
| Quick Insight | Quickly identify overall anomaly signs |
| Load | Identify the cause of session/query load and CPU/Memory increases |
| I/O | Identify disk read/write activity levels and the cause of response delays |
| Writes | Check for bulk write operations and surges in transaction rollbacks |
| All | Look up all metrics at once to diagnose complex causes |

You can pin frequently used tabs. Hovering over a tab displays a pin icon, and clicking it sets that tab as the default entry tab. Only one tab can be pinned at a time, and pinning another tab automatically unpins the existing one.

The metrics displayed for each tab vary depending on the DB engine.

{% tabs %}
{% tab title="Tibero" %}
**Quick Insight**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of sessions currently connected |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | Number of pages read directly from disk |
| Redo Log Rate (MB/s) | Amount of data written to the Redo Log per unit of time |
| User Rollbacks (CNT) | Number of transactions rolled back within a unit of time |

**Load**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of sessions currently connected |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Logical Reads (CNT) | Number of times data was read from the buffer cache |
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
| Total Session Count (CNT) | Total number of sessions currently connected |
| Execute Count (CNT) | Number of queries executed within a unit of time |
| Buffer Cache Hit (%) | Ratio of Logical Reads to Physical Reads |
| Physical Reads (CNT) | Number of pages read directly from disk |
| WAL Rate (MB/s) | Amount of data written to the WAL Log per unit of time |
| Rollbacks (CNT) | Number of transactions rolled back within a unit of time |

**Load**

| Metric Name | Description |
| --- | --- |
| Total Session Count (CNT) | Total number of sessions currently connected |
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

### OpenSQL View Switching <a href="#opensql-view" id="opensql-view"></a>

When OpenSQL is selected, **Instance View** and **Database View** toggle buttons are additionally displayed on the screen.

<table><thead><tr><th>View</th><th>Description</th></tr></thead><tbody><tr><td>Instance View (default)</td><td><ul><li>Displays the metrics of the selected Database summed or averaged at the Instance level</li><li>The DB Select tree is displayed in 2 levels</li></ul></td></tr><tr><td>Database View</td><td><ul><li>Displays metrics separately for each selected Database</li><li>The DB Select tree switches to 3 levels</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

Even when switching to Database View, metrics provided only at the Instance level continue to be displayed on an Instance basis.
{% endhint %}
