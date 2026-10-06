OwlDB can restart databases in single-instance or multi-instance configurations.

{% hint style="warning" %}
**Caution**

When the database is restarted, all currently connected sessions are terminated. It may take a few minutes before it becomes active again.
{% endhint %}

### Menu Path

The database restart feature can be accessed from the following menu paths.

- **OwlDB console screen > Dashboard**
- **OwlDB console screen > Overview > Instances tab**
- **OwlDB console screen > Overview > Instance details**

### Restart button

**Restart** The button is enabled according to the Status of the DB Service. The detailed enablement and selection conditions based on Status and Health are described in the 'Restart Conditions' section below.

### Restart options 

**Restart** When you click the button, the restart options modal appears.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td><code>{instance alias}</code>Do you want to restart?</td></tr><tr><td>Content</td><td><ul><li>Terminate all currently connected sessions</li><li>Typically takes a few minutes; the DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection is disabled in a single-instance configuration</li></ul></td></tr><tr><td>Shutdown mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How the feature works

Database restart behaves differently depending on the status of the selected instance.

### Restart Conditions

**Restart** The button is enabled when the Status of the DB Service is one of `Running`, `Degraded`, `Down`, `Updating`, `Failover` . However, the list of instances that can be selected in the modal is limited based on the Health status of the instances.

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
- `Retired` (In this case, on the instance details page it is **Delete** displayed as a button instead.)

### Status change after restart

- **When all instances are selected**: Status changes to `Updating - In Progress`[T_54]
When some instances are selected
- **일부 인스턴스 선택 시**: Status changes to `Degraded - In Progress`[T_57]
How to use

## 사용 방법

The steps to restart a database are as follows.

1. **OwlDB console screen > Dashboard** Navigate to the menu.
2. Select the database to restart.
3. **Restart** Click the button. **Overview > Instances tab** or **Overview > Instance details** You can also restart from the menu.
4. `{instance alias}`When the "Do you want to restart?" modal appears, **Instance to restart** Select the instance to restart from the dropdown. In a single-instance configuration, the dropdown is disabled.
5. **Shutdown mode** Select the database shutdown mode from the dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly halts all operations currently in progress, rolls back all in-progress transactions, and then shuts down | **Default** |
| ABORT | Forcibly Terminating the Tibero Process |   |
| ABNORMAL | Forcibly Terminating the Server Process Without Connecting to the Tibero Server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Because some system resources (shared memory, semaphores, etc.) are not released, there is a possibility of corruption recovery, so its use is recommended only in the following situations. When a normal shutdown is not possible due to an internal Tibero error When Tibero must be shut down immediately due to an H/W problem When Tibero must be shut down immediately due to an emergency such as hacking
- **ABNORMAL**: The server is shut down immediately by an OS forced-termination signal, and because system resources (shared memory, semaphores, etc.) are not released, a corruption recovery process is required after restart, so its use is recommended only in the following situations. When a normal shutdown is not possible due to an internal Tibero error When a shutdown command has been issued in a shutdown mode other than ABNORMAL but is delayed and an immediate forced shutdown is required When a problem has occurred due to external factors such as H/W or OS and the execution of a shutdown mode other than ABNORMAL has failed When Tibero must be shut down immediately due to an emergency such as hacking
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| FAST | Rolls back transactions in progress, terminates connected sessions, and then quickly shuts down the server | **Default value** |
| IMMEDIATE | Immediately halts all operations, forcibly rolls back transactions in progress, and then shuts down the server |   |

{% hint style="warning" %}
**Caution**

**IMMEDIATE** When shutting down in this mode, some system resources may not be cleaned up properly, so it runs in automatic recovery mode. Its use is recommended only in the following cases.

- When a normal shutdown is not possible due to an internal OpenSQL error
- When a FAST mode shutdown does not respond for a certain period of time
- When an emergency shutdown is required due to an external factor such as H/W or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. **Restart** Click the button.

## Checking the Results

Once the restart operation begins, you can check the progress status by clicking the notification icon in the upper-right corner of the console screen. After the restart is complete, verify that the Status of the DB Service has changed to normal.

{% hint style="warning" %}
**Caution**

If the Status Code in the banner after the restart is `Issue: VM Down`, depending on the CSP you are using, **AWS** [aws_owldb_support@tibero.com](mailto:aws_owldb_support@tibero.com) or **Azure** [azure_owldb_support@tibero.com](mailto:azure_owldb_support@tibero.com) please contact.
{% endhint %}
