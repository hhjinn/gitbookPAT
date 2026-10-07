OwlDB can restart a database in a single-instance or multi-instance configuration.

{% hint style="warning" %}
**Caution**

When restarting the database, all currently connected sessions are terminated. It may take several minutes until it becomes active again.
{% endhint %}

### **Menu Path** <a href="#undefined" id="undefined"></a>

The database restart feature can be accessed from the following menu paths.

- **OwlDB console screen > Dashboard**
- **OwlDB console screen > Overview > Instances tab**
- **OwlDB console screen > Overview > Instance details**

### **Restart Button** <a href="#undefined-1" id="undefined-1"></a>

**Restart** The button is activated according to the Status of the DB Service. The detailed activation and selection conditions based on Status and Health state are explained in 'Restart Conditions' below.

### **Restart Options** <a href="#undefined-2" id="undefined-2"></a>

**Restart** When you click the button, the restart options modal appears.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td><code>{instance alias}</code>Do you want to restart?</td></tr><tr><td>Content</td><td><ul><li>Terminate all currently connected sessions</li><li>Typically takes several minutes; the DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to Restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection is disabled in a single-instance configuration</li></ul></td></tr><tr><td>Shutdown Mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How the Feature Works <a href="#how-it-works" id="how-it-works"></a>

The database restart operates differently depending on the state of the selected instance.

### **Restart Conditions** <a href="#undefined-3" id="undefined-3"></a>

**Restart** The button is activated when the Status of the DB Service is `Running`, `Degraded`, `Down`, `Updating`, `Failover` one of these. However, the list of instances that can be selected in the modal is limited depending on the Health state of the instances.

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
- `Retired` (In this case, on the instance details page **Delete** it is displayed as changed to the button.)

### **State Change After Restart** <a href="#undefined-4" id="undefined-4"></a>

- **When all instances are selected**: Status changes `Updating - In Progress`to
- **When some instances are selected**: Status changes `Degraded - In Progress`to

## How to Use <a href="#how-to-use" id="how-to-use"></a>

The way to restart the database is as follows.

1. **OwlDB console screen > Dashboard** Go to the menu.
2. Select the database to restart.
3. **Restart** Click the button. **Overview > Instances tab** or **Overview > Instance details** You can also restart from the menu.
4. `{instance alias}`When the "Do you want to restart?" modal appears, **Instance to Restart** Select the instance to restart from the dropdown. In a single-instance configuration, this dropdown is disabled.
5. **Shutdown Mode** Select the database shutdown mode from the dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly stops all currently running operations, rolls back all in-progress transactions, and then shuts down | **Default value** |
| ABORT | Forcibly terminates the Tibero process |   |
| ABNORMAL | Forcibly terminates the server process without connecting to the Tibero server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Since some system resources (shared memory, semaphores, etc.) are not released and there is a possibility of corruption recovery, it is recommended to use this only in the following situations. When normal shutdown is not possible due to an internal Tibero error. When an H/W problem occurs and Tibero must be shut down immediately. When an emergency such as hacking occurs and Tibero must be shut down immediately.
- **ABNORMAL**: The server is shut down immediately with an OS forced-termination signal, and since system resources (shared memory, semaphores, etc.) are not released, a corruption recovery process is required after restart. Therefore, it is recommended to use this only in the following situations. When normal shutdown is not possible due to an internal Tibero error. When a shutdown command has been issued in a mode other than ABNORMAL but is delayed and must be forcibly shut down immediately. When a problem occurs due to external factors such as H/W or OS, and the execution of a shutdown mode other than ABNORMAL fails. When an emergency such as hacking occurs and Tibero must be shut down immediately.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| FAST | Rolls back in-progress transactions, terminates connected sessions, and then quickly shuts down the server | **Default value** |
| IMMEDIATE | Immediately stops all operations, forcibly rolls back in-progress transactions, and then shuts down the server |   |

{% hint style="warning" %}
**Caution**

**IMMEDIATE** When shutting down in this mode, some system resources may not be properly cleaned up, so it runs in automatic recovery mode. It is recommended to use this only in the following cases.

- When normal shutdown is not possible due to an internal OpenSQL error
- When FAST mode shutdown does not respond for a certain period of time
- When an emergency shutdown is required due to external factors such as H/W or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. **Restart** Click the button.

## Checking the results <a href="#check-results" id="check-results"></a>

Once the restart operation begins, you can check the progress by clicking the notification icon in the top-right corner of the console screen. After the restart is complete, verify that the Status of the DB Service has changed to normal.
