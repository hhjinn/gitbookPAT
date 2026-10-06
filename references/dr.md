OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and when the Primary (Tibero) or Leader (OpenSQL) DB is in the `Unavailable` status for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are in the `Unavailable` status.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly to both tables and scenarios are `Primary/Leader`, `Standby/Replica`noted together as follows.
{% endhint %}

## Step-by-Step Notification Policy <a href="#notification-policy-by-level" id="notification-policy-by-level"></a>

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution Time</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Complete</li><li>Failure</li></ul></td><td><ul><li>Complete</li><li>Failure</li></ul></td></tr><tr><td>Transition History Management</td><td>Displays whether promotion succeeded in the result column<ul><li>Success / Failure</li><li>Determines success based on completion of the Primary/Leader promotion of the Standby/Replica</li></ul></td><td>Displayed in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>Displayed with a comma when both fail</li></ul></td></tr></tbody></table>

## Summary of Failover Automation Steps by Stage <a href="#automation-level-summary" id="automation-level-summary"></a>

<table><thead><tr><th>Automation Level</th><th>Automatic Failover</th><th>Automatic Configuration Normalization</th><th>Additional Actions</th></tr></thead><tbody><tr><td>Stage 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>The user <code>Role Switching</code> performs promotion and normalization directly with the button</td></tr><tr><td>Stage 1 (<strong>Automatic Failover</strong>)</td><td>✓</td><td>△<ul><li>TAC configuration recovers the node count via Scale-Out</li><li>New Standby/Replica not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>old Primary/Leader is <code>retired</code> processed</td></tr><tr><td>Stage 2 (<strong>Automatic Configuration Recovery</strong>)</td><td>—</td><td>—</td><td>Currently not supported (as of OwlDB v1.3)</td></tr><tr><td>Stage 3 (<strong>Full Automation</strong>)</td><td>✓</td><td>✓ Automatic de-synchronization of old Primary/Leader and automatic creation of new Standby/Replica</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is a TAC, Scale-out is performed to recover the configuration so that the new Primary cluster is also restored to a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

If the existing Primary/Leader terminates abnormally during the automatic failover process, there may be logs that were not transmitted to the Standby/Replica. To ensure data consistency and prevent the Split-Brain phenomenon, the affected instance is **retired** processed to this state.
{% endhint %}

{% hint style="warning" %}
**Caution**

When the failover automation level is **Level 1**, if there is only one Standby, that Standby is promoted to Primary, so there may be no Standby after failover. In this case, the high-availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Scope of Automation Provided by Topology <a href="#automation-scope-by-topology" id="automation-scope-by-topology"></a>

The automation level provided differs depending on the engine and topology.

| Automation Level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Stage 0 (**Manual**) | ✓ | ✓ | ✓ |
| Stage 1 (**Automatic Failover**) | ✓ | ✓ | — |
| Stage 2 (**Automatic Configuration Recovery**) | — | — | — |
| Stage 3 (**Full Automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Level 1 (Automatic Failover)** is provided only in Tibero (Single+DR, TAC+DR), and OpenSQL is not supported.
- **Level 3 (Full Automation)** is provided in Tibero Single+DR and OpenSQL HA, and **Tibero TAC+DR is not supported.**
- **Level 2 (Automatic Configuration Recovery)** is currently not provided in any topology.
{% endhint %}

## Detailed Scenarios by Automation Level <a href="#automation-scenarios" id="automation-scenarios"></a>

{% hint style="info" %}
**Note**

**Note — Topology Status Determination Criteria**

The configuration normalization status after promotion is determined based on whether it has the same form as the topology method.

1. **Tibero TAC Configuration** : Checks whether the TAC structure is maintained by performing Scale-out even after Failover. When there are 2 or more Primary Nodes: `Running` When there are fewer than 2 Primary Nodes: `Degraded`
2. **DR / HA Configuration (Tibero DR · OpenSQL HA)** : Checks whether a Standby/Replica is held. When there is 1 or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it may be `Degraded`) When there are 0 Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual <a href="#id-0" id="id-0"></a>

**Apply** : Tibero Single+DR · TAC+DR, OpenSQL HA

No automatic action is performed, and when a failure occurs, the user directly `Role Switching` promotes the Standby/Replica to Primary/Leader using the button. The configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic Failover <a href="#id-1" id="id-1"></a>

**Apply** : Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and no new Standby is created.

<table><thead><tr><th>Step</th><th>Main Actions</th><th>Status</th><th>System Notifications</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Sending Auto Failover Request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover Started</strong></li><li>When the request fails, "<strong>Auto Failover Request Failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promotes the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. New Primary Configuration Change</td><td>Performs Scale Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>New Primary DB available → "<strong>Auto Failover Completed</strong>"</li><li>When failed, "<strong>Auto Failover Failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Processing</td><td>Marks as Retired status and then terminates the instance</td><td>-</td><td></td></tr><tr><td>5. Completion</td><td>Configuration Normalization Completed</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration Normalization Completed</strong>"</li><li>When failed, "<strong>Configuration Normalization Failed</strong>"</li></ul></td></tr></tbody></table>

### Level 2: Automatic Configuration Recovery <a href="#id-2" id="id-2"></a>

**Currently Not Supported** (as of OwlDB v1.3). It is defined as the step that recovers the high-availability configuration by automatically creating a new Standby/Replica after automatic failover, but in the current version it is not provided in any topology.

### Level 3: Full Automation <a href="#id-3" id="id-3"></a>

**Apply** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

Automatically handles everything from failover to de-synchronization of the old Primary/Leader and creation of a new Standby/Replica. It applies only to single Primary/Leader topologies, so the TAC Scale Out process is not included.

<table><thead><tr><th>Step</th><th>Main Actions</th><th>Status</th><th>System Notifications</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Sending Auto Failover Request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover Started</strong></li><li>When the request fails, "<strong>Auto Failover Request Failed</strong>"</li></ul></td></tr><tr><td>2. Old Primary/Leader Restart Attempt</td><td>Attempts DB restart after restarting the instance (deletes if it fails)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promotes the Standby/Replica that reflects the most recent log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. New Primary/Leader Switchover</td><td>Starts the service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>New Primary/Leader available → "<strong>Auto Failover Completed</strong>"</li><li>When failed, "<strong>Auto Failover Failed</strong>"</li></ul></td></tr><tr><td>5. De-synchronization Attempt</td><td><ul><li>On restart <strong>Success</strong>, reconnects the Old Primary/Leader as a Standby/Replica (→ No. 8)</li><li>On restart<strong>Failure</strong> On failure, deletes the affected instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. New Standby/Replica Creation</td><td>Creates a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and Synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Complete</td><td>Configuration Normalization Completed</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration Normalization Completed</strong>"</li><li>When failed, "<strong>Configuration Normalization Failed</strong>"</li></ul></td></tr></tbody></table>
