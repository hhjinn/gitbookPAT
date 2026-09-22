{% hint style="info" %}
**Reference**

In AWS environments, only the Tibero engine is supported, while in Azure environments both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by displaying key session metrics such as connected users, running SQL, wait events, and elapsed time in a table format.

## Session Monitoring

**Monitoring > Session Monitoring** This menu displays the list of sessions currently connected to the DB instance as a real-time table. In the GNB, **DB Type** switching between Tibero and OpenSQL with the toggle displays the session metrics for the corresponding engine.

If there is no instance for the selected DB Type or no instance is selected, a no-data screen is displayed. If an instance is in an abnormal state, ⚠️ is displayed on that instance in the DB selection tree, and if only abnormal instances are selected, no session data is displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. In the GNB, **DB Type** with the toggle, **Tibero** or **OpenSQL**select it.
3. Filter the desired sessions using search.
4. **Elapsed Time notification settings (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the details drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual refresh (🔃)**: Click to update the session data immediately.
- **Auto refresh (⚙️)**: Configure whether to auto-refresh and the interval.
- **Search**: Select a condition and enter a search term to filter the desired sessions. If no matching session exists, the message "확인 가능한 데이터가 없습니다." is displayed.
- **Table settings (⚙️)**: Add or hide columns to display in the table.

Search conditions vary depending on the selected DB Type.

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

- **Always Shown**: Essential columns that cannot be hidden
- **Shown by Default**: Columns that are shown by default and can be hidden by the user
- **Optionally Shown**: Columns that are hidden by default and can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Shown |
| Instance Alias | Instance identifier | Always Shown |
| SID | Session ID (Tibero unique identifier) | Always Shown |
| Serial# | Serial number used to distinguish session reuse | Optionally Shown |
| PID | Server backend process ID | Always Shown |
| Username | DB connection user | Always Shown |
| Schema | Current schema name | Shown by Default |
| Session Type | Session type | Shown by Default |
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

For SQL ID and Prev SQL ID, negative values are also within the normal range.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Column | Description | Display Policy |
| --- | --- | --- |
| Service Name | Service identifier | Always displayed |
| Instance Alias | Instance identifier | Always displayed |
| datname | Name of the connected database | Always displayed |
| pid | Server backend process ID | Always displayed |
| usename | DB connection user | Always displayed |
| state | Session State | Always displayed |
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

### Elapsed Time Alert Settings

When you set an Elapsed Time threshold, session rows whose elapsed time exceeds the criteria are highlighted in color.

| Status | Condition | Display Color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time Alert Settings (⚙️)** Clicking the icon opens the settings panel. Turn on the alert enable toggle, then enter the Warning and Caution thresholds and save.

| Item | Description | Input Rules |
| --- | --- | --- |
| Enable Alert* | Turn the alert feature on/off | Toggle |
| Warning* | Warning Level Threshold (seconds) | `0.1` ~ `1000.0`, decimals allowed up to one place |
| Caution* | Caution Level Threshold (seconds) | `0.1` ~ `999.9`, decimals allowed up to one place, must be less than the Warning Level |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

If you close the panel without turning on the notification enable toggle, the entered thresholds are retained. However, navigating away from the page resets them to their initial values.
{% endhint %}

### Viewing Session Details

Clicking a specific row in the session table opens a detail drawer on the right. The drawer title is displayed in the **Session Detail ({SID or PID})** format.

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service the session is connected to | ✓ | ✓ |
| Instance Alias | Alias of the instance the session is connected to | ✓ | ✓ |
| Database Name | Name of the database the session is connected to | — | ✓ |
| State | Detailed execution phase of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Identifier of the SQL being executed | ✓ | ✓ |
| SQL Text / Query | Full text of the SQL being executed | ✓ | ✓ |

Clicking the 📋 icon to the right of the SQL Text area copies the full SQL text to the clipboard. For sessions without SQL Text, a no-data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
