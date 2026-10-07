OwlDB can restart a database in a single-instance or multi-instance configuration.

{% hint style="warning" %}
**Caution**

When restarting the database, all currently connected sessions are terminated. It may take several minutes before it becomes active again.
{% endhint %}

### Menu Path <a href="#undefined" id="undefined"></a>

The database restart feature can be accessed from the following menu paths.

- **OwlDB Console Screen > Dashboard**
- **OwlDB Console Screen > Overview > Instances tab**
- **OwlDB Console Screen > Overview > Instance Details**

### Restart Button <a href="#undefined-1" id="undefined-1"></a>

The **Restart** button is enabled according to the Status of the DB Service. The detailed enablement and selection conditions based on Status and Health state are described below in 'Restart Conditions'.

### Restart Options <a href="#undefined-2" id="undefined-2"></a>

Clicking the **Restart** button displays the restart options modal.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Title</td><td>Do you want to restart <code>{instance alias}</code>?</td></tr><tr><td>Content</td><td><ul><li>Terminate all currently connected sessions</li><li>Generally takes a few minutes; DB Service is reactivated after completion</li></ul></td></tr><tr><td>Instance to Restart</td><td><ul><li>Select the instance to restart from the dropdown</li><li>Selection is disabled for single-instance configurations</li></ul></td></tr><tr><td>Shutdown Mode</td><td>Select the database shutdown mode from the dropdown</td></tr></tbody></table>

## How It Works <a href="#how-it-works" id="how-it-works"></a>

Database restart behaves differently depending on the status of the selected instance.

### Restart Conditions <a href="#undefined-3" id="undefined-3"></a>

The **Restart** button is enabled when the Status of the DB Service is one of `Running`, `Degraded`, `Down`, `Updating`, or `Failover`. However, the list of instances selectable in the modal is limited based on the Health state of the instances.

| Status | Selectable Instances |
| --- | --- |
| `Running` | All instances in the `Available` state |
| `Degraded` | Instances in the `Available` or `Limited` state |
| `Down` | All instances in the `Unavailable` state |
| `Updating` | Instances in the `Available` state |
| `Failover` | Instances in the `Available` state |

The **Restart** button is disabled in the following Status states.

- `Provisioning`
- `Stopped`
- `Terminating`
- `Retired` (In this case, it is displayed as a **Delete** button on the instance details page instead.)

### Status Changes After Restart <a href="#undefined-4" id="undefined-4"></a>

- **When all instances are selected**: Status changes to `Updating - In Progress`
- **When some instances are selected**: Status changes to `Degraded - In Progress`

## How to Use <a href="#how-to-use" id="how-to-use"></a>

The procedure for restarting a database is as follows.

1. Navigate to the **OwlDB Console Screen > Dashboard** menu.
2. Select the database to restart.
3. Click the **Restart** button. You can also restart from the **Overview > Instances tab** or **Overview > Instance Details** menu.
4. When the "Do you want to restart `{instance alias}`?" modal appears, select the instance to restart from the **Instance to Restart** dropdown. For single-instance configurations, this dropdown is disabled.
5. Select the database shutdown mode from the **Shutdown Mode** dropdown.

{% tabs %}
{% tab title="Tibero" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| IMMEDIATE | Forcibly stops all currently running operations, rolls back all in-progress transactions, and then shuts down | **Default** |
| ABORT | Forcibly terminates the Tibero processes |   |
| ABNORMAL | Forcibly terminates the server processes without connecting to the Tibero server |   |

{% hint style="warning" %}
**Caution**

- **ABORT**: Since some system resources (shared memory, semaphores, etc.) are not released, there is a possibility of corruption recovery, so it is recommended to use this only in the following situations. When normal shutdown is not possible due to an internal Tibero error When Tibero must be shut down immediately due to a hardware problem When Tibero must be shut down immediately due to an emergency such as a hacking incident
- **ABNORMAL**: The server is shut down immediately with an OS forced termination signal, and since system resources (shared memory, semaphores, etc.) are not released, a corruption recovery process is required after restart, so it is recommended to use this only in the following situations. When normal shutdown is not possible due to an internal Tibero error When a shutdown command issued with a shutdown mode other than ABNORMAL is delayed and must be forcibly shut down immediately When a problem occurs due to external factors such as hardware or OS, and execution of a shutdown mode other than ABNORMAL fails When Tibero must be shut down immediately due to an emergency such as a hacking incident
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
| Shutdown Mode | Description | Remarks |
| --- | --- | --- |
| FAST | Rolls back in-progress transactions, terminates connected sessions, and then shuts down the server quickly | **Default** |
| IMMEDIATE | Immediately stops all operations, forcibly rolls back in-progress transactions, and then shuts down the server |   |

{% hint style="warning" %}
**Caution**

When shutting down in **IMMEDIATE** mode, some system resources may not be cleaned up properly, so it runs in automatic recovery mode. It is recommended to use this only in the following cases.

- When normal shutdown is not possible due to an internal OpenSQL error
- When FAST mode shutdown does not respond for a certain period of time
- When an emergency shutdown is required due to external factors such as hardware or OS
{% endhint %}
{% endtab %}
{% endtabs %}

6. Click the **Restart** button.

## Checking the Result <a href="#check-results" id="check-results"></a>

Once the restart operation begins, you can check the progress status by clicking the notification icon in the upper right of the console screen. After the restart is complete, verify that the Status of the DB Service has changed normally.

{% hint style="warning" %}
**Caution**

If the Status Code of the banner is `Issue: VM Down` after restart, please contact **AWS** [aws_owldb_support@tibero.com](mailto:aws_owldb_support@tibero.com) or **Azure** [azure_owldb_support@tibero.com](mailto:azure_owldb_support@tibero.com) depending on the CSP you are using.
{% endhint %}
