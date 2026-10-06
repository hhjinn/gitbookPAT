OwlDB continuously monitors the status of the Primary database configured with DR (or HA) to detect failures and automatically perform failover. This page describes automatic failover behavior and policies by automation level.

{% hint style="info" %}
**Note**

In this document, Tibero's **Primary / Standby**corresponds to OpenSQL's **Leader / Replica**. Items that apply commonly are noted together as `Primary/Leader`, `Standby/Replica`.
{% endhint %}

## Failure Detection

The system performs a health check every second, and if the Primary (Tibero) or Leader (OpenSQL) DB remains in the `Unavailable` state for 30 seconds, failover is performed automatically. In a Tibero TAC configuration, a failure is determined when all Primary nodes are in the `Unavailable` state.

## Step-by-Step Notification Policy

<table data-full-width="true"><thead><tr><th>Category</th><th>Standby/Replica Promotion</th><th>Configuration Normalization</th></tr></thead><tbody><tr><td>Execution Time</td><td>Immediately after failure detection</td><td>After Standby/Replica promotion</td></tr><tr><td>Notification</td><td>Start / Request Failed / Complete / Failed</td><td>Complete / Failed</td></tr><tr><td>Transition History Management</td><td>Display promotion success or failure in the result column<ul><li>Success / Failure</li><li>Success is defined based on the Standby/Replica being promoted to become the new Primary/Leader</li></ul></td><td>Display in the cause/remarks column<ul><li>Blank when fully successful</li><li><strong>Promotion failure</strong>: Standby Promotion Failed</li><li><strong>Configuration normalization failure after successful promotion</strong>: Cluster Normalization Failed — Primary scale out failed or New standby/replica creation failed</li></ul>(if both fail, they are indicated with a comma)</td></tr></tbody></table>

## Summary of Behavior by Failover Automation Level

<table data-full-width="true"><thead><tr><th>Automation Level</th><th>Automatic Failover</th><th>Automatic Configuration Normalization</th><th>Remarks</th></tr></thead><tbody><tr><td>Level 0 (<strong>Manual</strong>)</td><td>—</td><td>—</td><td>The user <code>역할전환</code> performs promotion and normalization directly using the button</td></tr><tr><td>Level 1 (<strong>Automatic Failover</strong>)</td><td>✓</td><td><ul><li>Recovers only the node count via TAC Scale-Out</li><li>No new Standby/Replica created → DR not recovered (<code>Degraded</code>)</li></ul></td><td>Old Primary/Leader is <code>retired</code> handled</td></tr><tr><td>Level 2 (<strong>Automatic Configuration Recovery</strong>)</td><td>✓</td><td><ul><li>TAC Scale-Out</li><li>New Standby/Replica automatically created → high availability configuration recovered</li></ul></td><td>Old Primary/Leader <code>retired</code> handled</td></tr><tr><td>Level 3 (<strong>Full Automation</strong>)</td><td>✓</td><td><ul><li>Old Primary/Leader reverse synchronization</li><li>New Standby/Replica automatically created</li></ul></td><td>None</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If the existing Primary cluster is TAC, Scale-Out is performed for configuration recovery, and the new Primary cluster is also recovered to a TAC configuration. (Applies to Tibero TAC)
{% endhint %}

{% hint style="warning" %}
**Caution**

- In the case of Levels 1 and 2, if the Primary/Leader Cluster shuts down abnormally, there may be logs that were not transmitted to the Standby/Replica. To ensure data consistency and prevent Split-Brain phenomena, the corresponding instance is handled in the **retired** state.
- When the failover automation level is **Level 1**and there is only one Standby, that Standby is promoted to Primary, so there may be no Standby after failover. In this case, care is needed because the high availability configuration is not maintained. (Applies to Tibero)
{% endhint %}

## Automation Coverage by License Type

In a Cloud environment, the level of automation provided differs depending on the DB engine and license type (LI/BYOL).

| DB Type | LI | BYOL |
| --- | --- | --- |
| Tibero | Levels 0 to 3 (all provided) | Levels 0, 2, 3 (**Level 1 not provided**) |
| OpenSQL | Levels 0, 3 (**Levels 1, 2 not provided**) | Levels 0, 3 (**Levels 1, 2 not provided**) |

## Detailed Scenarios by Automation Level

{% hint style="info" %}
**Note**

**Criteria for Determining Topology Status**

The configuration normalization status after promotion is determined based on whether it matches the same form as the topology method.

- **Tibero TAC Configuration** : Verify that Scale-Out is performed even after Failover to maintain the TAC structure. When there are two or more Primary Nodes: `Running` When there are fewer than two Primary Nodes: `Degraded`
- **DR / HA Configuration (Tibero DR · OpenSQL HA)** : Verify whether a Standby/Replica is retained. When there is one or more Standby/Replica: `Running` (However, if the Standby/Replica is in an abnormal state, it `Degraded`may be) When there are zero Standby/Replica: `Degraded`
{% endhint %}

