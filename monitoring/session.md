{% hint style="info" %}
**Note**

In AWS environments, it is supported only for the Tibero engine, and in Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It lets you quickly identify abnormal sessions by viewing key session metrics such as the connected user, running SQL, wait events, and elapsed time in table form.

## Session Monitoring <a href="#session-monitoring" id="session-monitoring"></a>

**Monitoring > Session Monitoring** In the menu, you can view the list of sessions currently connected to the DB instance in a real-time table. The GNB's **DB Type** toggle switches between Tibero and OpenSQL, and the session metrics of the corresponding engine are displayed.

If the selected DB Type has no instances or no instance is selected, the No Data screen is displayed. If an instance's status is abnormal, a ⚠️ is displayed for that instance in the DB Select tree, and if only abnormal instances are selected, no session data is displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. The GNB's **DB Type** toggle **Tibero** or **OpenSQL**is selected.
3. Filter the sessions you want using search.
4. **Elapsed Time alert settings (⚙️)** Click the icon to set the threshold.
5. Save the threshold you set.
6. Click a session row to open the detail drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual refresh (🔃)**: When clicked, the session data is refreshed immediately.
- **Auto refresh (⚙️)**: Set whether to auto-refresh and the interval.
- **Search**: Select a condition and enter a search term to filter the sessions you want. If there are no matching sessions, the message "There is no data available to view." is displayed.
- **Table settings (⚙️)**: Add or hide the columns to display in the table.

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

Table columns are categorized into three types according to the display policy.

- **Always Visible**: Required columns that cannot be hidden
- **Default Visible**: Columns shown by default that users can hide
- **Optional Visible**: Columns hidden by default that can be added from the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Visible |
| Instance Alias | Instance identifier | Always Visible |
| SID | Session ID (Tibero unique identifier) | Always Visible |
| Serial# | Serial number for distinguishing session reuse | Optional Visible |
| PID | Server backend process ID | Always Visible |
| Username | DB connection user | Always Visible |
| Schema | Current schema name | Default Visible |
| Session Type | Session type | Default Visible |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always Visible |
| State | Detailed execution stage of the session | Always Visible |
| LOGON TIME | Session creation time (based on database timezone) | Default Visible |
| Elapsed Time | Elapsed time of the current SQL or session status | Always Visible |
| Wait Event | Event the current session is waiting on | Default Visible |
| Wait Time | Wait Event wait time | Default Visible |
| WLock Wait | Lock type the session is waiting on | Default Visible |
| SQL ID | Identifier of the SQL being executed | Default Visible |
| SQL Text | Full text of the SQL being executed | Default Visible |
| Prev SQL ID | Identifier of the previously executed SQL | Optional Visible |
| Command | SQL command type (SELECT, INSERT, etc.) | Default Visible |
| Program | Connecting application name | Default Visible |
| Module | User-defined module name | Optional Visible |
| Action | Action name of the module | Optional Visible |
| IP Address | Client connection IP | Always Visible |
| Client PID | Client process ID | Optional Visible |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Default Visible |
| PGA Used Memory | PGA memory in use by the session | Default Visible |
| Machine | Host name of the connected session | Optional Visible |
| OS USER | OS account name of the connected session | Optional Visible |

{% hint style="info" %}
**Note**

SQL ID and Prev SQL ID include negative values within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Visible |
| Instance Alias | Instance identifier | Always Visible |
| datname | Connected database name | Always Visible |
| pid | Server backend process ID | Always Visible |
| usename | DB connection user | Always Visible |
| state | Session status | Always Visible |
| elapsed_time | Elapsed time of the current query execution | Always Visible |
| wait_event | Session wait event name | Always Visible |
| wait_event_type | Session wait event type | Always Visible |
| query | Current or last executed SQL statement | Always Visible |
| backend_type | Backend process type | Default Visible |
| backend_start | Session or backend process start time | Default Visible |
| application_name | Connecting application name | Default Visible |
| client_addr | Client IP address | Default Visible |
| query_id | Query identifier | Default Visible |
| leader_pid | Parallel operation leader process ID | Optional Visible |
| client_port | Client communication port number | Optional Visible |
| client_hostname | Client host name | Optional Visible |
| query_start | Start time of the current or last query | Optional Visible |
| xact_start | Current transaction start time | Optional Visible |
| state_change | Last change time of the session status | Optional Visible |
| xact_elapsed_time | Elapsed time of the current transaction | Optional Visible |
| backend_xid | Top-level transaction ID | Optional Visible |
| backend_xmin | xmin horizon of the current backend | Optional Visible |
{% endtab %}
{% endtabs %}

### Elapsed Time Alert Settings <a href="#elapsed-time" id="elapsed-time"></a>

When an Elapsed Time threshold is set, session rows whose elapsed time exceeds the criterion are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time alert settings (⚙️)** clicking the icon brings up the settings panel. Turn on the alert enable toggle, then enter the warning and caution thresholds and save.

| Item | Description | Input Rules |
| --- | --- | --- |
| Enable Alert* | Turn the alert feature on/off | Toggle |
| Warning* | Warning-level threshold (seconds) | `0.1` ~ `1000.0`, allowed up to one decimal place |
| Caution* | Caution-level threshold (seconds) | `0.1` ~ `999.9`, allowed up to one decimal place, must be smaller than the warning level |

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

If you close the panel without turning on the alert enable toggle, the entered thresholds are retained. However, navigating away from the page resets them to their initial values.
{% endhint %}

### Session Detail Lookup <a href="#undefined-3" id="undefined-3"></a>

Clicking a specific row in the session table opens a detail drawer on the right. The drawer title is displayed in the format **Session Detail ({SID or PID})** .

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service the session is connected to | ✓ | ✓ |
| Instance Alias | Alias of the instance the session is connected to | ✓ | ✓ |
| Database Name | Name of the database the session is connected to | — | ✓ |
| State | Detailed execution stage of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Identifier of the SQL being executed | ✓ | ✓ |
| SQL Text / Query | Full text of the SQL being executed | ✓ | ✓ |

Clicking the 📋 icon to the right of the SQL Text area copies the full SQL to the clipboard. For sessions without SQL Text, a no-data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
