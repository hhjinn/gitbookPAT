OwlDB can restart databases configured as single or multiple instances.

{% hint style="warning" %}
**Caution**

When a database is restarted, all currently connected sessions are terminated. It may take a few minutes before it becomes active again.
{% endhint %}

### Menu path <a href="#undefined" id="undefined"></a>

The database restart function can be accessed from the following menu paths.

- **OwlDB console screen > Dashboard**
- **OwlDB console screen > Overview > Instance tab**
- **OwlDB console screen > Overview > Instance detail information**

### Restart button <a href="#undefined-1" id="undefined-1"></a>

**Restart** The button is enabled according to the Status of the DB Service. Detailed enablement and selection conditions based on Status and Health state are described below in 'Restart Available Conditions'.

### Restart options <a href="#undefined-2" id="undefined-2"></a>

**Restart** When you click the button, the restart options modal appears.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td><code>{instance alias}</code>Do you want to restart?</td></tr><tr><td>Content</td><td><ul><li>Terminate all currently connected sessions</li><li>Typically takes a few minutes; DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection disabled for single instance configuration</li></ul></td></tr><tr><td>Shutdown mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How the function works <a href="#how-it-works" id="how-it-works"></a>

Database restart behaves differently depending on the state of the selected instance.

### Restart available conditions <a href="#undefined-3" id="undefined-3"></a>

**Restart** The button is enabled when the DB Service Status is `Running`, `Degraded`, `Down`, `Updating`, `Failover` one of these. However, depending on the Health state of the instance, the list of instances selectable in the modal is limited.

| Status | Selectable instances |
| --- | --- |
| `Running` | `Available` All instances in the state |
| `Degraded` | `Available` or `Limited` Instances in the state |
| `Down` | `Unavailable` All instances in the state |
| `Updating` | `Available` Instances in the state |
| `Failover` | `Available` Instances in the state |

In the following Status states, **Restart** the button is disabled.

- `Provisioning`
- `Stopped`
- `Terminating`
- `Retired` (In this case, on the instance detail information page it **Delete** is displayed as changed to the button.)

### State change after restart <a href="#undefined-4" id="undefined-4"></a>

- **When all instances are selected**: Status changes to `Updating - In Progress`changes to
- **When some instances are selected**: Status changes to `Degraded - In Progress`changes to

## How to use <a href="#how-to-use" id="how-to-use"></a>

The way to restart a database is as follows.

1. **OwlDB console screen > Dashboard** Navigate to the menu.
2. Select the database to restart.
3. **Restart** Click the button. **Overview > Instance tab** or **Overview > Instance detail information** You can also restart from the menu.
4. `{instance alias}`When the Do you want to restart? modal appears, **Instance to restart** select the instance to restart from the dropdown. For a single instance configuration, that dropdown is disabled.
5. **Shutdown mode** Select the database shutdown mode from the dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly stop all currently running operations, roll back all in-progress transactions, then shut down | **Default value** |
| ABORT | Force termination of the Tibero process |   |
| ABNORMAL | Force termination of the server process without connecting to the Tibero server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Some system resources (shared memory, semaphores, etc.) may not be released, raising the possibility of corruption recovery, so use is recommended only in the following situations. When normal shutdown is not possible due to a Tibero internal error When Tibero must be shut down immediately due to an H/W problem When Tibero must be shut down immediately due to an emergency such as hacking
- **ABNORMAL**: The server is shut down immediately with an OS forced termination signal, and system resources (shared memory, semaphores, etc.) are not released, so a corruption recovery process is required after restart; therefore, use is recommended only in the following situations. When normal shutdown is not possible due to a Tibero internal error When a shutdown command was issued in a shutdown mode other than ABNORMAL but is delayed, and immediate forced termination is required When a problem occurs due to external factors such as H/W or OS and execution of a shutdown mode other than ABNORMAL fails When Tibero must be shut down immediately due to an emergency such as hacking
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| FAST | Roll back in-progress transactions, terminate connected sessions, then quickly shut down the server | **Default value** |
| IMMEDIATE | Immediately stop all operations, forcibly roll back in-progress transactions, then shut down the server |   |

{% hint style="warning" %}
**Caution**

**IMMEDIATE** When shutting down in this mode, some system resources may not be cleaned up properly, so it runs in automatic recovery mode. Use is recommended only in the following cases.

- When normal shutdown is not possible due to an OpenSQL internal error
- When FAST mode shutdown is unresponsive for a certain period
- When emergency shutdown is required due to external factor problems such as H/W or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. **Restart** Click the button.

## Check the result <a href="#check-results" id="check-results"></a>

When the restart operation begins, you can check the progress status by clicking the notification icon in the top right of the console screen. After the restart completes, verify that the DB Service Status has changed normally.

{% hint style="warning" %}
**Caution**

If the Status Code in the banner after restart is `Issue: VM Down`, depending on the CSP you are using, please **AWS** [aws_owldb_support@tibero.com](mailto:aws_owldb_support@tibero.com) or **Azure** [azure_owldb_support@tibero.com](mailto:azure_owldb_support@tibero.com) contact
{% endhint %}
