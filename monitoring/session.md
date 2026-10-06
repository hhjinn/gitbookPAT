{% hint style="info" %}
**Note**

In the AWS environment, only the Tibero engine is supported, while in the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by querying key session metrics—such as the connected user, running SQL, wait events, and elapsed time—in table form.

It supports both the Tibero and OpenSQL engines, and using the **DB Type** toggle in the GNB displays the list of metrics appropriate for the corresponding engine. Enabling the Elapsed Time notification feature highlights session rows exceeding the configured threshold in color, visually identifying long-running sessions. Clicking a specific session row in the table shows detailed information and the full SQL text in a drawer.

## Session Monitoring

**Monitoring > Session Monitoring** In this menu, you query the list of sessions connected to the current DB instance in a real-time table. Using the GNB's **DB Type** toggle to switch between Tibero and OpenSQL displays the session metrics for the corresponding engine.

If the selected DB Type has no instances or no instance is selected, a no-data screen is displayed. If an instance status is abnormal, a ⚠️ is shown on that instance in the DB selection tree, and when only abnormal instances are selected, no session data is displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. In the GNB, **DB Type** using the toggle, **Tibero** or **OpenSQL**select.
3. Filter for the desired session using search.
4. **Elapsed Time notification settings (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the detail information drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual refresh (🔃)**: Clicking immediately refreshes the session data.
- **Auto refresh (⚙️)**: Configure whether to auto-refresh and the refresh interval.
- **Search**: Select a condition and enter a search term to filter for the desired session. If there are no matching sessions, the message "확인 가능한 데이터가 없습니다." (No data available to view.) is displayed.
- **Table settings (⚙️)**: Add or hide columns to display in the table.

The search conditions differ depending on the selected DB Type.

{% tabs %}
{% tab title="Tibero" %}
All / Instance Alias / SID / Username / Status
{% endtab %}
{% tab title="OpenSQL" %}
All / Instance Alias / Database Name / PID / Username / State
{% endtab %}
{% endtabs %}

### Session Column Configuration

Table columns are divided into three categories according to their display policy.

- **Always shown**: Required columns that cannot be hidden
- **Shown by default**: Columns shown by default that the user can hide
- **Optionally shown**: Columns hidden by default that can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always shown |
| Instance Alias | Instance identifier | Always shown |
| SID | Session ID (Tibero unique identifier) | Always shown |
| Serial# | Serial number for distinguishing session reuse | Optionally shown |
| PID | Server backend process ID | Always shown |
| Username | DB connection user | Always shown |
| Schema | Current schema name | Shown by default |
| Session Type | Session type | Shown by default |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always displayed |
| State | Detailed execution stage of the session | Always displayed |
| LOGON TIME | Session creation time (based on the database timezone) | Displayed by default |
| Elapsed Time | Elapsed time of the current SQL or session state | Always displayed |
| Wait Event | Event that the current session is waiting on | Displayed by default |
| Wait Time | Wait Event wait time | Displayed by default |
| WLock Wait | Type of lock that the session is waiting on | Displayed by default |
| SQL ID | Identifier of the SQL being executed | Displayed by default |
| SQL Text | Full text of the SQL being executed | Displayed by default |
| Prev SQL ID | Identifier of the previously executed SQL | Optionally displayed |
| Command | SQL command type (SELECT, INSERT, etc.) | Displayed by default |
| Program | Name of the connecting application | Displayed by default |
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

SQL ID and Prev SQL ID may also include negative values within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always displayed |
| Instance Alias | Instance identifier | Always displayed |
| datname | Name of the connected database | Always displayed |
| pid | Server backend process ID | Always displayed |
| usename | DB connection user | Always displayed |
| state | Session state | Always displayed |
| elapsed_time | Elapsed time of the current query execution | Always displayed |
| wait_event | Session wait event name | Always displayed |
| wait_event_type | Session wait event type | Always displayed |
| query | Current or last executed SQL statement | Always displayed |
| backend_type | Backend process type | Displayed by default |
| backend_start | Session or backend process start time | Displayed by default |
| application_name | Connected application name | Displayed by default |
| client_addr | Client IP address | Displayed by default |
| query_id | Query identifier | Displayed by default |
| leader_pid | Parallel operation leader process ID | Optionally displayed |
| client_port | Client communication port number | Optionally displayed |
| client_hostname | Client host name | Optionally displayed |
| query_start | Current or last query start time | Optionally displayed |
| xact_start | Current transaction start time | Optionally displayed |
| state_change | Time of the last session state change | Optionally displayed |
| xact_elapsed_time | Elapsed time of the current transaction | Optionally displayed |
| backend_xid | Top-level transaction ID | Optionally displayed |
| backend_xmin | xmin horizon of the current backend | Optionally displayed |
{% endtab %}
{% endtabs %}

### Elapsed Time alert settings

When an Elapsed Time threshold is set, session rows whose elapsed time exceeds the threshold are highlighted with color.

| State | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < warning threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time alert settings (⚙️)** click the icon to open the settings panel. Turn on the alert enable toggle, then enter the warning and caution thresholds and save.

| Item | Description | Input Rules |
| --- | --- | --- |
| Enable alerts* | Enable/Disable Notifications | Toggle |
| Warning* | Warning level threshold (seconds) | `0.1` ~ `1000.0`, allows up to one decimal place |
| Critical* | Critical level threshold (seconds) | `0.1` ~ `999.9`, allows up to one decimal place, must be less than the warning level |

*표기는 필수 입력 항목을 의미합니다.

Fields marked with * are required.

{% hint style="info" %}
**Note**

If you close the panel without turning on the notification enable toggle, the threshold values you entered are retained. However, they revert to their initial values when you navigate to another page.
{% endhint %}

### View Session Details

Clicking a specific row in the session table opens a details drawer on the right. The drawer title is displayed in the **Session Detail ({SID or PID})** format.

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service the session is connected to | ✓ | ✓ |
| Instance Alias | Alias of the instance the session is connected to | ✓ | ✓ |
| Database Name | Name of the database the session is connected to | — | ✓ |
| State | Detailed execution stage of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Identifier of the SQL being executed | ✓ | ✓ |
| SQL Text / Query | Full text of the SQL being executed | ✓ | ✓ |

Clicking the 📋 icon on the right of the SQL Text area copies the full SQL text to the clipboard. For sessions without SQL Text, a no data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
