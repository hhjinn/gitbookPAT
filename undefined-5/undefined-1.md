{% hint style="info" %}
**Note**

In AWS environments, only the Tibero engine is supported, while in Azure environments both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by querying key session metrics—such as the connected user, running SQL, wait events, and elapsed time—in table form.

## Session Monitoring

**Monitoring > Session Monitoring** From the menu, you can query the list of sessions currently connected to the DB instance as a real-time table. The GNB's **DB Type** When you switch between Tibero and OpenSQL using the toggle, the session metrics for the corresponding engine are displayed.

If there is no instance for the selected DB Type or no instance is selected, the No Data screen is displayed. If an instance's status is abnormal, a ⚠️ is displayed on that instance in the DB selection tree, and if only abnormal instances are selected, no session data is displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. The GNB's **DB Type** Using the toggle, **Tibero** or **OpenSQL**Select it.
3. Filter the sessions you want using search.
4. **Elapsed Time alert setting (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the details drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual refresh (🔃)**: Click to immediately update the session data.
- **Auto refresh (⚙️)**: Set whether to auto-refresh and the interval.
- **Search**: Select a condition and enter a search term to filter the sessions you want. If there are no matching sessions, the message "확인 가능한 데이터가 없습니다." (No data available) is displayed.
- **Table settings (⚙️)**: Add or hide the columns to display in the table.

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

Table columns are divided into three categories according to the display policy.

- **Always shown**: Required columns that cannot be hidden
- **Shown by default**: Columns that are shown by default and can be hidden by the user
- **Optionally shown**: Columns that are hidden by default and can be added in the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always shown |
| Instance Alias | Instance identifier | Always shown |
| SID | Session ID (Tibero unique identifier) | Always shown |
| Serial# | Serial number for distinguishing session reuse | Optionally shown |
| PID | Server backend process ID | Always shown |
| Username | DB connected user | Always shown |
| Schema | Current schema name | Shown by default |
| Session Type | Session type | Shown by default |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always displayed |
| State | Detailed execution steps of the session | Always displayed |
| LOGON TIME | Session creation time (based on database timezone) | Displayed by default |
| Elapsed Time | Elapsed time of the current SQL or session state | Always displayed |
| Wait Event | Event that the current session is waiting on | Displayed by default |
| Wait Time | Wait Event wait time | Displayed by default |
| WLock Wait | Type of lock the session is waiting on | Displayed by default |
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
| PGA Used Memory | PGA memory being used by the session | Displayed by default |
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
| state_change | Last change time of the session state | Optionally displayed |
| xact_elapsed_time | Elapsed time of the current transaction | Optionally displayed |
| backend_xid | Top-level transaction ID | Optionally displayed |
| backend_xmin | xmin horizon of the current backend | Optionally displayed |
{% endtab %}
{% endtabs %}

### Elapsed Time alert settings

When you set an Elapsed Time threshold, session rows whose elapsed time exceeds the criterion are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time alert settings (⚙️)** Clicking the icon opens the settings panel. Turn on the enable alerts toggle, then enter the Warning and Caution thresholds and save.

| Item | Description | Input rule |
| --- | --- | --- |
| Enable alerts* | Turn the alert feature on/off | Toggle |
| Warning* | Warning level threshold (seconds) | `0.1` ~ `1000.0`, up to one decimal place allowed |
| Caution* | Caution level threshold (seconds) | `0.1` ~ `999.9`, up to one decimal place allowed, must be smaller than the warning level |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

If you close the panel without turning on the notification enable toggle, the entered threshold values are retained. However, navigating away from the page resets them to their initial values.
{% endhint %}

### Viewing session details

Clicking a specific row in the session table opens a details drawer on the right. The drawer title is displayed in the format **Session Detail ({SID or PID})** .

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
