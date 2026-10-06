{% hint style="info" %}
**Note**

In the AWS environment, only the Tibero engine is supported, whereas in the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

Session monitoring is a feature that checks the current status of sessions connected to the database in real time. It quickly identifies abnormal sessions by viewing key session metrics—such as the connected user, running SQL, wait events, and elapsed time—in a table format.

## Session Monitoring <a href="#session-monitoring" id="session-monitoring"></a>

**Monitoring > Session Monitoring** From the menu, view the list of sessions currently connected to the DB instance in a real-time table. Using the GNB's **DB Type** toggle to switch between Tibero and OpenSQL displays the session metrics of that engine.

If the selected DB Type has no instances or no instance is selected, the No Data screen is displayed. If an instance's status is abnormal, a ⚠️ is shown for that instance in the DB Select tree, and if only the abnormal instance is selected, session data is not displayed.

1. **Monitoring > Session Monitoring** Click the menu.
2. The GNB's **DB Type** toggle **Tibero** or **OpenSQL**Select.
3. Filter the sessions you want using search.
4. **Elapsed Time alert setting (⚙️)** Click the icon to set the threshold.
5. Save the configured threshold.
6. Click a session row to open the details drawer.
7. Click the 📋 icon to copy the full SQL text.

Use the following features at the top of the table.

- **Manual refresh (🔃)**: Clicking it immediately refreshes the session data.
- **Auto refresh (⚙️)**: Set whether to auto-refresh and the interval.
- **Search**: Select a condition and enter a search term to filter the sessions you want. If no sessions match, the message "No data available for review." is displayed.
- **Table settings (⚙️)**: Add or hide columns to display in the table.

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

Table columns are classified into three categories according to their display policy.

- **Always Visible**: Required columns that cannot be hidden
- **Visible by Default**: Columns that are shown by default and can be hidden by the user
- **Optional Display**: Columns that are hidden by default and can be added from the table settings

{% tabs %}
{% tab title="Tibero" %}
| Column | Description | Display policy |
| --- | --- | --- |
| Service Name | Service identifier | Always Visible |
| Instance Alias | Instance identifier | Always Visible |
| SID | Session ID (Tibero unique identifier) | Always Visible |
| Serial# | Serial number for distinguishing session reuse | Optional Display |
| PID | Server backend process ID | Always Visible |
| Username | DB connection user | Always Visible |
| Schema | Current schema name | Visible by Default |
| Session Type | Session type | Visible by Default |
| Status | Session status (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | Always Visible |
| State | Detailed execution stage of the session | Always Visible |
| LOGON TIME | Session creation time (based on the database time zone) | Visible by Default |
| Elapsed Time | Elapsed time of the current SQL or session status | Always Visible |
| Wait Event | Event that the current session is waiting on | Visible by Default |
| Wait Time | Wait Event wait time | Visible by Default |
| WLock Wait | Type of lock that the session is waiting on | Visible by Default |
| SQL ID | Identifier of the currently executing SQL | Visible by Default |
| SQL Text | Full text of the currently executing SQL | Visible by Default |
| Prev SQL ID | Identifier of the previously executed SQL | Optional Display |
| Command | SQL command type (SELECT, INSERT, etc.) | Visible by Default |
| Program | Connecting application name | Visible by Default |
| Module | User-defined module name | Optional Display |
| Action | Action name of the module | Optional Display |
| IP Address | Client connection IP | Always Visible |
| Client PID | Client process ID | Optional Display |
| CONSUMED_CPU_TIME | Cumulative CPU time used by the session | Visible by Default |
| PGA Used Memory | PGA memory used by the session | Visible by Default |
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
| backend_type | Backend process type | Visible by Default |
| backend_start | Session or backend process start time | Visible by Default |
| application_name | Connecting application name | Visible by Default |
| client_addr | Client IP address | Visible by Default |
| query_id | Query identifier | Visible by Default |
| leader_pid | Parallel operation leader process ID | Optional Display |
| client_port | Client communication port number | Optional Display |
| client_hostname | Client host name | Optional Display |
| query_start | Current or last query start time | Optional Display |
| xact_start | Current transaction start time | Optional Display |
| state_change | Last change time of the session status | Optional Display |
| xact_elapsed_time | Elapsed time of the current transaction | Optional Display |
| backend_xid | Top-level transaction ID | Optional Display |
| backend_xmin | xmin horizon of the current backend | Optional Display |
{% endtab %}
{% endtabs %}

### Elapsed Time Alert Settings <a href="#elapsed-time" id="elapsed-time"></a>

When you set an Elapsed Time threshold, session rows whose elapsed time exceeds the criterion are highlighted in color.

| Status | Condition | Display color |
| --- | --- | --- |
| Normal | Elapsed Time < Caution threshold | Default color |
| Caution | Caution threshold ≤ Elapsed Time < Warning threshold | Orange |
| Warning | Elapsed Time ≥ Warning threshold | Red |

At the top of the table, **Elapsed Time alert setting (⚙️)** clicking the icon opens the settings panel. Turn on the alert enable toggle, then enter the Warning and Caution thresholds and save.

| Item | Description | Input Rules |
| --- | --- | --- |
| Enable Alerts* | Turn the alert feature on/off | Toggle |
| Warning* | Warning-level threshold (seconds) | `0.1` ~ `1000.0`, allowed to one decimal place |
| Caution* | Caution-level threshold (seconds) | `0.1` ~ `999.9`, allowed to one decimal place, must be smaller than the Warning level |

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

If you close the panel without turning on the alert enable toggle, the thresholds you entered are retained. However, if you navigate away from the page, they revert to their initial values.
{% endhint %}

### Session Detail Lookup <a href="#undefined-3" id="undefined-3"></a>

When you click a specific row in the session table, a detail drawer opens on the right. The drawer title is displayed in the format **Session Detail ({SID or PID})** .

| Item | Description | Tibero | OpenSQL |
| --- | --- | --- | --- |
| Service Name | Name of the DB Service to which the session is connected | ✓ | ✓ |
| Instance Alias | Alias of the instance to which the session is connected | ✓ | ✓ |
| Database Name | Name of the database to which the session is connected | — | ✓ |
| State | Detailed execution stage of the session | ✓ | ✓ |
| Elapsed Time | Elapsed time of the SQL or session state (seconds) | ✓ | ✓ |
| SQL ID / Query ID | Identifier of the currently executing SQL | ✓ | ✓ |
| SQL Text / Query | Full text of the currently executing SQL | ✓ | ✓ |

Clicking the 📋 icon on the right side of the SQL Text area copies the full SQL to the clipboard. For sessions with no SQL Text, a no-data screen is displayed in that area. To close the drawer, click the » icon; to view it wider, click the ⛶ icon.
