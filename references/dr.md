OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failures and automatically perform failover. This page explains the automatic failover behavior and the policy for each automation level.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby** corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly are noted together as `Primary/Leader` and `Standby/Replica`.
{% endhint %}

## Failure detection <a href="#failure-detection" id="failure-detection"></a>

The system performs a health check every second, and if the Primary (Tibero) or Leader (OpenSQL) DB remains in the `Unavailable` state for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are in the `Unavailable` state.

## Step-by-step notification policy <a href="#notification-policy-by-level" id="notification-policy-by-level"></a>

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution time</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td>Started / Request failed / Completed / Failed</td><td>Completed / Failed</td></tr><tr><td>Transition history management</td><td>Display promotion success status in the result column<ul><li>Success / Failure</li><li>Success is defined based on whether the Standby/Replica is promoted and becomes the new Primary/Leader</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed — Primary scale out failed or New standby/replica creation failed</li></ul>(If both fail, they are shown separated by a comma)</td></tr></tbody></table>

## Summary of behavior by failover automation level <a href="#automation-level-summary" id="automation-level-summary"></a>

<table><thead><tr><th>Automation level</th><th>Automatic failover</th><th>Automatic configuration normalization</th><th>Remarks</th></tr></thead><tbody><tr><td>Level 0 (<strong>Manual</strong>)</td><td>—</td><td>—</td><td>The user performs promotion and normalization directly using the <code>Switch Role</code> button</td></tr><tr><td>Level 1 (<strong>Automatic failover</strong>)</td><td>✓</td><td><ul><li>Recovers only the number of nodes via TAC Scale-Out</li><li>New Standby/Replica not created → DR not recovered (<code>Degraded</code>)</li></ul></td><td>Marks the Old Primary/Leader as <code>retired</code></td></tr><tr><td>Level 2 (<strong>Automatic configuration recovery</strong>)</td><td>✓</td><td><ul><li>TAC Scale-Out</li><li>Automatically creates a new Standby/Replica → recovers high-availability configuration</li></ul></td><td>Marks the Old Primary/Leader as <code>retired</code></td></tr><tr><td>Level 3 (<strong>Full automation</strong>)</td><td>✓</td><td><ul><li>Reverse-synchronizes the Old Primary/Leader</li><li>Automatically creates a new Standby/Replica</li></ul></td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-Out is performed to recover the configuration, so the new Primary cluster is also recovered as a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="warning" %}
**Caution**

- In levels 1 and 2, if the Primary/Leader cluster terminates abnormally, there may be logs that were not transmitted to the Standby/Replica. To ensure data consistency and prevent the split-brain phenomenon, the corresponding instance is marked as **retired**.
- When the failover automation level is **Level 1** and there is only one Standby, that Standby is promoted to Primary, so there may be no Standby after failover. In this case, the high-availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Automation scope provided by license type <a href="#automation-scope-by-license" id="automation-scope-by-license"></a>

In a Cloud environment, the automation level provided differs depending on the DB engine and license type (LI/BYOL).

| DB Type | LI | BYOL |
| --- | --- | --- |
| Tibero | Levels 0 to 3 (fully provided) | Levels 0, 2, 3 (**Level 1 not provided**) |
| OpenSQL | Levels 0, 3 (**Levels 1, 2 not provided**) | Levels 0, 3 (**Levels 1, 2 not provided**) |

## Detailed scenarios by automation level <a href="#automation-scenarios" id="automation-scenarios"></a>

{% hint style="info" %}
**Note**

**Criteria for determining topology status**

The configuration normalization status after promotion is determined based on whether it has the same form as the topology method.

- **Tibero TAC configuration**: Checks whether Scale-Out is performed to maintain the TAC structure even after failover. When there are two or more Primary Nodes: `Running` When there are fewer than two Primary Nodes: `Degraded`
- **DR / HA configuration (Tibero DR · OpenSQL HA)**: Checks whether a Standby/Replica is held. When there is one or more Standby/Replica: `Running` (however, if the Standby/Replica is in an abnormal state, it may be `Degraded`) When there are zero Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual <a href="#id-0" id="id-0"></a>

**Application** : Tibero LI · BYOL, OpenSQL LI · BYOL

No automatic action is performed, and when a failure occurs, the user directly promotes the Standby/Replica to Primary/Leader using the `Switch Role` button. Configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic failover <a href="#id-1" id="id-1"></a>

**Application**: Tibero LI (Tibero BYOL · OpenSQL not provided)

The Old Primary is terminated without restarting, and a new Standby is not created.

<table><thead><tr><th>Level</th><th>Key operations</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby promotion</td><td>Promote the Standby reflecting the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. Change new Primary configuration</td><td>Perform Scale-Out if it is a TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover complete</strong>"</li><li>On failure "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Handle Old Primary</td><td>Mark as Retired → terminate instance</td><td>-</td><td></td></tr><tr><td>5. Complete</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Stage 2: Automatic configuration recovery <a href="#id-2" id="id-2"></a>

**Applies to** : Tibero LI · BYOL (OpenSQL not provided)

Automatically creates a new Standby to recover the high-availability configuration.

<table><thead><tr><th>Level</th><th>Key operations</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby promotion</td><td>Promote the Standby reflecting the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. Change new Primary configuration</td><td>Perform Scale-Out if it is a TAC configuration (including license transfer)</td><td><code>Updating</code></td><td><ul><li>new Primary DB available → "<strong>Auto Failover complete</strong>"</li><li>On failure "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Handle Old Primary</td><td>Mark as Retired → terminate instance</td><td>-</td><td></td></tr><tr><td>5. Create new Standby</td><td>Create with identical specifications in the AZ where the Old Primary is located</td><td>-</td><td></td></tr><tr><td>6. Standby connection and synchronization</td><td>Standby connection and synchronization</td><td>-</td><td></td></tr><tr><td>7. Complete</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Stage 3: Full automation <a href="#id-3" id="id-3"></a>

**Application** : Tibero LI · BYOL, OpenSQL LI · BYOL

Automatically handles the entire process from failover to Old Primary/Leader resynchronization and new Standby/Replica creation.

<table><thead><tr><th>Level</th><th>Key operations</th><th>Status</th><th>System notification</th></tr></thead><tbody><tr><td>1. Failure detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempt to restart Old Primary/Leader</td><td>Restart instance → attempt DB restart (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica reflecting the most recent log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. Change new Primary/Leader configuration</td><td>Perform Scale-Out if it is a Tibero TAC configuration</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available → "<strong>Auto Failover complete</strong>"</li><li>On failure "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Attempt resynchronization</td><td><ul><li>On restart <strong>success</strong>, reconnect Old Primary/Leader as Standby/Replica → go to step 8</li><li>On restart <strong>failure</strong>, delete the corresponding instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Create new Standby/Replica</td><td>Create with identical specifications in the AZ where the Old Primary/Leader is located</td><td>-</td><td></td></tr><tr><td>7. Connection and synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Complete</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
