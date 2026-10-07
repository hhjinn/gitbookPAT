In **OwlDB**, you can monitor the status and resource usage of running instances, and modify or restart instances as needed. If the instance status is not `Available`, some information may be missing.

{% hint style="info" %}
**Note**

- From the dashboard, you can navigate to the instance management page through the following path. **[List View]** Arrow icon next to the database alias > Click the instance alias **[Card View]** Click the instance alias on the database card
- In AWS environments, only the Tibero engine is supported.
{% endhint %}

## View instance list <a href="#instance-list" id="instance-list"></a>

1. Go to **Management > Overview**.
2. Click the **Instances** tab.
3. Check the status and resource usage of the instance you want to review in the list.

### Primary (Leader) DB display items <a href="#primary-leader-db" id="primary-leader-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to go to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | DB operation mode (READ WRITE) |
| Replication Mode | Primary DB operation mode (PERFORMANCE) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions |
| Data Volume | data volume usage (including 90% threshold display) |
| Redo log Volume | redo log volume usage |
| Archive log Volume | archive log volume usage |
| Root Volume | root volume usage |
| Current Log | Most recent Redo log identifier value (displayed only in Standby configurations) |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to go to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | DB operation mode (READ WRITE) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions |
| Volume | Total and usage of OpenSQL volumes |
| Root Volume | root volume usage |
| Current Log | Most recent WAL log identifier value (LSN, displayed only in HA configurations) |
{% endtab %}
{% endtabs %}

### Standby (Replica) DB display items <a href="#standby-replica-db" id="standby-replica-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to go to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In progress / Limited / Unavailable / Retired) |
| Standby Status | Displays the `status` value returned when querying `v$standby` |
| Open Mode | DB operation mode (MOUNTED / RECOVERY / READ WRITE / READ ONLY / READ ONLY WITH APPLY) |
| Log Replication Type | Standby replication method (LGWR ASYNC / ARCH ASYNC) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions (displayed only in Read Only state) |
| log last received | Redo log identifier value recently received from Primary (TSN value) |
| log last applied | Redo log identifier value recently applied to Standby (TSN value) |
| Replication Lag (seconds) | Replication lag time with the Primary DB |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to go to the detailed information page) |
| Creation date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | DB operation mode (READ ONLY) |
| Log Replication Type | Replica replication method (ASYNC / SYNC) |
| CPU | Bar chart of usage relative to provisioned vCPU |
| Memory | Bar chart of usage relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions (always displayed) |
| log last received | WAL log identifier value recently received from Leader (LSN value) |
| log last applied | WAL log identifier value recently applied to Replica (LSN value) |
| Replication Lag (seconds) | Replication lag time with the Leader DB |
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Caution**

If the instance status is not `Available`, a banner appears at the top of the screen to provide guidance on the current status.
{% endhint %}

## View instance detailed information <a href="#instance-details" id="instance-details"></a>

<figure>
<img src="../../.gitbook/assets/image-fe0c2442.png" alt="">
<figcaption>Figure 1. Instance detailed information</figcaption>
</figure>

Clicking an alias in the instance list navigates to the detailed information page for that instance. The detail page is organized in the following order: top summary information, availability and replication information, resource usage status, network information, and database information.

1. On the Overview page, click the **Instances** tab.
2. In the instance list, click the alias of the instance whose detailed information you want to review.
3. On the detailed information page, check the top information, availability and replication information, resource usage information, network information, and database information.

### Top information <a href="#undefined-2" id="undefined-2"></a>

| Item | Description | Primary/Leader | Standby/Replica |
| --- | --- | --- | --- |
| Health | Instance status | Available / In progress / Limited / Unavailable | Common (Tibero includes Retired) |
| Role | Instance role | Tibero: Primary / OpenSQL: Leader | Tibero: Standby(Recovery/Read Only) / OpenSQL: Replica |
| Instance creation date | Creation date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |
| Open Mode | DB operation mode | READ WRITE | Tibero: MOUNTED/RECOVERY/READ WRITE/READ ONLY/READ ONLY WITH APPLY, OpenSQL: READ ONLY |
| Last update date | Configuration change date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |

### Availability and replication information <a href="#undefined-3" id="undefined-3"></a>

Primary/Leader displays the items below, and Standby/Replica additionally displays replication-related items in addition to the same items.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description | Display target |
| --- | --- | --- |
| Replication Mode | Primary DB operation mode (PERFORMANCE) | Primary |
| Current Log | Most recent Redo log identifier value (TSN) | Common |
| Standby Status | Standby replication status (may differ per node) | Standby |
| Log Replication Type | Replication method (LGWR ASYNC / ARCH ASYNC) | Standby |
| log last received | Most recent Redo log received from Primary (TSN) | Standby |
| log last applied | Most recent Redo log applied to Standby (TSN) | Standby |
| Replication Lag (seconds) | Replication lag time with Primary | Standby |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description | Display target |
| --- | --- | --- |
| Current Log | Most recent WAL log identifier value (LSN) | Common |
| Log Replication Type | Replication method (ASYNC / SYNC) | Standby |
| log last received | Most recent WAL log received from Leader (LSN) | Replica |
| log last applied | Most recent WAL log applied to Replica (LSN) | Replica |
| Replication Lag (seconds) | Replication lag time with Primary | Standby |
{% endtab %}
{% endtabs %}

### Resource usage information <a href="#undefined-5" id="undefined-5"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>CPU</td><td>Usage relative to provisioned vCPU (pie chart)</td><td>Refreshes every 5 seconds</td></tr><tr><td>Memory</td><td>Provisioned Memory vs. Usage (Pie Chart)</td><td>Refreshes every 5 seconds</td></tr><tr><td>Maximum Number of Connected Sessions</td><td>Number of Active Sessions (Line Chart, refreshed every 5 seconds)</td><td><ul><li>Tibero Standby: Displayed only in Read Only status</li><li>OpenSQL Replica: Always displayed</li></ul></td></tr></tbody></table>

### Network Information <a href="#undefined-6" id="undefined-6"></a>

| Item | Description |
| --- | --- |
| Host Name | Name of the host on which the database server is running |
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
| Database List | Displays sub-databases by Name, Data Size (GB), Active Sessions, Bloat Ratio (%), and Creation Date; clicking a name navigates to its detailed information. |

The Active Sessions value is displayed based on the Primary node if the instance being queried is Primary/Leader, and based on the Standby node if it is Standby/Replica.

{% hint style="warning" %}
**Caution**

- If Health is not `Available`, some information may be missing from the display.
- If Health is `Retired` (which occurs only on Tibero Standby instances), all information except Health is displayed as `-`, and a **Delete** button appears instead of the **Restart** button.
{% endhint %}
