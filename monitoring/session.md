{% hint style="info" %}
**Note**

In the AWS environment, it is supported only for the Tibero engine, while in the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

Session Monitoring is a feature that checks the current status of sessions connected to the database in real time. You can quickly identify abnormal sessions by viewing key session metrics such as the connected user, the running SQL, wait events, and elapsed time in a table format.

## Session Monitoring <a href="#session-monitoring" id="session-monitoring"></a>

In the **Monitoring > Session Monitoring** menu, you can view the list of sessions currently connected to the DB instance in a real-time table. When you switch between Tibero and OpenSQL using the **DB Type** toggle in the GNB, the session metrics of the respective engine are displayed.

If the selected DB Type has no instances or no instance is selected, the No Data screen is displayed. If the instance status is abnormal, a ⚠️ is displayed next to that instance in the DB Select tree, and if you select only abnormal instances, no session data is displayed.

1. Click the **Monitoring > Session Monitoring** menu.
2. Select **Tibero** or **OpenSQL** using the **DB Type** toggle in the GNB.
3. Filter the desired sessions using search.
4. Click the **Elapsed Time Alert Settings (⚙️)** icon to set a threshold.
5. Save the configured threshold.
6. Click a session row to open the detail drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual Refresh (🔃)**: Click to immediately refresh the session data.
- **Auto Refresh (⚙️)**: Sets whether to auto-refresh and the refresh interval.
- **Search**: Select a condition and enter a search term to filter the sessions you want. If no sessions match, the message "No data available to display." is shown.
- **Table Settings (⚙️)**: Add or hide columns to display in the table.

The search conditions vary depending on the selected DB Type.

{% tabs %}
{% tab title="Tibero" %}
All / Instance Alias / SID / Username / Status
{% endtab %}
{% tab title="OpenSQL" %}
All / Instance Alias / Database Name / PID / Username / State
{% endtab %}
{% endtabs %}

### Session Column Configuration <a href="#undefined-1" id="undefined-1"></a>

Table columns are classified into three types according to their display policy.

- **Always Displayed**: Required columns that cannot be hidden
- **Displayed by Default**: Columns that are displayed by default and can be hidden by the user
- **Optionally Displayed**: Columns that are hidden by default and can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always displayed |
| Instance Alias | Instance identifier | Always displayed |
| SID | Session ID (Tibero unique identifier) | Always displayed |
| Serial# | Serial number for distinguishing session reuse | Optionally displayed |
| PID | Server backend process ID | Always displayed |
| Username | DB connection user | Always displayed |
| Schema | Current schema name | Displayed by default |
| Session Type | Session type | Displayed by default |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always displayed |
| State | Detailed execution phase of the session | Always displayed |
| LOGON TIME | Session creation time (based on the database time zone) | Displayed by default |
| Elapsed Time | Elapsed time of the current SQL or session state | Always displayed |
| Wait Event | Event the current session is waiting on | Displayed by default |
| Wait Time | Wait Event wait time | Displayed by default |
| WLock Wait | Lock type the session is waiting on | Displayed by default |
| SQL ID | Identifier of the running SQL | Displayed by default |
| SQL Text | Full text of the running SQL | Displayed by default |
| Prev SQL ID | Identifier of the previously executed SQL | Optionally displayed |
| Command | SQL command type (SELECT, INSERT, etc.) | Displayed by default |
| Program | Connecting application name | Displayed by default |
| Module | User-defined module name | Optionally displayed |
| Action | Action name of the module | Optionally displayed |
| IP Address | Client connection IP | Always displayed |
| Client PID | Client process ID | Optionally displayed |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Displayed by default |
| PGA Used Memory | PGA memory in use by the session | Displayed by default |
| Machine | Host name of the connected session | Optionally displayed |
| OS USER | OS account name of the connected session | Optionally displayed |

{% hint style="info" %}
**Note**

For SQL ID and Prev SQL ID, negative values are also within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always displayed |
| Instance Alias | Instance identifier | Always displayed |
| datname | Connected database name | Always displayed |
| pid | Server backend process ID | Always displayed |
| usename | DB connection user | Always displayed |
| state | Session state | Always displayed |
| elapsed_time | Elapsed time of the current query execution | Always displayed |
| wait_event | Session wait event name | Always displayed |
| wait_event_type | Session wait event type | Always displayed |
| query | Current or last executed SQL statement | Always displayed |
| backend_type | Backend process type | Displayed by default |
| backend_start | Session or backend process start time | Displayed by default |
| application_name | Connecting application name | Displayed by default |
| client_addr | Client IP address | Displayed by default |
| query_id | Query identifier | Displayed by default |
| leader_pid | Parallel work leader process ID | Optionally displayed |
| client_port | Client communication port number | Optionally displayed |
| client_hostname | Client host name | Optionally displayed |
| query_start | Current or last query start time | Optionally displayed |
| xact_start | Current transaction start time | Optionally displayed |
| state_change | Last change time of the session state | Optionally displayed |
| xact_elapsed_time | Elapsed time of the current transaction | Optionally displayed |
| backend_xid | Top-level transaction ID | Optionally displayed |
| backend_xmin | xmin horizon of the current backend | Optionally displayed |
{% endtab %}
{% endtabs %}

### Elapsed Time Alert Settings <a href="#elapsed-time" id="elapsed-time"></a>

If you set an Elapsed Time threshold, session rows whose elapsed time exceeds the threshold are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

Click the **Elapsed Time Alert Settings (⚙️)** icon at the top of the table to open the settings panel. Turn on the alert enable toggle, then enter the warning and caution thresholds and save.

| Item | Description | Input rules |
| --- | --- | --- |
| Enable Alerts* | Turn the alert feature on/off | Toggle |
| Warning* | Warning level threshold (seconds) | `0.1` to `1000.0`, up to one decimal place allowed |
| Caution* | Caution level threshold (seconds) | `0.1` to `999.9`, up to one decimal place allowed, must be smaller than the warning level |

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

If you close the panel without enabling the alert activation toggle, the threshold you entered is retained. However, if you navigate away from the page, it reverts to the initial value.
{% endhint %}

### View session details <a href="#undefined-3" id="undefined-3"></a>

When you click a specific row in the session table, a details drawer opens on the right. The drawer title is displayed in the format **Session Detail ({SID or PID})**.

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service the session is connected to | ✓ | ✓ |
| Instance Alias | Alias of the instance the session is connected to | ✓ | ✓ |
| Database Name | Name of the database the session is connected to | — | ✓ |
| State | Detailed execution phase of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Identifier of the running SQL | ✓ | ✓ |
| SQL Text / Query | Full text of the running SQL | ✓ | ✓ |

Clicking the 📋 icon on the right of the SQL Text area copies the full SQL to the clipboard. For sessions with no SQL Text, a no-data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
