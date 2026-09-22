OwlDB can restart databases in single or multiple instance configurations.

{% hint style="warning" %}
**Caution**

When restarting the database, all currently connected sessions are terminated. It may take a few minutes before the database becomes active again.
{% endhint %}

### **Menu Path**

The database restart feature can be accessed from the following menu paths.

- **OwlDB Console Screen > Dashboard**
- **OwlDB Console Screen > Overview > Instances tab**
- **OwlDB Console Screen > Overview > Instance Details**

### **Restart Button**

**Restart** The button is enabled according to the Status of the DB Service. The detailed enablement and selection conditions based on Status and Health state are described below in 'Restart Conditions'.

### **Restart Options **

**Restart** When you click the button, the restart options modal appears.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td><code>{instance alias}</code>Do you want to restart?</td></tr><tr><td>Content</td><td><ul><li>Terminate all currently connected sessions</li><li>Typically takes a few minutes; the DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to Restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection is disabled for single instance configurations</li></ul></td></tr><tr><td>Shutdown Mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How the Feature Works

Database restart behaves differently depending on the state of the selected instance.

### **Restart Conditions**

**Restart** The button is enabled when the Status of the DB Service is one of `Running`, `Degraded`, `Down`, `Updating`, `Failover` . However, the list of instances selectable in the modal is restricted based on the Health state of the instances.

| Status | Selectable Instances |
| --- | --- |
| `Running` | `Available` All instances in the state |
| `Degraded` | `Available` or `Limited` Instances in the state |
| `Down` | `Unavailable` All instances in the state |
| `Updating` | `Available` Instances in the state |
| `Failover` | `Available` Instances in the state |

In the following Status states, the **Restart** button is disabled.

- `Provisioning`
- `Stopped`
- `Terminating`
- `Retired` (In this case, it is displayed as changed to the **Delete** button on the Instance Details page.)

### **Status Change After Restart**

- **When all instances are selected**: Status changes to `Updating - In Progress`changes to
- **When some instances are selected**: Status changes to `Degraded - In Progress`changes to

## How to Use

The steps to restart the database are as follows.

1. **OwlDB Console Screen > Dashboard** Navigate to the menu.
2. Select the database to restart.
3. **Restart** Click the button. **Overview > Instances tab** or **Overview > Instance Details** You can also restart from the menu.
4. `{instance alias}`When the "Do you want to restart?" modal appears, **Instance to Restart** Select the instance to restart from the dropdown. For single instance configurations, the dropdown is disabled.
5. **Shutdown Mode** Select the database shutdown mode from the dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly halts all currently running operations, rolls back all in-progress transactions, and then shuts down | **Default value** |
| ABORT | Forcibly terminates the Tibero process |   |
| ABNORMAL | Forcibly terminate the server process without connecting to the Tibero server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Since some system resources (shared memory, semaphores, etc.) are not released, there is a possibility of corruption recovery, so its use is recommended only in the following situations. When normal shutdown is not possible due to an internal Tibero error When Tibero must be shut down immediately due to a H/W problem When Tibero must be shut down immediately due to an emergency situation such as hacking
- **ABNORMAL**: The server is shut down immediately with an OS forced termination signal, and since system resources (shared memory, semaphores, etc.) are not released, a corruption recovery process is required after restart, so its use is recommended only in the following situations. When normal shutdown is not possible due to an internal Tibero error When a shutdown command has been issued in a mode other than ABNORMAL but is being delayed and immediate forced shutdown is required When a problem occurs due to external factors such as H/W or OS and execution of a shutdown mode other than ABNORMAL fails When an emergency situation such as hacking occurs and Tibero must be shut down immediately
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown mode | Description | Remarks |
| --- | --- | --- |
| FAST | Roll back in-progress transactions, terminate connected sessions, and then quickly shut down the server | **Default value** |
| IMMEDIATE | Immediately halt all operations, forcibly roll back in-progress transactions, and then shut down the server |   |

{% hint style="warning" %}
**Caution**

**IMMEDIATE** When shutting down in this mode, some system resources may not be cleaned up properly, so it runs in automatic recovery mode. Its use is recommended only in the following cases.

- When normal shutdown is not possible due to an internal OpenSQL error
- When FAST mode shutdown does not respond for a certain period of time
- When emergency shutdown is required due to external factors such as H/W or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. **Restart** Click the button.

## Check the result

Once the restart operation begins, you can check the progress status by clicking the notification icon in the upper-right corner of the console screen. After the restart is complete, verify that the DB Service Status has changed to normal.