### Level 0: Manual

**Applies to** : Tibero LI · BYOL, OpenSQL LI · BYOL

No automatic action is performed. When a failure occurs, the user `역할전환` directly promotes the Standby/Replica to Primary/Leader using the button. Configuration normalization after promotion is also performed manually by the user.

### Level 1: Automatic Failover

**Applies to** : Tibero LI (Tibero BYOL · OpenSQL not provided)

The Old Primary is terminated without restarting, and no new Standby is created.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main Operation</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. New Primary Configuration Change</td><td>Perform Scale-Out in the case of a TAC configuration</td><td><code>Updating</code></td><td><ul><li>New Primary DB available → "<strong>Auto Failover complete</strong>"</li><li>On failure, "<strong>Auto Failover failed</strong>"</li></ul></td></tr><tr><td>4. Old Primary Handling</td><td>Marked as Retired status → instance terminated</td><td>-</td><td></td></tr><tr><td>5. Completion</td><td>Configuration normalization complete</td><td><code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure, "<strong>Configuration normalization failed</strong>"</li></ul></td></tr></tbody></table>

### Level 2: Automatic Configuration Recovery

**Applies to** : Tibero LI · BYOL (OpenSQL not provided)

Automatically creates a new Standby to recover the high-availability configuration.

<table data-full-width="true"><thead><tr><th>Step</th><th>Main Action</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure, "<strong>Auto Failover request failed</strong>"</li></ul></td></tr><tr><td>2. Standby Promotion</td><td>Promote the Standby that reflects the most recent log (TSN) to Primary</td><td>-</td><td></td></tr><tr><td>3. new Primary Configuration Change</td><td>For a TAC configuration, perform Scale-Out (including license transfer)</td><td><code>Updating</code></td><td><ul><li>new Primary DB available →</li></ul>"<strong>Auto Failover complete</strong>"<ul><li>On failure,</li></ul>"<strong>Auto Failover failed</strong>"</td></tr><tr><td>4. Old Primary Handling</td><td>Mark as Retired → terminate the instance</td><td>-</td><td></td></tr><tr><td>5. New Standby Creation</td><td>Create with the same specifications in the AZ where the Old Primary was located</td><td>-</td><td></td></tr><tr><td>6. Standby Connection and Synchronization</td><td>Standby connection and synchronization</td><td>-</td><td></td></tr><tr><td>7. Completion</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure,</li></ul>"<strong>Configuration normalization failed</strong>"</td></tr></tbody></table>

### Stage 3: Full Automation

**Applies to** : Tibero LI · BYOL, OpenSQL LI · BYOL

Automatically handles the entire process, from failover to Old Primary/Leader reverse synchronization and new Standby/Replica creation.

<table data-full-width="true"><thead><tr><th>Step</th><th>Key Operation</th><th>Status</th><th>System Notification</th></tr></thead><tbody><tr><td>1. Failure Detection</td><td>Send Auto Failover request</td><td><code>Failover</code></td><td><ul><li><strong>Auto Failover started</strong></li><li>On request failure,</li></ul>"<strong>Auto Failover request failed</strong>"</td></tr><tr><td>2. Old Primary/Leader Restart Attempt</td><td>Restart the instance → attempt DB restart (delete on failure)</td><td>-</td><td></td></tr><tr><td>3. Promotion</td><td>Promote the Standby/Replica that reflects the most recent log to Primary/Leader</td><td>-</td><td></td></tr><tr><td>4. new Primary/Leader Configuration Change</td><td>For a Tibero TAC configuration, perform Scale-Out</td><td><code>Updating</code></td><td><ul><li>new Primary/Leader available →</li></ul>"<strong>Auto Failover complete</strong>"<ul><li>On failure,</li></ul>"<strong>Auto Failover failed</strong>"</td></tr><tr><td>5. Reverse Synchronization Attempt</td><td><ul><li>Restart <strong>success</strong>On success, reconnect the Old Primary/Leader as a Standby/Replica → proceed to step 8</li><li>Restart<strong>failure</strong> On failure, delete the corresponding instance</li></ul></td><td>-</td><td></td></tr><tr><td>6. New Standby/Replica Creation</td><td>Create with the same specifications in the AZ where the Old Primary/Leader was located</td><td>-</td><td></td></tr><tr><td>7. Connection and Synchronization</td><td>Standby/Replica connection and synchronization</td><td>-</td><td></td></tr><tr><td>8. Completion</td><td>Configuration normalization complete</td><td><code>Running</code> / <code>Degraded</code></td><td><ul><li>"<strong>Configuration normalization complete</strong>"</li><li>On failure,</li></ul>"<strong>Configuration normalization failed</strong>"</td></tr></tbody></table>
