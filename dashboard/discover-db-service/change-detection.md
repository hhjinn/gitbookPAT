Change detection is a feature that detects when a database configuration has been changed outside of OwlDB and reflects it in OwlDB.

When you click the **Discover** button at the top of the dashboard, any DB with detected changes is displayed on the dashboard. The change detection feature is provided only for **registered DBs**. Installed DBs are excluded from change detection.

---

## Change type <a href="#change-types" id="change-types"></a>

Change detection is divided into the following two types.

| Type | Description | Supported engines |
| --- | --- | --- |
| Status Change | When the DB configuration remains the same but the node's role has changed<br>(e.g., Failover occurrence, Primary ↔ Standby role switch) | Tibero |
| Spec Change | When the DB configuration has been changed outside of OwlDB (e.g., Primary or Standby node Scale In/Out) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- When a status change and a spec change are detected simultaneously, the status change is processed first, and the DB is displayed on the dashboard only as a status-changed DB.
- Change detection is not supported for topology changes.
- In OpenSQL, Patroni performs node role changes (status changes), and OwlDB automatically reflects the detection results, so no user action screen is provided. The automatically reflected role change is recorded as Failover in the switchover history, and if a spec change is detected together, only the **Spec Change** button is activated on the dashboard.
{% endhint %}

---

## Spec-changed DB <a href="#spec-changed-db" id="spec-changed-db"></a>

When Scale In/Out occurs manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Spec Change** button activated.

- Scaled-In instances are not displayed in the card view.
- Scaled-Out instances are added and displayed at the bottom of the corresponding role list.

### How to apply <a href="#undefined" id="undefined"></a>

1. Select the DB with a detected spec change on the dashboard.
2. Click the **Spec Change** button to move to the spec change page.
3. Review the changed spec information and enter additional information to reflect it in OwlDB.

---

## Status-changed DB <a href="#status-changed-db" id="status-changed-db"></a>

When a role switch or Failover occurs manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Status Change** button activated. The status change action is provided only for Tibero DB.

The card view displays the existing status stored in OwlDB until the status change is applied, and when you click the status change button, a modal window comparing before and after the change is loaded.

- Left card view: Existing status (status stored in OwlDB)
- Right card view: Current status detected after discovery

{% hint style="info" %}
**Note**

When a status change is detected, only the instance's Health information is queried, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### How to apply <a href="#undefined-1" id="undefined-1"></a>

It is divided into the case where only a status change is detected and the case where a status change and a spec change are detected simultaneously.

**When only a status change is detected**

1. Select the DB with a detected status change on the dashboard.
2. Click the **Status Change** button.
3. Check the status before and after the change in the modal window.
4. Select the desired action from below.

- **Apply**: Reflects the status change details in OwlDB and closes the modal.
- **Cancel**: Closes the modal without reflecting the changes.

**When a status change and a spec change are detected simultaneously**

1. Select the DB with a detected status change on the dashboard.
2. Click the **Status Change** button.
3. Check the status before and after the change in the modal window.
4. Select the desired action from below.

- **Apply**: Reflects only the status change in OwlDB and returns to the dashboard.
- **Cancel**: Closes the modal without reflecting the changes.
- **Go to Spec Change**: After reflecting the status change, moves to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

If you select **Apply** to reflect only the status change, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, the DB is displayed again as a spec change target when you run **Discover** again.
{% endhint %}
