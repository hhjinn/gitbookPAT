OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and when the Primary (Tibero) or Leader (OpenSQL) DB is `Unavailable` in this state continuously for 30 seconds, failover is automatically performed. In a Tibero TAC configuration, a failure is determined when all Primary nodes are `Unavailable` in this state.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly across tables and scenarios are `Primary/Leader`, `Standby/Replica`noted together as shown.
{% endhint %}

## Stage-by-Stage Notification Policy

<table data-full-width="true"><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution Point</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Completed</li><li>Failed</li></ul></td><td><ul><li>Completed</li><li>Failed</li></ul></td></tr><tr><td>Transition History Management</td><td>Display promotion success status in the result column<ul><li>Success / Failure</li><li>Determine success based on completion of Standby/Replica promotion to Primary/Leader</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failure</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failure after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>When both fail, display separated by comma</li></ul></td></tr></tbody></table>

## Summary of Failover Automation Actions by Stage

<table data-full-width="true"><thead><tr><th>Automation Level</th><th>Automatic Failover</th><th>Automatic Configuration Normalization</th><th>Additional Actions</th></tr></thead><tbody><tr><td>Stage 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>The user <code>역할전환</code> performs promotion and normalization directly using the button</td></tr><tr><td>Stage 1 (<strong>Automatic Failover</strong>)</td><td>✓</td><td>△<ul><li>In a TAC configuration, the node count is recovered via Scale-Out</li><li>New Standby/Replica is not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>old Primary/Leader is <code>retired</code> handled</td></tr><tr><td>Stage 2 (<strong>Automatic Configuration Recovery</strong>)</td><td>—</td><td>—</td><td>Currently not supported (as of OwlDB v1.3)</td></tr><tr><td>Stage 3 (<strong>Full Automation</strong>)</td><td>✓</td><td>✓ old Primary/Leader reverse synchronization and automatic creation of new Standby/Replica</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-Out is performed for configuration recovery, and the new Primary cluster is also recovered as a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

If the existing Primary/Leader terminates abnormally during the automatic failover process, there may be logs that were not transmitted to the Standby/Replica. Because data consistency cannot be guaranteed and to prevent the Split-Brain phenomenon, the affected instance is **retired** handled in this state.
{% endhint %}

{% hint style="warning" %}
**Caution**

Failover automation level **Level 1**When it is Level 1, if there is only one Standby, that Standby is promoted to Primary and there may be no Standby after failover. In this case, the high availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Automation coverage by topology

The automation level provided differs depending on the engine and topology.

| Automation Level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Level 0 (**Manual**) | ✓ | ✓ | ✓ |
| Level 1 (**Automatic Failover**) | ✓ | ✓ | — |
| Level 2 (**Automatic Configuration Recovery**) | — | — | — |
| Level 3 (**Full Automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Level 1 (Automatic Failover)** is provided only in Tibero (Single+DR, TAC+DR), and OpenSQL does not support it.
- **Level 3 (Full Automation)** is provided in Tibero Single+DR and OpenSQL HA, **Tibero TAC+DR does not support it.**
- **Level 2 (Automatic Configuration Recovery)** is currently not provided in any topology.
{% endhint %}

## Detailed scenarios by automation level

{% hint style="info" %}
**Note**

**Note — Criteria for determining topology status**

The configuration normalization status after promotion is determined based on whether it has the same form as the topology method.

1. **Tibero TAC configuration** : Verifies whether Scale-out is performed after Failover to maintain the TAC structure. When there are 2 or more Primary Nodes: `Running` When there are fewer than 2 Primary Nodes: `Degraded`
2. **DR / HA configuration (Tibero DR · OpenSQL HA)** : Verifies whether it has a Standby/Replica. When there is 1 or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it may be `Degraded`) When there are 0 Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual

**Applies to** : Tibero Single+DR · TAC+DR, OpenSQL HA

No automatic action is performed, and when a failure occurs, the user `역할전환` directly promotes the Standby/Replica to Primary/Leader using the button. Configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic Failover

**Applies to** : Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and no new Standby is created.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby promotion</td><td>Promotes the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. new Primary configuration change</td><td>Performs Scale Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary handling</td><td>Marked as Retired, then the instance is terminated</td><td>-</td><td></td></tr><tr><td>5. Completion</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Stage 2: Automatic configuration recovery

**Currently not supported** (as of OwlDB v1.3). It is defined as the stage that restores the high-availability configuration by automatically creating a new Standby/Replica after automatic failover, but in the current version it is not provided in any topology.

### Stage 3: Full automation

**Applicability** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

Handles everything automatically, from failover to reverse synchronization of the old Primary/Leader and creation of a new Standby/Replica. Since it applies only to single Primary/Leader topologies, the TAC Scale Out process is not included.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempt to restart Old Primary/Leader</td><td>Attempt to restart the DB after restarting the instance (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica that reflects the most recent logs to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. Switch to new Primary/Leader</td><td>Start service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available → "<strong>Auto Failover complete</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Attempt reverse synchronization</td><td><ul><li>Restart <strong>Success</strong>On success, reconnect the Old Primary/Leader as a Standby/Replica (→ step 8)</li><li>Restart<strong>Failure</strong> On failure, delete the corresponding instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Create new Standby/Replica</td><td>Create a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Completion</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
