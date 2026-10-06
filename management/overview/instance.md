**OwlDB**You can monitor the status and resource usage of running instances, and modify or restart instances as needed. When the instance status is `Available`some information may be missing.

{% hint style="info" %}
**Note**

- From the dashboard, you can navigate to the instance management page via the following path. **[List View]** Click the arrow icon next to the database alias > click the instance alias **[Card View]** Click the instance alias on the database card
- In the AWS environment, only the Tibero engine is supported.
{% endhint %}

## View Instance List <a href="#instance-list" id="instance-list"></a>

1. **Management > Overview**to navigate.
2. **Instance** Click the tab.
3. Check the status and resource usage of the instance you want to review in the list.

### Primary(Leader) DB display items <a href="#primary-leader-db" id="primary-leader-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | The current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | The operation mode of the DB (READ WRITE) |
| Replication Mode | The operating mode of the Primary DB (PERFORMANCE) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active sessions | Bar chart of the number of active sessions |
| Data Volume | data volume usage (including 90% threshold display) |
| Redo log Volume | redo log volume usage |
| Archive log Volume | archive log volume usage |
| Root Volume | root volume usage |
| Current Log | Most recent Redo log identifier value (displayed only in Standby configurations) |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | The operation mode of the DB (READ WRITE) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active sessions | Bar chart of the number of active sessions |
| Volume | The total and usage of OpenSQL volumes |
| Root Volume | root volume usage |
| Current Log | Most recent WAL log identifier value (LSN, displayed only in HA configurations) |
{% endtab %}
{% endtabs %}

### Standby(Replica) DB display items <a href="#standby-replica-db" id="standby-replica-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable / Retired) |
| Standby Status | `v$standby` Displayed when queried `status` Displays the value |
| Open Mode | The operation mode of the DB (MOUNTED / RECOVERY / READ WRITE / READ ONLY / READ ONLY WITH APPLY) |
| Log Replication Type | The replication method of the Standby (LGWR ASYNC / ARCH ASYNC) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active sessions | Bar chart of the number of active sessions (displayed only in Read Only status) |
| log last received | Redo log identifier value most recently received from the Primary (TSN value) |
| log last applied | Redo log identifier value most recently applied to the Standby (TSN value) |
| Replication Lag (seconds) | Replication lag time with the Primary DB |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | The current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | The operation mode of the DB (READ ONLY) |
| Log Replication Type | The replication method of the Replica (ASYNC / SYNC) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active sessions | Bar chart of the number of active sessions (always displayed) |
| log last received | WAL log identifier value most recently received from the Leader (LSN value) |
| log last applied | WAL log identifier value most recently applied to the Replica (LSN value) |
| Replication Lag (seconds) | Replication lag time with the Leader DB |
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Caution**

If the instance status is not `Available`If it is not, a banner appears at the top of the screen providing guidance on the current status.
{% endhint %}

## View Instance Details <a href="#instance-details" id="instance-details"></a>

<figure>
<img src="../../.gitbook/assets/image-fe0c2442.png" alt="">
<figcaption>Figure 1. Instance Detailed Information</figcaption>
</figure>

Clicking an alias in the instance list navigates to the detailed information page for that instance. The detail page is organized in the following order: top summary information, availability and replication information, resource usage status, network information, and database information.

1. On the Overview page **Instance** Click the tab.
2. In the instance list, click the alias of the instance whose detailed information you want to view.
3. On the detailed information page, review the top information, availability and replication information, resource usage information, network information, and database information.

### Top Information <a href="#undefined-2" id="undefined-2"></a>

| Item | Description | Primary/Leader | Standby/Replica |
| --- | --- | --- | --- |
| Health | Instance Status | Available / In progress / Limited / Unavailable | Common (Tibero includes Retired) |
| Role | Instance Role | Tibero: Primary / OpenSQL: Leader | Tibero: Standby(Recovery/Read Only) / OpenSQL: Replica |
| Instance Creation Date | Creation date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |
| Open Mode | DB Operation Mode | READ WRITE | Tibero: MOUNTED/RECOVERY/READ WRITE/READ ONLY/READ ONLY WITH APPLY, OpenSQL: READ ONLY |
| Last Update Date | Configuration change date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |

### Availability and Replication Information <a href="#undefined-3" id="undefined-3"></a>

Primary/Leader displays the items below, and Standby/Replica displays the same items with additional replication-related items.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description | Display Target |
| --- | --- | --- |
| Replication Mode | The operating mode of the Primary DB (PERFORMANCE) | Primary |
| Current Log | Latest Redo log identifier value (TSN) | Common |
| Standby Status | Standby replication status (may differ per node) | Standby |
| Log Replication Type | Replication method (LGWR ASYNC / ARCH ASYNC) | Standby |
| log last received | Latest Redo log received from Primary (TSN) | Standby |
| log last applied | Latest Redo log applied to Standby (TSN) | Standby |
| Replication Lag (seconds) | Replication lag time with Primary | Standby |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description | Display Target |
| --- | --- | --- |
| Current Log | Latest WAL log identifier value (LSN) | Common |
| Log Replication Type | Replication method (ASYNC / SYNC) | Standby |
| log last received | Latest WAL log received from Leader (LSN) | Replica |
| log last applied | Latest WAL log applied to Replica (LSN) | Replica |
| Replication Lag (seconds) | Replication lag time with Primary | Standby |
{% endtab %}
{% endtabs %}

### Resource Usage Information <a href="#undefined-5" id="undefined-5"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>CPU</td><td>Usage relative to provisioned vCPU (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Memory</td><td>Usage relative to provisioned memory (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Maximum number of connected sessions</td><td>Number of active sessions (line chart, refreshed every 5 seconds)</td><td><ul><li>Tibero Standby: Displayed only when in Read Only status</li><li>OpenSQL Replica: Always displayed</li></ul></td></tr></tbody></table>

### Network Information <a href="#undefined-6" id="undefined-6"></a>

| Item | Description |
| --- | --- |
| Host Name | Host name on which the database server is running |
| End Point | Client connection address (Private IP) |
| Port | Database communication port number |

### Database Information <a href="#undefined-7" id="undefined-7"></a>

{% hint style="info" %}
**Note**

Database information is provided only by the OpenSQL engine.
{% endhint %}

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is used (On / Off) |
| Database List | Displays sub-databases by name, Data Size (GB), active sessions, Bloat Ratio (%), and creation date; clicking a name navigates to the detailed information. |

The active session value is displayed based on the Primary node if the instance being queried is Primary/Leader, and based on the Standby node if it is Standby/Replica.

{% hint style="warning" %}
**Caution**

- When Health is `Available`If it is not, some information may be displayed as missing.
- When Health is `Retired`In this case (occurs only on Tibero's Standby instance), all information except Health is displayed as `-`, and **Restart** instead of the **Delete** button, the
{% endhint %}
