{% hint style="info" %}
**Note**

In AWS environments, it is supported only for the Tibero engine, while in Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by looking up key session metrics such as the connected user, running SQL, wait events, and elapsed time in a table format.

It supports both the Tibero and OpenSQL engines, and switching with the **DB Type** toggle in the GNB displays the metric list appropriate for that engine. Enabling the Elapsed Time notification feature highlights session rows that exceed the configured threshold with color, visually identifying long-running sessions. Clicking a specific session row in the table lets you check the detailed information and the full SQL text in a drawer.

## Session Monitoring <a href="#session-monitoring" id="session-monitoring"></a>

In the **Monitoring > Session Monitoring** menu, look up the list of sessions currently connected to the DB instance in a real-time table. Switching to Tibero or OpenSQL with the **DB Type** toggle in the GNB displays the session metrics for that engine.

If there is no instance for the selected DB Type or no instance is selected, a no-data screen is displayed. If an instance status is abnormal, a ⚠️ is displayed on that instance in the DB selection tree, and if only abnormal instances are selected, session data is not displayed.

1. Click the **Monitoring > Session Monitoring** menu.
2. Select **Tibero** or **OpenSQL** with the **DB Type** toggle in the GNB.
3. Filter the desired sessions using search.
4. Click the **Elapsed Time Notification Settings (⚙️)** icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the detailed information drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual Refresh (🔃)**: Clicking it immediately updates the session data.
- **Auto Refresh (⚙️)**: Set whether to auto-refresh and the interval.
- **Search**: Select a condition and enter a search term to filter the desired sessions. If no sessions match, the message "There is no data available to check." is displayed.
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

Table columns are classified into three types according to the display policy.

- **Always Displayed**: Required columns that cannot be hidden
- **Displayed by Default**: Columns displayed by default that the user can hide
- **Optionally Displayed**: Columns hidden by default that can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| SID | Session ID (Tibero unique identifier) | Always Displayed |
| Serial# | Serial number for distinguishing session reuse | Optionally Displayed |
| PID | Server backend process ID | Always Displayed |
| Username | DB connected user | Always Displayed |
| Schema | Current schema name | Displayed by Default |
| Session Type | Session type | Displayed by Default |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always Displayed |
| State | Detailed execution phase of the session | Always Displayed |
| LOGON TIME | Session creation time (based on the database timezone) | Displayed by Default |
| Elapsed Time | Elapsed time of the current SQL or session status | Always Displayed |
| Wait Event | Event the current session is waiting on | Displayed by Default |
| Wait Time | Wait Event wait time | Displayed by Default |
| WLock Wait | Type of lock the session is waiting on | Displayed by Default |
| SQL ID | Running SQL identifier | Displayed by Default |
| SQL Text | Full text of the running SQL | Displayed by Default |
| Prev SQL ID | Previously executed SQL identifier | Optionally Displayed |
| Command | SQL command type (SELECT, INSERT, etc.) | Displayed by Default |
| Program | Connecting application name | Displayed by Default |
| Module | User-defined module name | Optionally Displayed |
| Action | Module action name | Optionally Displayed |
| IP Address | Client connection IP | Always Displayed |
| Client PID | Client process ID | Optionally Displayed |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Displayed by Default |
| PGA Used Memory | PGA memory used by the session | Displayed by Default |
| Machine | Host name of the connected session | Optionally Displayed |
| OS USER | OS account name of the connected session | Optionally Displayed |

{% hint style="info" %}
**Note**

Negative values are also within the normal range for SQL ID and Prev SQL ID.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| datname | Connecting database name | Always Displayed |
| pid | Server backend process ID | Always Displayed |
| usename | DB connected user | Always Displayed |
| state | Session state | Always Displayed |
| elapsed_time | Elapsed time of the current query execution | Always Displayed |
| wait_event | Session wait event name | Always Displayed |
| wait_event_type | Session wait event type | Always Displayed |
| query | Current or last executed SQL statement | Always Displayed |
| backend_type | Backend process type | Displayed by Default |
| backend_start | Session or backend process start time | Displayed by Default |
| application_name | Connecting application name | Displayed by Default |
| client_addr | Client IP address | Displayed by Default |
| query_id | Query identifier | Displayed by Default |
| leader_pid | Parallel operation leader process ID | Optionally Displayed |
| client_port | Client communication port number | Optionally Displayed |
| client_hostname | Client host name | Optionally Displayed |
| query_start | Current or last query start time | Optionally Displayed |
| xact_start | Current transaction start time | Optionally Displayed |
| state_change | Time of the last session state change | Optionally Displayed |
| xact_elapsed_time | Elapsed time of the current transaction | Optionally Displayed |
| backend_xid | Top-level transaction ID | Optionally Displayed |
| backend_xmin | xmin horizon of the current backend | Optionally Displayed |
{% endtab %}
{% endtabs %}

### Elapsed Time alert settings <a href="#elapsed-time" id="elapsed-time"></a>

When you set an Elapsed Time threshold, session rows whose elapsed time exceeds the criteria are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < caution threshold | Default color |
| Caution | caution threshold ≤ Elapsed Time < warning threshold | Orange |
| Warning | Elapsed Time ≥ warning threshold | Red |

Click the **Elapsed Time alert settings (⚙️)** icon at the top of the table to open the settings panel. Turn on the enable alert toggle, enter the warning and caution thresholds, and save.

| Item | Description | Input rules |
| --- | --- | --- |
| Enable alert* | Turn the alert feature on/off | Toggle |
| Warning* | Warning-level threshold (seconds) | `0.1` to `1000.0`, allowed to one decimal place |
| Caution* | Caution-level threshold (seconds) | `0.1` to `999.9`, allowed to one decimal place, must be smaller than the warning level |

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

If you close the panel without turning on the enable alert toggle, the thresholds you entered are retained. However, they revert to their initial values when you navigate away from the page.
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
| SQL ID / Query ID | Running SQL identifier | ✓ | ✓ |
| SQL Text / Query | Full text of the running SQL | ✓ | ✓ |

Click the 📋 icon on the right of the SQL Text area to copy the full SQL text to the clipboard. For a session with no SQL Text, a no data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
