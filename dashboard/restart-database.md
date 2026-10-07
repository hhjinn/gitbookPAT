OwlDB can restart a database in a single-instance or multi-instance configuration.

{% hint style="warning" %}
**Caution**

When the database is restarted, all currently connected sessions are terminated. It may take a few minutes before it becomes active again.
{% endhint %}

### **Menu path** <a href="#undefined" id="undefined"></a>

The database restart feature can be accessed from the following menu paths.

- **OwlDB console screen > Dashboard**
- **OwlDB console screen > Overview > Instance tab**
- **OwlDB console screen > Overview > Instance details**

### **Restart button** <a href="#undefined-1" id="undefined-1"></a>

The **Restart** button is enabled according to the DB Service's Status. The detailed enablement and selection conditions based on Status and Health are described below in 'Restart conditions'.

### **Restart options** <a href="#undefined-2" id="undefined-2"></a>

When you click the **Restart** button, the restart options modal appears.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td>Do you want to restart <code>{instance alias}</code>?</td></tr><tr><td>Content</td><td><ul><li>Terminates all currently connected sessions</li><li>Generally takes a few minutes; the DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection is disabled in a single-instance configuration</li></ul></td></tr><tr><td>Shutdown mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How the feature works <a href="#how-it-works" id="how-it-works"></a>

The database restart behaves differently depending on the state of the selected instance.

### **Restart conditions** <a href="#undefined-3" id="undefined-3"></a>

The **Restart** button is enabled when the DB Service's Status is one of `Running`, `Degraded`, `Down`, `Updating`, or `Failover`. However, the list of instances selectable in the modal is limited according to each instance's Health status.

| Status | Selectable instances |
| --- | --- |
| `Running` | All instances in the `Available` status |
| `Degraded` | Instances in the `Available` or `Limited` status |
| `Down` | All instances in the `Unavailable` status |
| `Updating` | Instances in the `Available` status |
| `Failover` | Instances in the `Available` status |

In the following Status states, the **Restart** button is disabled.

- `Provisioning`
- `Stopped`
- `Terminating`
- `Retired` (In this case, on the instance details page it changes to display a **Delete** button.)

### **Status change after restart** <a href="#undefined-4" id="undefined-4"></a>

- **When all instances are selected**: Status changes to `Updating - In Progress`
- **When some instances are selected**: Status changes to `Degraded - In Progress`

## How to use <a href="#how-to-use" id="how-to-use"></a>

The way to restart a database is as follows.

1. Go to the **OwlDB console screen > Dashboard** menu.
2. Select the database to restart.
3. Click the **Restart** button. You can also restart from the **Overview > Instance tab** or **Overview > Instance details** menu.
4. When the "Do you want to restart `{instance alias}`?" modal appears, select the instance to restart from the **Instance to restart** dropdown. In a single-instance configuration, this dropdown is disabled.
5. Select the database shutdown mode from the **Shutdown mode** dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly stops all currently running operations, rolls back all in-progress transactions, and then shuts down | **Default value** |
| ABORT | Forcibly terminates the Tibero process |   |
| ABNORMAL | Forcibly terminates the server process without connecting to the Tibero server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Because some system resources (shared memory, semaphores, etc.) are not released and there is a possibility of corruption recovery, its use is recommended only in the following situations. When a normal shutdown is not possible due to an internal Tibero error; when an H/W problem occurs and Tibero must be shut down immediately; when an emergency such as hacking occurs and Tibero must be shut down immediately
- **ABNORMAL**: Shuts down the server immediately with an OS forced-termination signal, and because system resources (shared memory, semaphores, etc.) are not released, a corruption recovery process is required after restart, so its use is recommended only in the following situations. When a normal shutdown is not possible due to an internal Tibero error; when a shutdown command has been issued with a mode other than ABNORMAL but is delayed and must be forcibly shut down immediately; when a problem occurs due to external factors such as H/W or OS and the execution of a shutdown mode other than ABNORMAL fails; when an emergency such as hacking occurs and Tibero must be shut down immediately
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| FAST | Rolls back in-progress transactions, terminates connected sessions, and then shuts down the server quickly | **Default value** |
| IMMEDIATE | Immediately stops all operations, forcibly rolls back in-progress transactions, and then shuts down the server |   |

{% hint style="warning" %}
**Caution**

When shutting down in **IMMEDIATE** mode, some system resources may not be cleaned up properly, so it runs in automatic recovery mode. Its use is recommended only in the following cases.

- When a normal shutdown is not possible due to an internal OpenSQL error
- When FAST mode shutdown does not respond for a certain period of time
- When an emergency shutdown is required due to external factors such as H/W or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. Click the **Restart** button.

## Checking the results <a href="#check-results" id="check-results"></a>

Once the restart operation begins, you can check the progress by clicking the notification icon at the top right of the console screen. After the restart is complete, verify that the DB Service's Status has changed normally.
