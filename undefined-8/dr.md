OwlDB continuously monitors the state of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and when the Primary (Tibero) or Leader (OpenSQL) DB `Unavailable` remains in this state for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are `Unavailable` in this state.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly across tables and scenarios are written together as `Primary/Leader`, `Standby/Replica`in this manner.
{% endhint %}

## Stage-by-stage notification policy

<table data-full-width="true"><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution point</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Completed</li><li>Failed</li></ul></td><td><ul><li>Completed</li><li>Failed</li></ul></td></tr><tr><td>Transition history management</td><td>Displays whether the promotion succeeded in the result column<ul><li>Success / Failure</li><li>Determines success based on the completion of the Standby/Replica promotion to Primary/Leader</li></ul></td><td>Displayed in the cause/remarks column<ul><li>Blank when everything succeeds</li><li><strong>Promotion failure</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failure after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>When both fail, they are displayed separated by a comma</li></ul></td></tr></tbody></table>

## Summary of failover automation behavior by stage

<table data-full-width="true"><thead><tr><th>Automation level</th><th>Automatic Failover</th><th>Automatic configuration normalization</th><th>Additional behavior</th></tr></thead><tbody><tr><td>Stage 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>The user <code>역할전환</code> performs promotion and normalization directly using the button</td></tr><tr><td>Stage 1 (<strong>Automatic failover</strong>)</td><td>✓</td><td>△<ul><li>For TAC configurations, the number of nodes is recovered via Scale-Out</li><li>New Standby/Replica not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>The old Primary/Leader is <code>retired</code> handled</td></tr><tr><td>Stage 2 (<strong>Automatic configuration recovery</strong>)</td><td>—</td><td>—</td><td>Currently not supported (as of OwlDB v1.3)</td></tr><tr><td>Stage 3 (<strong>Full automation</strong>)</td><td>✓</td><td>✓ Reverse synchronization of the old Primary/Leader and automatic creation of a new Standby/Replica</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-out is performed for configuration recovery so that the new Primary cluster is also recovered as a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

If the existing Primary/Leader terminates abnormally during the automatic failover process, there may be logs that were not transmitted to the Standby/Replica. Because data consistency cannot be guaranteed and to prevent the Split-Brain phenomenon, the corresponding instance is **retired** handled in this state.
{% endhint %}

{% hint style="warning" %}
**Caution**

Failover automation level **Level 1**When there is only one Standby, that Standby is promoted to Primary, which may leave no Standby after the failover. In this case, the high-availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Automation scope by topology

The automation level provided differs depending on the engine and topology.

| Automation level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Level 0 (**Manual**) | ✓ | ✓ | ✓ |
| Level 1 (**Automatic failover**) | ✓ | ✓ | — |
| Level 2 (**Automatic configuration recovery**) | — | — | — |
| Level 3 (**Full automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Level 1 (Automatic failover)** is provided only in Tibero (Single+DR, TAC+DR), and OpenSQL does not support it.
- **Level 3 (Full automation)** is provided in Tibero Single+DR and OpenSQL HA, **Tibero TAC+DR does not support it.**
- **Level 2 (Automatic configuration recovery)** is currently not provided in any topology.
{% endhint %}

## Detailed scenarios by automation level

{% hint style="info" %}
**Note**

**Note — Topology state determination criteria**

The configuration normalization state after promotion is determined based on whether it has the same form as the topology method.

1. **Tibero TAC configuration** : Checks whether Scale-out is performed even after Failover to maintain the TAC structure. When there are two or more Primary Nodes: `Running` When there are fewer than two Primary Nodes: `Degraded`
2. **DR / HA configuration (Tibero DR · OpenSQL HA)** : Checks whether a Standby/Replica is held. When there is one or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it `Degraded`may be the case) When there are zero Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual

**Applies to** : Tibero Single+DR · TAC+DR, OpenSQL HA

Does not perform automatic actions, and when a failure occurs the user `역할전환` directly promotes the Standby/Replica to Primary/Leader using the button. The user also manually proceeds with configuration normalization after promotion.

### Level 1: Automatic failover

**Applies to** : Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and a new Standby is not created.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>When the request fails, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby promotion</td><td>Promotes the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. new Primary configuration change</td><td>Performs Scale Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover completed</strong>"</li><li>When it fails, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary handling</td><td>Marks it as Retired status and then terminates the instance</td><td>-</td><td></td></tr><tr><td>5. Complete</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Stage 2: Automatic configuration recovery

**Currently not supported** (as of OwlDB v1.3). It is defined as the stage that restores the high-availability configuration by automatically creating a new Standby/Replica after automatic failover, but in the current version it is not provided for any topology.

### Stage 3: Full automation

**Applies to** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

Automatically handles everything from failover to reverse synchronization of the old Primary/Leader and creation of a new Standby/Replica. Since it applies only to single Primary/Leader topologies, the TAC Scale Out process is not included.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempt to restart Old Primary/Leader</td><td>Attempt to restart the DB after restarting the instance (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica that reflects the most recent logs to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. Switch to new Primary/Leader</td><td>Start service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available → "<strong>Auto Failover complete</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Attempt reverse synchronization</td><td><ul><li>Restart <strong>success</strong>On success, reconnect the Old Primary/Leader as a Standby/Replica (→ No. 8)</li><li>Restart<strong>failure</strong> On failure, delete the instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Create new Standby/Replica</td><td>Create a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Complete</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
