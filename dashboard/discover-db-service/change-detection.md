Change detection is a feature that detects when a database configuration has been changed outside of OwlDB and reflects it in OwlDB.

At the top of the dashboard, **Explore** when you click the button, any DBs with detected changes are displayed on the dashboard. **Registration DB**Change detection is provided only for this. Installed DBs are excluded from change detection.

---

## Change Type <a href="#change-types" id="change-types"></a>

Change detection is divided into the following two types.

| Type | Description | Supported Engines |
| --- | --- | --- |
| Status Change | When the DB configuration remains the same but a node's role has changed<br>(e.g., Failover occurrence, Primary ↔ Standby role switch) | Tibero |
| Spec Change | When the DB configuration has been changed outside of OwlDB (e.g., Primary or Standby node Scale In/Out) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- When a status change and a spec change are detected simultaneously, the status change is processed first, and the dashboard displays the corresponding DB only as a status-changed DB.
- Change detection is not supported for topology changes.
- For OpenSQL, node role changes (status changes) are performed by Patroni, and OwlDB automatically reflects the detection results, so no user action screen is provided. Automatically reflected role changes are recorded as Failover in the switchover history, and if a spec change is detected together, the dashboard **Spec Change** activates only the button.
{% endhint %}

---

## Spec-changed DB <a href="#spec-changed-db" id="spec-changed-db"></a>

When Scale In/Out occurs manually outside of OwlDB, after discovery, the dashboard **Spec Change** displays it as a card view with the button activated.

- Scale In instances are not displayed in the card view.
- Scale Out instances are added and displayed at the bottom of the corresponding role list.

### Reflection Method <a href="#undefined" id="undefined"></a>

1. Select the DB with a detected spec change from the dashboard.
2. **Spec Change** Click the button to move to the spec change page.
3. Verify the changed spec information, enter additional information, and reflect it in OwlDB.

---

## Status-changed DB <a href="#status-changed-db" id="status-changed-db"></a>

When a role switch or Failover occurs manually outside of OwlDB, after discovery, the dashboard **Status Change** displays it as a card view with the button activated. Status change actions are provided only for Tibero DB.

The card view displays the existing status stored in OwlDB until the status change is applied, and when you click the status change button, a modal window comparing before and after the change is loaded.

- Left card view: Existing status (status stored in OwlDB)
- Right card view: Current status detected after discovery

{% hint style="info" %}
**Note**

When a status change is detected, only the instance's Health information is retrieved, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### Reflection Method <a href="#undefined-1" id="undefined-1"></a>

It is divided into cases where only a status change is detected and cases where a status change and a spec change are detected simultaneously.

**When only a status change is detected**

1. Select the DB with a detected status change from the dashboard.
2. **Status Change** Click the button.
3. Verify the before and after status in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflect the status change details in OwlDB and close the modal.
- **Cancel** : Close the modal without reflecting the changes.

**When a status change and a spec change are detected simultaneously**

1. Select the DB with a detected status change from the dashboard.
2. **Status Change** Click the button.
3. Verify the before and after status in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflect only the status change in OwlDB and return to the dashboard.
- **Cancel** : Close the modal without reflecting the changes.
- **Go to Spec Change** : After reflecting the status change, move to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

**Apply**If you select this to reflect only the status change, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, the corresponding DB will be displayed again as a spec change target when you run **Explore**again.
{% endhint %}
