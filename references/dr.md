OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failure situations. The system performs a health check every second, and when the Primary (Tibero) or Leader (OpenSQL) DB is `Unavailable` in the status for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are `Unavailable` in the status.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly in the tables and scenarios are `Primary/Leader`, `Standby/Replica`noted together as follows.
{% endhint %}

## Step-by-step notification policy <a href="#notification-policy-by-level" id="notification-policy-by-level"></a>

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution point</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td><ul><li>Start</li><li>Request failed</li><li>Complete</li><li>Failure</li></ul></td><td><ul><li>Complete</li><li>Failure</li></ul></td></tr><tr><td>Switchover history management</td><td>Display whether promotion succeeded in the result column<ul><li>Success / Failure</li><li>Determine the completion of Standby/Replica promotion to Primary/Leader as the success criterion</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed</li><li>Primary scale out failed</li><li>New standby/replica creation failed</li><li>Displayed with a comma when both fail</li></ul></td></tr></tbody></table>

## Summary of step-by-step failover automation behavior <a href="#automation-level-summary" id="automation-level-summary"></a>

<table><thead><tr><th>Automation level</th><th>Automatic Failover</th><th>Automatic configuration normalization</th><th>Additional actions</th></tr></thead><tbody><tr><td>Step 0 (<strong>Manual</strong>)</td><td>❌</td><td>❌</td><td>The user <code>Role Switching</code> performs promotion and normalization directly using the button</td></tr><tr><td>Step 1 (<strong>Automatic failover</strong>)</td><td>✓</td><td>△<ul><li>In a TAC configuration, the number of nodes is restored via Scale-Out</li><li>New Standby/Replica not created</li><li>DR not recovered (<code>Degraded</code>)</li></ul></td><td>the old Primary/Leader is <code>retired</code> processed</td></tr><tr><td>Step 2 (<strong>Automatic configuration recovery</strong>)</td><td>—</td><td>—</td><td>Currently unsupported (as of OwlDB v1.3)</td></tr><tr><td>Step 3 (<strong>Full automation</strong>)</td><td>✓</td><td>✓ Desync of old Primary/Leader and automatic creation of new Standby/Replica</td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is a TAC, Scale-out is performed to recover the configuration, so the new Primary cluster is also recovered to a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="info" %}
**Note**

During the automatic failover process, if the existing Primary/Leader terminates abnormally, there may be logs that were not transmitted to the Standby/Replica. To prevent the Split-Brain phenomenon and because data consistency cannot be guaranteed, the instance is **retired** handled in the state.
{% endhint %}

{% hint style="warning" %}
**Caution**

The failover automation level is **Level 1**When it is set and there is only one Standby, that Standby is promoted to Primary, so there may be no Standby after failover. In this case, the high-availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Automation coverage by topology <a href="#automation-scope-by-topology" id="automation-scope-by-topology"></a>

The automation level provided differs depending on the engine and topology.

| Automation level | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| Step 0 (**Manual**) | ✓ | ✓ | ✓ |
| Step 1 (**Automatic failover**) | ✓ | ✓ | — |
| Step 2 (**Automatic configuration recovery**) | — | — | — |
| Step 3 (**Full automation**) | ✓ | — | ✓ |

{% hint style="info" %}
**Note**

- **Level 1 (Automatic failover)** is provided only in Tibero (Single+DR, TAC+DR), and OpenSQL is not supported.
- **Level 3 (Full automation)** is provided in Tibero Single+DR and OpenSQL HA, **Tibero TAC+DR is not supported.**
- **Level 2 (Automatic configuration recovery)** is currently not provided in any topology.
{% endhint %}

## Detailed scenarios by automation level <a href="#automation-scenarios" id="automation-scenarios"></a>

{% hint style="info" %}
**Note**

**Note — Criteria for determining topology status**

The configuration normalization status after promotion is determined based on whether it has the same form as the topology method.

1. **Tibero TAC configuration** : Checks whether the TAC structure is maintained by performing Scale-out even after Failover. When there are two or more Primary Nodes: `Running` When there are fewer than two Primary Nodes: `Degraded`
2. **DR / HA configuration (Tibero DR · OpenSQL HA)** : Checks whether a Standby/Replica is held. When there is one or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it `Degraded`may be) When there are zero Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual <a href="#id-0" id="id-0"></a>

**Apply** : Tibero Single+DR · TAC+DR, OpenSQL HA

No automatic actions are performed, and when a failure occurs, the user `Role Switching` directly promotes the Standby/Replica to Primary/Leader using the button. The configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic failover <a href="#id-1" id="id-1"></a>

**Apply** : Tibero Single+DR · TAC+DR (OpenSQL not supported)

The Old Primary is terminated without restarting, and no new Standby is created.

<table><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Sending Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>When the request fails, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby promotion</td><td>Promoting the Standby that reflects the latest log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. New Primary configuration change</td><td>Performing Scale Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>New Primary DB available → "<strong>Auto Failover complete</strong>"</li><li>When it fails, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary handling</td><td>Displaying as Retired state, then terminating the instance</td><td>-</td><td></td></tr><tr><td>5. Complete</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>When it fails, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Level 2: Automatic configuration recovery <a href="#id-2" id="id-2"></a>

**Currently not supported** (as of OwlDB v1.3). It is defined as the step that automatically creates a new Standby/Replica after automatic failover to recover the high-availability configuration, but in the current version it is not provided in any topology.

### Level 3: Full automation <a href="#id-3" id="id-3"></a>

**Apply** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR not supported)

It automatically handles everything from failover to desync of old Primary/Leader and creation of new Standby/Replica. Since it applies only to single Primary/Leader topologies, the TAC Scale Out process is not included.

<table><thead><tr><th>Step</th><th>Main action</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Sending Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>When the request fails, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempting to restart Old Primary/Leader</td><td>Attempting DB restart after instance restart (deleted on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promoting the Standby/Replica that reflects the latest log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. New Primary/Leader switchover</td><td>Starting service with the promoted node as the new Primary/Leader</td><td><code>Updating</code></td><td><ul><li>New Primary/Leader available → "<strong>Auto Failover complete</strong>"</li><li>When it fails, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Desync attempt</td><td><ul><li>Restart <strong>Success</strong>On restart, reconnecting Old Primary/Leader as Standby/Replica (→ No. 8)</li><li>Restart<strong>Failure</strong> On this, deleting the instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Creating new Standby/Replica</td><td>Creating a new Standby/Replica with the same specifications</td><td>-</td><td></td></tr><tr><td>7. Connection and synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Complete</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>When it fails, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
