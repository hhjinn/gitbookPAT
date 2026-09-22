**OwlDB**You can monitor the status and resource usage of running instances, and modify or restart instances as needed. If the instance status is `Available`not this, some information may be missing.

{% hint style="info" %}
**Note**

- From the dashboard, you can navigate to the instance management page through the following path. **[List View]** Arrow icon next to the database alias > Click the instance alias **[Card View]** Click the instance alias on the database card
- In the AWS environment, only the Tibero engine is supported.
{% endhint %}

## Viewing the Instance List

1. **Management > Overview**Navigate to.
2. **Instances** Click the tab.
3. Check the status and resource usage of the instance to review in the list.

### Primary(Leader) DB Display Items

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the details page) |
| Created Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | DB operation mode (READ WRITE) |
| Replication Mode | Primary DB operation mode (PERFORMANCE) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart |
| Data Volume | data volume usage (including 90% threshold display) |
| Redo log Volume | redo log volume usage |
| Archive log Volume | archive log volume usage |
| Root Volume | root volume usage |
| Current Log | Most recent Redo log identifier (displayed only in Standby configuration) |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the details page) |
| Created Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In progress / Limited / Unavailable) |
| Open Mode | DB operation mode (READ WRITE) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Active session count bar chart |
| Volume | Total and usage of OpenSQL volumes |
| Root Volume | root volume usage |
| Current Log | Most recent WAL log identifier (LSN, displayed only in HA configuration) |
{% endtab %}
{% endtabs %}

### Standby(Replica) DB Display Items

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (click to navigate to the details page) |
| Created Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In progress / Limited / Unavailable / Retired) |
| Standby Status | `v$standby` Displayed when queried `status` Value display |
| Open Mode | DB operating mode (MOUNTED / RECOVERY / READ WRITE / READ ONLY / READ ONLY WITH APPLY) |
| Log Replication Type | Standby replication method (LGWR ASYNC / ARCH ASYNC) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions (displayed only in READ ONLY state) |
| log last received | Identifier (TSN value) of the most recently received Redo log from the Primary |
| log last applied | Identifier (TSN value) of the most recently applied Redo log on the Standby |
| Replication Lag (seconds) | Replication lag time with the Primary DB |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Alias | Displays the instance alias (clicking moves to the detailed information page) |
| Creation Date | Instance creation date and time (`yyyy-mm-dd HH:mm:ss`) |
| Health | Current status of the instance (Available / In Progress / Limited / Unavailable) |
| Open Mode | DB operating mode (READ ONLY) |
| Log Replication Type | Replica replication method (ASYNC / SYNC) |
| CPU | Usage bar chart relative to provisioned vCPU |
| Memory | Usage bar chart relative to provisioned memory |
| Active Sessions | Bar chart of the number of active sessions (always displayed) |
| log last received | Identifier (LSN value) of the most recently received WAL log from the Leader |
| log last applied | Identifier (LSN value) of the most recently applied WAL log on the Replica |
| Replication Lag (seconds) | Replication lag time with the Leader DB |
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Caution**

When the instance status is `Available`not this, a banner appears at the top of the screen to provide guidance on the current status.
{% endhint %}

## Viewing Instance Details

> 📷 **[이미지]** 이미지

Clicking an alias in the instance list moves to the detailed information page for that instance. The detail page is organized in the following order: top summary information, availability and replication information, resource usage status, network information, and database information.

1. On the Overview page, **Instances** click the tab.
2. In the instance list, click the alias of the instance whose details you want to view.
3. On the detail page, review the top information, availability and replication information, resource usage information, network information, and database information.

### Top Information

| Item | Description | Primary/Leader | Standby/Replica |
| --- | --- | --- | --- |
| Health | Instance status | Available / In progress / Limited / Unavailable | Common (Tibero includes Retired) |
| Role | Instance role | Tibero: Primary / OpenSQL: Leader | Tibero: Standby (Recovery/Read Only) / OpenSQL: Replica |
| Instance creation date | Creation date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |
| Open Mode | DB operating mode | READ WRITE | Tibero: MOUNTED/RECOVERY/READ WRITE/READ ONLY/READ ONLY WITH APPLY, OpenSQL: READ ONLY |
| Last update date | Configuration change date and time (`yyyy-mm-dd HH:mm:ss`) | Common | Common |

### Availability and Replication Information

Primary/Leader displays the items below, and Standby/Replica additionally displays replication-related items for the same items.

{% tabs %}
{% tab title="Tibero" %}
| Item | Description | Display Target |
| --- | --- | --- |
| Replication Mode | Operation mode of the Primary DB (PERFORMANCE) | Primary |
| Current Log | Latest Redo log identifier (TSN) | Common |
| Standby Status | Standby replication status (may differ per node) | Standby |
| Log Replication Type | Replication method (LGWR ASYNC / ARCH ASYNC) | Standby |
| log last received | Latest Redo log received from the Primary (TSN) | Standby |
| log last applied | Latest Redo log applied to the Standby (TSN) | Standby |
| Replication Lag (seconds) | Replication delay time relative to the Primary | Standby |
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description | Display Target |
| --- | --- | --- |
| Current Log | Latest WAL log identifier (LSN) | Common |
| Log Replication Type | Replication method (ASYNC / SYNC) | Standby |
| log last received | Latest WAL log received from the Leader (LSN) | Replica |
| log last applied | Latest WAL log applied to the Replica (LSN) | Replica |
| Replication Lag (seconds) | Replication delay time relative to the Primary | Standby |
{% endtab %}
{% endtabs %}

### Resource Usage Information

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>CPU</td><td>Usage relative to provisioned vCPU (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Memory</td><td>Usage relative to provisioned memory (pie chart)</td><td>Refreshed every 5 seconds</td></tr><tr><td>Maximum number of connected sessions</td><td>Number of active sessions (line chart, refreshed every 5 seconds)</td><td><ul><li>Tibero Standby: displayed only in Read Only state</li><li>OpenSQL Replica: always displayed</li></ul></td></tr></tbody></table>

### Network Information

| Item | Description |
| --- | --- |
| Host Name | Name of the host on which the database server is running |
| End Point | Client connection address (Private IP) |
| Port | Database communication port number |

### Database Information

{% hint style="info" %}
**Note**

Database information is provided only by the OpenSQL engine.
{% endhint %}

| Item | Description |
| --- | --- |
| Auto Vacuum | Whether Auto Vacuum is enabled (On / Off) |
| Database list | Displays child databases by name, Data Size (GB), active sessions, Bloat Ratio (%), and creation date; clicking a name navigates to its detailed information. |

The active session value is displayed based on the Primary node if the instance being queried is Primary/Leader, or based on the Standby node if it is Standby/Replica.

{% hint style="warning" %}
**Caution**

- If Health is `Available`not, some information may be missing from the display.
- If Health is `Retired`(occurs only on Tibero Standby instances), all information except Health is displayed as `-`, and **Restart** instead of the button, **Delete** a button appears.
{% endhint %}
