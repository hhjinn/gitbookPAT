{% hint style="info" %}
**Note**

In AWS environments, only the Tibero engine is supported, while in Azure environments both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current state of sessions connected to the database in real time. It quickly identifies abnormal sessions by querying key session metrics—such as connected user, running SQL, wait events, and elapsed time—in table form.

It supports both the Tibero and OpenSQL engines, and using the GNB **DB Type** toggle to switch displays the metric list appropriate for the selected engine. Enabling the Elapsed Time alert feature highlights session rows that exceed the configured threshold in color, visually identifying long-running sessions. Clicking a specific session row in the table shows the detailed information and full SQL text in a drawer.

## Session Monitoring <a href="#session-monitoring" id="session-monitoring"></a>

**Monitoring > Session Monitoring** From the menu, query the list of sessions connected to the current DB instance in a real-time table. Using the GNB **DB Type** toggle to switch between Tibero and OpenSQL displays the session metrics for the selected engine.

If the selected DB Type has no instances or no instance is selected, the no-data screen is displayed. If an instance's status is abnormal, a ⚠️ is shown on that instance in the DB selection tree, and if only abnormal instances are selected, session data is not displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. Using the GNB **DB Type** toggle, **Tibero** or **OpenSQL**Select
3. filter the desired sessions through search.
4. **Elapsed Time Alert Settings (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the detailed information drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual Refresh (🔃)**: Click to immediately update the session data.
- **Auto Refresh (⚙️)**: Configure whether auto-refresh is enabled and its interval.
- **Search**: Select a condition and enter a search term to filter the desired sessions. If no sessions match, the message "No data available to view." is displayed.
- **Table Settings (⚙️)**: Add or hide columns to display in the table.

Search conditions vary depending on the selected DB Type.

{% tabs %}
{% tab title="Tibero" %}
All / Instance Alias / SID / Username / Status
{% endtab %}
{% tab title="OpenSQL" %}
All / Instance Alias / Database Name / PID / Username / State
{% endtab %}
{% endtabs %}

### Session Column Configuration <a href="#undefined-1" id="undefined-1"></a>

Table columns are divided into three types according to their display policy.

- **Always Displayed**: Required columns that cannot be hidden
- **Default Displayed**: Columns displayed by default that the user can hide
- **Optional Displayed**: Columns hidden by default that can be added from Table Settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display Policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| SID | Session ID (Tibero unique identifier) | Always Displayed |
| Serial# | Serial number for distinguishing session reuse | Optional Displayed |
| PID | Server backend process ID | Always Displayed |
| Username | DB connection user | Always Displayed |
| Schema | Current schema name | Default Displayed |
| Session Type | Session type | Default Displayed |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always Displayed |
| State | Detailed execution stage of the session | Always Displayed |
| LOGON TIME | Session creation time (based on the database timezone) | Default Displayed |
| Elapsed Time | Elapsed time of the current SQL or session state | Always Displayed |
| Wait Event | Event the current session is waiting on | Default Displayed |
| Wait Time | Wait Event wait time | Default Displayed |
| WLock Wait | Type of lock the session is waiting on | Default Displayed |
| SQL ID | Running SQL identifier | Default Displayed |
| SQL Text | Full text of the running SQL | Default Displayed |
| Prev SQL ID | Previously executed SQL identifier | Optional Displayed |
| Command | SQL command type (SELECT, INSERT, etc.) | Default Displayed |
| Program | Connecting application name | Default Displayed |
| Module | User-defined module name | Optional Displayed |
| Action | Action name of the module | Optional Displayed |
| IP Address | Client connection IP | Always Displayed |
| Client PID | Client process ID | Optional Displayed |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Default Displayed |
| PGA Used Memory | PGA memory in use by the session | Default Displayed |
| Machine | Host name of the connected session | Optional Displayed |
| OS USER | OS account name of the connected session | Optional Displayed |

{% hint style="info" %}
**Note**

For SQL ID and Prev SQL ID, negative values are also within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display Policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| datname | Connected database name | Always Displayed |
| pid | Server backend process ID | Always Displayed |
| usename | DB connection user | Always Displayed |
| state | Session status | Always Displayed |
| elapsed_time | Elapsed time of the current query execution | Always Displayed |
| wait_event | Session wait event name | Always Displayed |
| wait_event_type | Session wait event type | Always Displayed |
| query | Current or last executed SQL statement | Always Displayed |
| backend_type | Backend process type | Default Displayed |
| backend_start | Session or backend process start time | Default Displayed |
| application_name | Connecting application name | Default Displayed |
| client_addr | Client IP address | Default Displayed |
| query_id | Query identifier | Default Displayed |
| leader_pid | Parallel operation leader process ID | Optional Displayed |
| client_port | Client communication port number | Optional Displayed |
| client_hostname | Client host name | Optional Displayed |
| query_start | Current or last query start time | Optional Displayed |
| xact_start | Current transaction start time | Optional Displayed |
| state_change | Last session state change time | Optional Displayed |
| xact_elapsed_time | Current transaction elapsed time | Optional Displayed |
| backend_xid | Top-level transaction ID | Optional Displayed |
| backend_xmin | xmin horizon of the current backend | Optional Displayed |
{% endtab %}
{% endtabs %}

### Elapsed Time alert settings <a href="#elapsed-time" id="elapsed-time"></a>

When an Elapsed Time threshold is set, session rows whose elapsed time exceeds the criterion are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time Alert Settings (⚙️)** clicking the icon brings up the settings panel. Turn on the enable-alert toggle, then enter the Warning and Caution thresholds and save.

| Item | Description | Input rules |
| --- | --- | --- |
| Enable alert* | Turn the alert feature on/off | Toggle |
| Warning* | Warning-level threshold (seconds) | `0.1` ~ `1000.0`, allowed to one decimal place |
| Caution* | Caution-level threshold (seconds) | `0.1` ~ `999.9`, allowed to one decimal place, must be smaller than the Warning level |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

If you close the panel without turning on the enable-alert toggle, the entered thresholds are retained. However, navigating away from the page reverts them to their initial values.
{% endhint %}

### Viewing session detail information <a href="#undefined-3" id="undefined-3"></a>

Clicking a specific row in the session table opens a detail drawer on the right. The drawer title is displayed in the format **Session Detail ({SID or PID})** .

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service the session is connected to | ✓ | ✓ |
| Instance Alias | Alias of the instance the session is connected to | ✓ | ✓ |
| Database Name | Name of the database the session is connected to | — | ✓ |
| State | Detailed execution stage of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Running SQL identifier | ✓ | ✓ |
| SQL Text / Query | Full text of the running SQL | ✓ | ✓ |

Clicking the 📋 icon on the right of the SQL Text area copies the full SQL to the clipboard. For sessions without SQL Text, a no-data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
