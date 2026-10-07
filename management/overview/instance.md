Monitor the status and resource usage of instances running in **OwlDB**, and modify or restart instances as needed. If the instance status is not `Available`, some information may be missing.

{% hint style="info" %}
**Note**

- From the dashboard, you can navigate to the instance management page via the following paths. **[List View]** Arrow icon next to the database alias > Click the instance alias **[Card View]** Click the instance alias on the database card
- In the AWS environment, only the Tibero engine is supported.
{% endhint %}

## Viewing the instance list <a href="#instance-list" id="instance-list"></a>

1. Go to **Management > Overview**.
2. Click the **Instances** tab.
3. Check the status and resource usage of the instance you want to review in the list.

### Primary (Leader) DB display items <a href="#primary-leader-db" id="primary-leader-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (clicking navigates to the detail page) |
| Creation Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| AZ | The availability zone where the instance is placed |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | The DB's operation mode (READ WRITE) |
| Replication Mode | The Primary DB's operating mode (PERFORMANCE) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart |
| Data Volume | data volume usage (including 90% threshold indicator) |
| Redo log Volume | redo log volume usage |
| Archive log Volume | archive log volume usage |
| Root Volume | root volume usage |
| Current Log | Most recent Redo log identifier (displayed only in Standby configuration) |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (clicking navigates to the detail page) |
| Creation Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| AZ | The availability zone where the instance is placed |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | The DB's operation mode (READ WRITE) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart |
| Volume | Total and usage of the OpenSQL volume |
| Root Volume | root volume usage |
| Current Log | Most recent WAL log identifier (LSN, displayed only in HA configuration) |
{% endtab %}
{% endtabs %}

### Standby (Replica) DB display items <a href="#standby-replica-db" id="standby-replica-db"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (clicking navigates to the detail page) |
| Creation Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| AZ | The availability zone where the instance is placed |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable / Retired) |
| Standby Status | Displays the `status` value shown when querying `v$standby` |
| Open Mode | The DB's operation mode (MOUNTED / RECOVERY / READ WRITE / READ ONLY / READ ONLY WITH APPLY) |
| Log Replication Type | The Standby's replication method (LGWR ASYNC / ARCH ASYNC) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart (displayed only in Read Only status) |
| log last received | Identifier of the Redo log most recently received from the Primary (TSN value) |
| log last applied | Identifier of the Redo log most recently applied to the Standby (TSN value) |
| Replication Lag (seconds) | Replication lag time with the Primary DB |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (clicking navigates to the detail page) |
| Creation Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| AZ | The availability zone where the instance is placed |
| Health | The current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | The DB's operation mode (READ ONLY) |
| Log Replication Type | The Replica's replication method (ASYNC / SYNC) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart (always displayed) |
| log last received | Identifier of the WAL log most recently received from the Leader (LSN value) |
| log last applied | Identifier of the WAL log most recently applied to the Replica (LSN value) |
| Replication Lag (seconds) | Replication lag time with the Leader DB |
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Caution**

When the instance status is not `Available`, a banner appears at the top of the screen providing guidance on the current status.
{% endhint %}

## Viewing instance detail information <a href="#instance-details" id="instance-details"></a>

<figure>
<img src="../../.gitbook/assets/image-dd2ff447.png" alt="">
<figcaption>Figure 1. Instance detail information</figcaption>
</figure>

Clicking an alias in the instance list navigates to the detail page for that instance. The detail page is organized in the following order: top summary information, availability and replication information, resource usage status, network information, and database information.

1. On the Overview page, click the **Instances** tab.
2. In the instance list, click the alias of the instance whose detail information you want to review.
3. On the detail page, check the top information, availability and replication information, resource usage information, network information, and database information.

### Top information <a href="#undefined-2" id="undefined-2"></a>

| Item | Description | Primary/Leader | Standby/Replica |
| --- | --- | --- | --- |
| Health | Instance status | Available / In progress / Limited / Unavailable | Common (Tibero includes Retired) |
| Role | Instance role | Tibero: Primary / OpenSQL: Leader | Tibero: Standby(Recovery/Read Only) / OpenSQL: Replica |
| Instance creation date | Creation date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |
| Open Mode | DB operation mode | READ WRITE | Tibero: MOUNTED/RECOVERY/READ WRITE/READ ONLY/READ ONLY WITH APPLY, OpenSQL: READ ONLY |
| Last update date | Configuration change date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |

### Availability and replication information <a href="#undefined-3" id="undefined-3"></a>

Primary/Leader displays the items below, and Standby/Replica displays the same items with additional replication-related items.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description | Display target |
| --- | --- | --- |
| AZ | The availability zone where it is placed | Common |
| Replication Mode | The Primary DB's operating mode (PERFORMANCE) | Primary |
| Current Log | Most recent Redo log identifier (TSN) | Common |
| Standby Status | Standby replication status (may differ per node) | Standby |
| Log Replication Type | Replication method (LGWR ASYNC / ARCH ASYNC) | Standby |
| log last received | Most recent Redo log received from the Primary (TSN) | Standby |
| log last applied | Most recent Redo log applied to the Standby (TSN) | Standby |
| Replication Lag (seconds) | Replication lag time with the Primary | Standby |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description | Display target |
| --- | --- | --- |
| AZ | The availability zone where it is placed | Common |
| Current Log | Most recent WAL log identifier (LSN) | Common |
| Log Replication Type | Replication method (ASYNC / SYNC) | Standby |
| log last received | Most recent WAL log received from the Leader (LSN) | Replica |
| log last applied | Most recent WAL log applied to the Replica (LSN) | Replica |
| Replication Lag (seconds) | Replication lag time with the Primary | Standby |
{% endtab %}
{% endtabs %}

### Resource usage information <a href="#undefined-5" id="undefined-5"></a>

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>CPU</td><td>Usage relative to provisioned vCPU (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Memory</td><td>Usage relative to provisioned memory (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Maximum number of connected sessions</td><td>Active session count (line chart, refreshed every 5 seconds)</td><td><ul><li>Tibero Standby: displayed only in Read Only status</li><li>OpenSQL Replica: always displayed</li></ul></td></tr></tbody></table>

### Network information <a href="#undefined-6" id="undefined-6"></a>

| Item | Description |
| --- | --- |
| Host Name | Host name where the database server is running |
| End Point | Client connection address (Private IP) |
| Port | Database communication port number |

### Database information <a href="#undefined-7" id="undefined-7"></a>

{% hint style="info" %}
**Note**

Database information is provided only by the OpenSQL engine.
{% endhint %}

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is used (On / Off) |
| Database list | Displays child databases by name, Data Size (GB), active sessions, Bloat Ratio (%), and creation date; clicking a name navigates to its detailed information. |

The active session value is displayed based on the Primary node if the instance being queried is Primary/Leader, and based on the Standby node if it is Standby/Replica.

{% hint style="warning" %}
**Caution**

- If Health is not `Available`, some information may be displayed as missing.
- If Health is `Retired` (which occurs only on a Tibero Standby instance), all information except Health is displayed as `-`, and a **Delete** button appears instead of the **Restart** button.
{% endhint %}
