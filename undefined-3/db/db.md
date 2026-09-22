Change detection is a feature that detects when the database configuration has been changed outside of OwlDB and reflects those changes in OwlDB.

At the top of the dashboard, **Discover** when you click the button, any DB with detected changes is displayed on the dashboard. **Registered DB**The change detection feature is provided only for. Installed DBs are excluded from change detection.

---

## Change Types

Change detection is divided into the following two types.

| Type | Description | Supported Engines |
| --- | --- | --- |
| Status Change | When the DB configuration remains the same but the node's role has changed<br>(e.g., Failover occurred, Primary ↔ Standby role switch) | Tibero |
| Spec Change | When the DB configuration has been changed outside of OwlDB (e.g., Primary or Standby node Scale In/Out) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- When a status change and a spec change are detected simultaneously, the status change is processed first, and the DB is displayed on the dashboard only as a status-changed DB.
- Change detection is not supported for topology changes.
- For OpenSQL, node role changes (status changes) are performed by Patroni, and OwlDB automatically reflects the detection results, so no user action screen is provided. Automatically reflected role changes are recorded as Failover in the switchover history, and if a spec change is detected together, the dashboard activates only the **Spec Change** button.
{% endhint %}

---

## Spec-Changed DB

When Scale In/Out has occurred manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Spec Change** button activated.

- Scale In instances are not displayed in the card view.
- Scale Out instances are added and displayed at the bottom of the corresponding role list.

### How to Reflect

1. Select the DB with a detected spec change on the dashboard.
2. **Spec Change** Click the button to go to the spec change page.
3. Verify the changed spec information and enter additional information to reflect it in OwlDB.

---

## Status-Changed DB

When a role switch or Failover has occurred manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Status Change** button activated. The status change action is provided only for Tibero DBs.

The card view continues to display the existing status stored in OwlDB until the status change is applied, and clicking the status change button loads a modal window comparing the before and after states.

- Left card view: Existing status (status stored in OwlDB)
- Right card view: Current status detected after discovery

{% hint style="info" %}
**Note**

When a status change is detected, only the instance's Health information is queried, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### How to Reflect

This is divided into cases where only a status change is detected and cases where a status change and a spec change are detected simultaneously.

**When only a status change is detected**

1. Select the DB with a detected status change on the dashboard.
2. **Status Change** Click the button.
3. Verify the before and after states in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects the status change details in OwlDB and closes the modal.
- **Cancel** : Closes the modal without reflecting the changes.



**When a status change and a spec change are detected simultaneously**

1. Select the DB with a detected status change on the dashboard.
2. **Status Change** Click the button.
3. Verify the before and after states in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects only the status change in OwlDB and returns to the dashboard.
- **Cancel** : Closes the modal without reflecting the changes.
- **Go to Spec Change** : After reflecting the status change, moves to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

**Apply**If you select to reflect only the status change, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, the DB will be displayed again as a spec change target when you run **Discover**again.
{% endhint %}
