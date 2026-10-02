OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and when the Primary (Tibero) or Leader (OpenSQL) DB is `Unavailable` in the status for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, when all Primary nodes are `Unavailable` in the status, it is determined as a failure.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly in the tables and scenarios are `Primary/Leader`, `Standby/Replica`noted together as follows.
{% endhint %}

## Step-by-Step Notification Policy

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution Point</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Complete</li><li>Failure</li></ul></td><td><ul><li>Complete</li><li>Failure</li></ul></td></tr><tr><td>Transition History Management</td><td>Display whether the promotion succeeded in the result column<ul><li>Success / Failure</li><li>Determine the completion of Standby/Replica promotion to Primary/Leader as the success criterion</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>When both fail, display separated by a comma</li></ul></td></tr></tbody></table>

## Summary of Failover Automation Step-by-Step Actions

<table><thead><tr><th>Automation Level</th><th>Automatic Failover</th><th>Automatic Configuration Normalization</th><th>Additional Actions</th></tr></thead><tbody><tr><td>Step 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>The user <code>역할전환</code> performs promotion and normalization directly using the button</td></tr><tr><td>Step 1 (<strong>Automatic Failover</strong>)</td><td>✓</td><td>△<ul><li>TAC configuration recovers the number of nodes with Scale-Out</li><li>New Standby/Replica not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>old Primary/Leader <code>retired</code> processed</td></tr><tr><td>Step 2 (<strong>Automatic Configuration Recovery</strong>)</td><td>—</td><td>—</td><td>Currently not supported (as of OwlDB v1.3)</td></tr><tr><td>Step 3 (<strong>Full Automation</strong>)</td><td>✓</td><td>✓ old Primary/Leader reverse synchronization and new Standby/Replica automatic creation</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When the existing Primary cluster is TAC, Scale-out is performed to recover the configuration, and the new Primary cluster is also recovered to a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

If the existing Primary/Leader terminates abnormally during the automatic failover process, there may be logs that were not transmitted to the Standby/Replica. Since data consistency cannot be guaranteed and to prevent the Split-Brain phenomenon, the corresponding instance is **retired** processed with the status.
{% endhint %}

{% hint style="warning" %}
**Caution**

When the failover automation level is **Step 1**When there is only 1 Standby, that Standby is promoted to Primary, and there may be no Standby after the failover. In this case, caution is needed because the high availability configuration is not maintained. (Applies to Tibero)
{% endhint %}

## Automation Scope by Topology

The level of automation provided differs depending on the engine and topology.

| Automation Level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Step 0 (**Manual**) | ✓ | ✓ | ✓ |
| Step 1 (**Automatic Failover**) | ✓ | ✓ | — |
| Step 2 (**Automatic Configuration Recovery**) | — | — | — |
| Step 3 (**Full Automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Level 1 (Automatic Failover)** is provided only in Tibero (Single+DR, TAC+DR) and is not supported by OpenSQL.
- **Level 3 (Full Automation)** is provided in Tibero Single+DR and OpenSQL HA, **but Tibero TAC+DR is not supported.**
- **Level 2 (Automatic Configuration Recovery)** is currently not provided in any topology.
{% endhint %}

## Detailed Scenarios by Automation Level

{% hint style="info" %}
**Note**

**Note — Criteria for Determining Topology State**

The post-promotion configuration normalization state is determined based on whether it has the same form as the topology method.

1. **Tibero TAC Configuration** : Verifies whether Scale-out is performed even after Failover to maintain the TAC structure. When there are 2 or more Primary Nodes: `Running` When there are fewer than 2 Primary Nodes: `Degraded`
2. **DR / HA Configuration (Tibero DR · OpenSQL HA)** : Verifies whether a Standby/Replica is held. When there are 1 or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it `Degraded`may be) When there are 0 Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual

**Apply** : Tibero Single+DR · TAC+DR, OpenSQL HA

No automatic actions are performed, and in the event of a failure, the user `역할전환` directly promotes the Standby/Replica to Primary/Leader using the button. Configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic Failover

**Apply** : Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and no new Standby is created.

<table><thead><tr><th>Stage</th><th>Key Operations</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. new Primary Configuration Change</td><td>Perform Scale Out if it is a TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Handling</td><td>Mark as Retired status, then terminate the instance</td><td>-</td><td></td></tr><tr><td>5. Completion</td><td>Configuration normalization completed</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization completed</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Level 2: Automatic Configuration Recovery

**Currently not supported** (as of OwlDB v1.3). It is defined as the stage that restores the high availability configuration by automatically creating a new Standby/Replica after automatic failover, but it is not provided in any topology in the current version.

### Level 3: Full Automation

**Apply** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

Handles everything automatically, from failover to de-synchronization of the old Primary/Leader and creation of a new Standby/Replica. Since it applies only to single Primary/Leader topologies, the TAC Scale Out process is not included.

<table><thead><tr><th>Stage</th><th>Key Operations</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Old Primary/Leader Restart Attempt</td><td>Attempt to restart the DB after restarting the instance (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica that reflects the most recent log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. new Primary/Leader Transition</td><td>Start service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. De-synchronization Attempt</td><td><ul><li>On restart <strong>Success</strong>, reconnect the Old Primary/Leader as a Standby/Replica (→ No. 8)</li><li>On restart<strong>Failure</strong> On failure, delete the corresponding instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. New Standby/Replica Creation</td><td>Create a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and Synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Completion</td><td>Configuration normalization completed</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization completed</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
