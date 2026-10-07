Change detection is a feature that detects when a database configuration has been changed outside of OwlDB and reflects it in OwlDB.

At the top of the dashboard, the **Explore** When you click the button, any DB with detected changes will be displayed on the dashboard. **Registered DB**The change detection feature is provided only for this. Installed DBs are excluded from change detection.

---

## Change type <a href="#change-types" id="change-types"></a>

Change detection is divided into the following two types.

| Type | Description | Supported engines |
| --- | --- | --- |
| Status change | When the DB configuration remains the same but the node's role has changed<br>(e.g., Failover occurred, Primary ↔ Standby role switch) | Tibero |
| Spec change | When the DB configuration has been changed outside of OwlDB (e.g., Primary or Standby node Scale In/Out) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- If a status change and a spec change are detected simultaneously, the status change is processed first, and the dashboard displays that DB only as a status-changed DB.
- Change detection is not supported for topology changes.
- In OpenSQL, Patroni performs node role changes (status changes), and OwlDB automatically reflects the detection results, so no user action screen is provided. Automatically reflected role changes are recorded as Failover in the switch history, and if a spec change is detected together, the dashboard activates only the **Spec change** button.
{% endhint %}

---

## Spec-changed DB <a href="#spec-changed-db" id="spec-changed-db"></a>

When Scale In/Out occurs manually outside of OwlDB, after detection it is displayed on the dashboard as a card view with the **Spec change** button activated.

- Scale-In instances are not displayed in the card view.
- Scale-Out instances are added and displayed at the bottom of the corresponding role list.

### Reflection method <a href="#undefined" id="undefined"></a>

1. On the dashboard, select the DB for which a spec change was detected.
2. **Spec change** Click the button to go to the spec change page.
3. Verify the changed spec information and enter additional information to reflect it in OwlDB.

---

## Status-changed DB <a href="#status-changed-db" id="status-changed-db"></a>

When a role switch or Failover occurs manually outside of OwlDB, after detection it is displayed on the dashboard as a card view with the **Status change** button activated. Status change actions are provided only for Tibero DB.

The card view displays the existing status stored in OwlDB until the status change is applied, and when you click the status change button, a modal window comparing before and after the change is loaded.

- Left card view: Existing status (status stored in OwlDB)
- Right card view: Current status detected after exploration

{% hint style="info" %}
**Note**

When a status change is detected, only the instance's Health information is queried, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### Reflection method <a href="#undefined-1" id="undefined-1"></a>

It is divided into cases where only a status change is detected and cases where a status change and a spec change are detected simultaneously.

**When only a status change is detected**

1. On the dashboard, select the DB for which a status change was detected.
2. **Status change** Click the button.
3. Check the before and after status in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects the status change details in OwlDB and closes the modal.
- **Cancel** : Closes the modal without reflecting the changes.

**When a status change and a spec change are detected simultaneously**

1. On the dashboard, select the DB for which a status change was detected.
2. **Status change** Click the button.
3. Check the before and after status in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects only the status change in OwlDB and returns to the dashboard.
- **Cancel** : Closes the modal without reflecting the changes.
- **Go to spec change** : After reflecting the status change, goes to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

**Apply**If you select this to reflect only the status change, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, when you run **Explore**again, that DB is displayed again as a spec change target.
{% endhint %}
