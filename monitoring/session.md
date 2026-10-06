{% hint style="info" %}
**Note**

In AWS environments, only the Tibero engine is supported, while in Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

Session Monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by retrieving key session metrics such as connected users, running SQL, wait events, and elapsed time in table form.

## Session Monitoring

**Monitoring > Session Monitoring** In the menu, you can view the list of sessions connected to the current DB instance in a real-time table. In the GNB, **DB Type** switching between Tibero and OpenSQL via the toggle displays the session metrics for that engine.

If the selected DB Type has no instance or no instance is selected, a no-data screen is displayed. If an instance's status is abnormal, a ⚠️ is shown next to that instance in the DB selection tree, and if only abnormal instances are selected, no session data is displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. In the GNB, **DB Type** using the toggle, **Tibero** or **OpenSQL**.
3. filter the desired sessions through search.
4. **Elapsed Time Alert Settings (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the detail drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual Refresh (🔃)**: Click to immediately refresh the session data.
- **Auto Refresh (⚙️)**: Configure whether auto-refresh is enabled and its interval.
- **Search**: Select a condition and enter a search term to filter the desired sessions. If no matching session exists, the message "확인 가능한 데이터가 없습니다." (No data available to view.) is displayed.
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

### Session Column Configuration

Table columns are classified into three types according to the display policy.

- **Always Displayed**: Required columns that cannot be hidden
- **Default Displayed**: Columns displayed by default that the user can hide
- **Optional Display**: Columns hidden by default that can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| SID | Session ID (Tibero unique identifier) | Always Displayed |
| Serial# | Serial number for distinguishing session reuse | Optional Display |
| PID | Server backend process ID | Always Displayed |
| Username | DB connection user | Always Displayed |
| Schema | Current schema name | Default Displayed |
| Session Type | Session type | Default Displayed |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always Displayed |
| State | Detailed execution phase of the session | Always Displayed |
| LOGON TIME | Session creation time (based on database timezone) | Default Displayed |
| Elapsed Time | Elapsed time of the current SQL or session status | Always Displayed |
| Wait Event | Event the current session is waiting on | Default Displayed |
| Wait Time | Wait Event wait time | Default Displayed |
| WLock Wait | Lock type the session is waiting on | Default Displayed |
| SQL ID | Executing SQL identifier | Default Displayed |
| SQL Text | Full text of the executing SQL | Default Displayed |
| Prev SQL ID | Previously executed SQL identifier | Optional Display |
| Command | SQL command type (SELECT, INSERT, etc.) | Default Displayed |
| Program | Connecting application name | Default Displayed |
| Module | User-defined module name | Optional Display |
| Action | Module action name | Optional Display |
| IP Address | Client connection IP | Always Displayed |
| Client PID | Client process ID | Optional Display |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Default Displayed |
| PGA Used Memory | PGA memory in use by the session | Default Displayed |
| Machine | Host name of the connected session | Optional Display |
| OS USER | OS account name of the connected session | Optional Display |

{% hint style="info" %}
**Note**

For SQL ID and Prev SQL ID, negative values are also within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Displayed |
| Instance Alias | Instance identifier | Always Displayed |
| datname | Connected database name | Always Displayed |
| pid | Server backend process ID | Always Displayed |
| usename | DB connection user | Always Displayed |
| state | Session status | Always Displayed |
| elapsed_time | Current query execution elapsed time | Always Displayed |
| wait_event | Session wait event name | Always Displayed |
| wait_event_type | Session wait event type | Always Displayed |
| query | Current or last executed SQL statement | Always Displayed |
| backend_type | Backend process type | Default Displayed |
| backend_start | Session or backend process start time | Default Displayed |
| application_name | Connecting application name | Default Displayed |
| client_addr | Client IP address | Default Displayed |
| query_id | Query identifier | Default Displayed |
| leader_pid | Parallel operation leader process ID | Optional Display |
| client_port | Client communication port number | Optional Display |
| client_hostname | Client host name | Optional Display |
| query_start | Current or last query start time | Optional Display |
| xact_start | Current transaction start time | Optional Display |
| state_change | Last change time of the session status | Optional Display |
| xact_elapsed_time | Current transaction elapsed time | Optional Display |
| backend_xid | Top-level transaction ID | Optional Display |
| backend_xmin | xmin horizon of the current backend | Optional Display |
{% endtab %}
{% endtabs %}

### Elapsed Time notification settings

When an Elapsed Time threshold is set, session rows whose elapsed time exceeds the criterion are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time Alert Settings (⚙️)** Clicking the icon brings up the settings panel. Turn on the enable notifications toggle, then enter the Warning and Caution thresholds and save.

| Item | Description | Input rules |
| --- | --- | --- |
| Enable notifications* | Turn notification feature on/off | Toggle |
| Warning* | Warning level threshold (seconds) | `0.1` ~ `1000.0`, allowed up to one decimal place |
| Caution* | Caution level threshold (seconds) | `0.1` ~ `999.9`, allowed up to one decimal place, must be smaller than the Warning level |

*표기는 필수 입력 항목을 의미합니다.

* indicates a required input field.

{% hint style="info" %}
**Note**

If you close the panel without turning on the enable notifications toggle, the entered thresholds are retained. However, navigating away from the page resets them to their initial values.
{% endhint %}

### View session details

Clicking a specific row in the session table opens a details drawer on the right. The drawer title is displayed **Session Detail ({SID or PID})** in this format.

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | DB Service name the session is connected to | ✓ | ✓ |
| Instance Alias | Instance alias the session is connected to | ✓ | ✓ |
| Database Name | Database name the session is connected to | — | ✓ |
| State | Detailed execution phase of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session status (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Executing SQL identifier | ✓ | ✓ |
| SQL Text / Query | Full text of the executing SQL | ✓ | ✓ |

Clicking the 📋 icon to the right of the SQL Text area copies the full SQL text to the clipboard. For sessions without SQL Text, a no data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
