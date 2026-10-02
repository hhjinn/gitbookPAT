Change detection is a feature that detects when the database configuration has been changed outside of OwlDB and reflects it in OwlDB.

At the top of the dashboard, **Explore** When you click the button, if there is a DB with detected changes, it is displayed on the dashboard. **Registered DB**Change detection is provided only for these. The installation DB is excluded from change detection.

---

## Change Types

Change detection is categorized into the following two types.

| Type | Description | Supported Engines |
| --- | --- | --- |
| Status Change | When the DB configuration remains the same but a node's role has changed<br>(e.g., Failover occurrence, Primary ↔ Standby role switch) | Tibero |
| Spec Change | When the DB configuration has been changed outside of OwlDB (e.g., Scale In/Out of a Primary or Standby node) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- When a state change and a spec change are detected simultaneously, the state change is processed first, and the DB is displayed on the dashboard only as a state-changed DB.
- Change detection is not supported for topology changes.
- For OpenSQL, node role changes (state changes) are performed by Patroni, and OwlDB automatically reflects the detection results, so no user action screen is provided. An automatically reflected role change is recorded as Failover in the switch history, and if a spec change is detected along with it, on the dashboard only the **Spec Change** button is activated.
{% endhint %}

---

## Spec-Changed DB

When Scale In/Out occurs manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Spec Change** button activated.

- A Scale In instance is not displayed in the card view.
- A Scale Out instance is added and displayed at the bottom of the corresponding role list.

### Reflection Method

1. Select the DB in which a spec change was detected on the dashboard.
2. **Spec Change** Click the button to move to the spec change page.
3. Check the changed spec information and enter additional information to reflect it in OwlDB.

---

## State-Changed DB

When a role switch or Failover occurs manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Status Change** button activated. State change action is provided only for Tibero DB.

The card view displays the existing state stored in OwlDB as is until the state change is applied, and clicking the state change button loads a modal window comparing the state before and after the change.

- Left card view: Existing state (state stored in OwlDB)
- Right card view: Current state detected after discovery

{% hint style="info" %}
**Note**

When a state change is detected, only the instance's Health information is retrieved, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### Reflection Method

It is divided into cases where only a state change is detected and cases where a state change + spec change are detected simultaneously.

**When Only a State Change Is Detected**

1. Select the DB in which a state change was detected on the dashboard.
2. **Status Change** Click the button.
3. Check the state before and after the change in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects the state change details in OwlDB and closes the modal.
- **Cancel** : Closes the modal without reflecting the changes.



**When a State Change + Spec Change Are Detected Simultaneously**

1. Select the DB in which a state change was detected on the dashboard.
2. **Status Change** Click the button.
3. Check the state before and after the change in the modal window.
4. Select the desired action from the following.

- **Apply** : Reflects only the state change in OwlDB and returns to the dashboard.
- **Cancel** : Closes the modal without reflecting the changes.
- **Go to Spec Change** : After reflecting the state change, moves to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

**Apply**When only the state change is reflected by selecting this, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, the DB is displayed again as a spec change target when you run **Explore**again.
{% endhint %}
