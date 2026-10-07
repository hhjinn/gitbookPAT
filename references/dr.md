OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and if the Primary (Tibero) or Leader (OpenSQL) DB remains in the `Unavailable` state for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are in the `Unavailable` state.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby** corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly in tables and scenarios are written together as `Primary/Leader` and `Standby/Replica`.
{% endhint %}

## Step-by-Step Notification Policy <a href="#notification-policy-by-level" id="notification-policy-by-level"></a>

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution Timing</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Completed</li><li>Failure</li></ul></td><td><ul><li>Completed</li><li>Failure</li></ul></td></tr><tr><td>Transition History Management</td><td>Display whether promotion succeeded in the result column<ul><li>Success / Failure</li><li>Determine success based on the completion of the Standby/Replica's promotion to Primary/Leader</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>Displayed with a comma when both fail</li></ul></td></tr></tbody></table>

## Summary of Failover Automation Behavior by Step <a href="#automation-level-summary" id="automation-level-summary"></a>

<table><thead><tr><th>Automation level</th><th>Automatic Failover</th><th>Automatic configuration normalization</th><th>Additional actions</th></tr></thead><tbody><tr><td>Step 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>User performs promotion and normalization directly with the <code>Role Switch</code> button</td></tr><tr><td>Step 1 (<strong>Automatic Failover</strong>)</td><td>✓</td><td>△<ul><li>TAC configuration recovers the node count via Scale-Out</li><li>New Standby/Replica not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>old Primary/Leader processed as <code>retired</code></td></tr><tr><td>Step 2 (<strong>Automatic Configuration Recovery</strong>)</td><td>—</td><td>—</td><td>Currently not supported (as of OwlDB v1.3)</td></tr><tr><td>Step 3 (<strong>Full Automation</strong>)</td><td>✓</td><td>✓ old Primary/Leader reverse synchronization and automatic creation of new Standby/Replica</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-out is performed for configuration recovery so that the new Primary cluster is also recovered as a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

If the existing Primary/Leader terminates abnormally during the automatic failover process, there may be logs that were not transmitted to the Standby/Replica. Since data consistency cannot be guaranteed and to prevent the Split-Brain phenomenon, that instance is processed into the **retired** state.
{% endhint %}

{% hint style="warning" %}
**Caution**

When the failover automation level is **Step 1** and there is only one Standby, that Standby is promoted to Primary, which may leave no Standby after failover. In this case, caution is needed because the high-availability configuration is not maintained. (Applies to Tibero)
{% endhint %}

## Automation Coverage by Topology <a href="#automation-scope-by-topology" id="automation-scope-by-topology"></a>

The automation level provided differs depending on the engine and topology.

| Automation level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Step 0 (**Manual**) | ✓ | ✓ | ✓ |
| Step 1 (**Automatic Failover**) | ✓ | ✓ | — |
| Step 2 (**Automatic Configuration Recovery**) | — | — | — |
| Step 3 (**Full Automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Step 1 (Automatic Failover)** is provided only on Tibero (Single+DR, TAC+DR), and is not supported by OpenSQL.
- **Step 3 (Full Automation)** is provided on Tibero Single+DR and OpenSQL HA, and **Tibero TAC+DR is not supported.**
- **Step 2 (Automatic Configuration Recovery)** is currently not provided on any topology.
{% endhint %}

## Detailed Scenarios by Automation Step <a href="#automation-scenarios" id="automation-scenarios"></a>

{% hint style="info" %}
**Note**

**Note — Topology Status Determination Criteria**

The configuration normalization status after promotion is determined based on whether it has the same form as the topology method.

1. **Tibero TAC Configuration**: Checks whether Scale-out is performed even after Failover to maintain the TAC structure. When there are 2 or more Primary Nodes: `Running`. When there are fewer than 2 Primary Nodes: `Degraded`
2. **DR / HA Configuration (Tibero DR · OpenSQL HA)**: Checks whether it has a Standby/Replica. When there is 1 or more Standby/Replica: `Running` (however, if the Standby/Replica is in an abnormal state, it may be `Degraded`). When there are 0 Standby/Replica: `Degraded`
{% endhint %}

### Step 0: Manual <a href="#id-0" id="id-0"></a>

**Application** : Tibero Single+DR · TAC+DR, OpenSQL HA

No automatic action is performed; when a failure occurs, the user directly promotes the Standby/Replica to Primary/Leader using the `Role Switch` button. Configuration normalization after promotion is also performed manually by the user.

### Step 1: Automatic Failover <a href="#id-1" id="id-1"></a>

**Application**: Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and no new Standby is created.

<table><thead><tr><th>Step</th><th>Main Operation</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover start</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby reflecting the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. new Primary Configuration Change</td><td>Perform Scale Out in case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover complete</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Handling</td><td>Mark as Retired state, then terminate the instance</td><td>-</td><td></td></tr><tr><td>5. Complete</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Phase 2: Automatic Configuration Recovery <a href="#id-2" id="id-2"></a>

**Currently not supported** (as of OwlDB v1.3). It is defined as the phase that restores the high-availability configuration by automatically creating a new Standby/Replica after automatic failover, but it is not provided in any topology in the current version.

### Phase 3: Full Automation <a href="#id-3" id="id-3"></a>

**Applies to**: Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

It automatically handles everything from failover to reverse synchronization of the old Primary/Leader and creation of a new Standby/Replica. Since it applies only to a single Primary/Leader topology, the TAC Scale Out process is not included.

<table><thead><tr><th>Step</th><th>Main Operation</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover start</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempt to Restart Old Primary/Leader</td><td>Attempt DB restart after restarting the instance (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica reflecting the most recent log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. new Primary/Leader Switchover</td><td>Start service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available → "<strong>Auto Failover complete</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Reverse Synchronization Attempt</td><td><ul><li>On restart <strong>success</strong>, reconnect the Old Primary/Leader as a Standby/Replica (→ No. 8)</li><li>On restart <strong>failure</strong>, delete the corresponding instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Create New Standby/Replica</td><td>Create a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and Synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Complete</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
