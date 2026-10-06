OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failures and automatically perform failover. This page describes the automatic failover behavior and the policies for each automation level.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply in common are `Primary/Leader`, `Standby/Replica`noted together as follows.
{% endhint %}

## Failure detection <a href="#failure-detection" id="failure-detection"></a>

The system performs a health check every second, and if the Primary (Tibero) or Leader (OpenSQL) DB remains in the `Unavailable` state for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are in the `Unavailable` state.

## Step-by-step notification policy <a href="#notification-policy-by-level" id="notification-policy-by-level"></a>

<table><thead><tr><th>Category</th><th>Standby/Replica promotion</th><th>Configuration normalization</th></tr></thead><tbody><tr><td>Execution point</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td>Start / Request failed / Complete / Failed</td><td>Complete / Failed</td></tr><tr><td>Failover history management</td><td>Indicates whether promotion succeeded in the result column<ul><li>Success / Failure</li><li>Success is defined based on whether the Standby/Replica is promoted to become the new Primary/Leader</li></ul></td><td>Displayed in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failed</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failed after successful promotion</strong>: Cluster Normalization Failed — Primary scale out failed or New standby/replica creation failed</li></ul>(if both fail, they are shown separated by a comma)</td></tr></tbody></table>

## Summary of behavior by failover automation level <a href="#automation-level-summary" id="automation-level-summary"></a>

<table><thead><tr><th>Automation level</th><th>Automatic Failover</th><th>Automatic configuration normalization</th><th>Remarks</th></tr></thead><tbody><tr><td>Level 0 (<strong>Manual</strong>)</td><td>—</td><td>—</td><td>The user <code>Role Switch</code> performs promotion and normalization directly using the button</td></tr><tr><td>Level 1 (<strong>Automatic failover</strong>)</td><td>✓</td><td><ul><li>Recovers only the number of nodes via TAC Scale-Out</li><li>New Standby/Replica not created → DR not recovered (<code>Degraded</code>)</li></ul></td><td>The Old Primary/Leader is <code>retired</code> handled</td></tr><tr><td>Level 2 (<strong>Automatic configuration recovery</strong>)</td><td>✓</td><td><ul><li>TAC Scale-Out</li><li>New Standby/Replica automatically created → high-availability configuration recovered</li></ul></td><td>Old Primary/Leader <code>retired</code> handled</td></tr><tr><td>Level 3 (<strong>Full automation</strong>)</td><td>✓</td><td><ul><li>Old Primary/Leader reverse synchronization</li><li>New Standby/Replica automatically created</li></ul></td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-Out is performed to recover the configuration, and the new Primary cluster is also recovered as a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="warning" %}
**Caution**

- In the case of Levels 1 and 2, if the Primary/Leader Cluster terminates abnormally, there may be logs that were not transmitted to the Standby/Replica. Since data consistency cannot be guaranteed and to prevent the Split-Brain phenomenon, the corresponding instance is **retired** handled as the state
- The failover automation level is **Level 1**When set to this level, if there is only one Standby, that Standby is promoted to Primary, and there may be no Standby after failover. In this case, the high availability configuration is not maintained, so caution is required. (Applies to Tibero)
{% endhint %}

## Automation Scope by License Type <a href="#automation-scope-by-license" id="automation-scope-by-license"></a>

In a Cloud environment, the automation levels provided differ depending on the DB engine and license type (LI/BYOL).

| DB Type | LI | BYOL |
| --- | --- | --- |
| Tibero | Levels 0 to 3 (fully provided) | Levels 0, 2, 3 (**Level 1 not provided**) |
| OpenSQL | Levels 0, 3 (**Levels 1, 2 not provided**) | Levels 0, 3 (**Levels 1, 2 not provided**) |

## Detailed Scenarios by Automation Level <a href="#automation-scenarios" id="automation-scenarios"></a>

{% hint style="info" %}
**Note**

**Topology State Determination Criteria**

The configuration normalization state after promotion is determined based on whether it has the same form as the topology method.

- **Tibero TAC Configuration** : Checks whether Scale-Out is performed even after Failover to maintain the TAC structure. When there are 2 or more Primary Nodes: `Running` When there are fewer than 2 Primary Nodes: `Degraded`
- **DR / HA Configuration (Tibero DR · OpenSQL HA)** : Checks whether a Standby/Replica is held. When there is 1 or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it may be `Degraded`) When there are 0 Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual <a href="#id-0" id="id-0"></a>

**Apply** : Tibero LI · BYOL, OpenSQL LI · BYOL

No automatic action is performed, and when a failure occurs, the user `Role Switch` directly promotes the Standby/Replica to Primary/Leader using the button. The user also manually performs the configuration normalization after promotion.

### Level 1: Automatic Failover <a href="#id-1" id="id-1"></a>

**Apply** : Tibero LI (Tibero BYOL · OpenSQL not provided)

The Old Primary is terminated without restarting, and no new Standby is created.

<table><thead><tr><th>Level</th><th>Key Operations</th><th>Status</th><th>System Notifications</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby that reflects the latest log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. New Primary Configuration Change</td><td>Perform Scale-Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>New Primary DB available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Handling</td><td>Marked as Retired state → terminate instance</td><td>-</td><td></td></tr><tr><td>5. Completion</td><td>Configuration normalization completed</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization completed</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Level 2: Automatic Configuration Recovery <a href="#id-2" id="id-2"></a>

**Apply** : Tibero LI · BYOL (OpenSQL not provided)

Automatically creates a new Standby to recover the high availability configuration.

<table><thead><tr><th>Level</th><th>Key Operations</th><th>Status</th><th>System Notifications</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby that reflects the latest log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. New Primary Configuration Change</td><td>Perform Scale-Out in the case of a TAC configuration (including license transfer)</td><td><code>Updating</code></td><td><ul><li>New Primary DB available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Handling</td><td>Marked as Retired state → terminate instance</td><td>-</td><td></td></tr><tr><td>5. Create New Standby</td><td>Created with the same specifications in the AZ where the Old Primary is located</td><td>-</td><td></td></tr><tr><td>6. Standby Connection and Synchronization</td><td>Standby connection and synchronization</td><td>-</td><td></td></tr><tr><td>7. Completion</td><td>Configuration normalization completed</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization completed</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Level 3: Full Automation <a href="#id-3" id="id-3"></a>

**Apply** : Tibero LI · BYOL, OpenSQL LI · BYOL

Automatically handles the entire process from failover to Old Primary/Leader reverse synchronization and new Standby/Replica creation.

<table><thead><tr><th>Level</th><th>Key Operations</th><th>Status</th><th>System Notifications</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Attempt to Restart Old Primary/Leader</td><td>Restart instance → attempt DB restart (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica that reflects the latest log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. New Primary/Leader Configuration Change</td><td>Perform Scale-Out in the case of a Tibero TAC configuration</td><td><code>Updating</code></td><td><ul><li>New Primary/Leader available → "<strong>Auto Failover completed</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>5. Attempt Reverse Synchronization</td><td><ul><li>On restart <strong>Success</strong>, reconnect the Old Primary/Leader as a Standby/Replica → go to step 8</li><li>On restart<strong>Failure</strong> , delete the instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. Create New Standby/Replica</td><td>Created with the same specifications in the AZ where the Old Primary/Leader is located</td><td>-</td><td></td></tr><tr><td>7. Connection and Synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Completion</td><td>Configuration normalization completed</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization completed</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>
