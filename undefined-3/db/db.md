Change detection is a feature that detects when the database configuration has been changed outside of OwlDB and reflects those changes in OwlDB.

At the top of the dashboard, **Discover** When you click the button, any DBs with detected changes are displayed on the dashboard. **Registered DBs**The change detection feature is provided only for these. Installed DBs are excluded from change detection.

---

## Change Types

Change detection is classified into the following two types.

| Type | Description | Supported Engine |
| --- | --- | --- |
| State Change | When the DB configuration remains the same but the role of a node has changed<br>(e.g., Failover occurrence, Primary ↔ Standby role switch) | Tibero |
| Spec Change | When the DB configuration has been changed outside of OwlDB (e.g., Primary or Standby node Scale In/Out) | Tibero, OpenSQL |

{% hint style="info" %}
**Note**

- When a state change and a spec change are detected simultaneously, the state change is processed first, and the DB is displayed on the dashboard only as a state-changed DB.
- Change detection is not supported for topology changes.
- For OpenSQL, node role changes (state changes) are performed by Patroni, and OwlDB automatically reflects the detection results, so no user action screen is provided. Automatically reflected role changes are recorded as Failover in the switchover history, and if a spec change is detected together, only the **Spec Change** button is enabled on the dashboard.
{% endhint %}

---

## Spec-Changed DBs

When Scale In/Out has occurred manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **Spec Change** button enabled.

- Scale In instances are not displayed in the card view.
- Scale Out instances are added and displayed at the bottom of the corresponding role list.

### Reflection Method

1. On the dashboard, select the DB for which a spec change was detected.
2. **Spec Change** Click the button to navigate to the spec change page.
3. Review the changed spec information and enter additional information to reflect it in OwlDB.

---

## State-Changed DBs

When a role switch or Failover has occurred manually outside of OwlDB, after discovery it is displayed on the dashboard as a card view with the **State Change** button enabled. State change actions are provided only for Tibero DBs.

In the card view, the existing state stored in OwlDB is displayed as-is until the state change is applied, and when you click the state change button, a modal window comparing the before and after states is loaded.

- Left card view: Existing state (state stored in OwlDB)
- Right card view: Current state detected after discovery

{% hint style="info" %}
**Note**

When a state change is detected, only the Health information of the instance is retrieved, and detailed information such as CPU, Memory, and active sessions is not displayed.
{% endhint %}

### Reflection Method

This is divided into cases where only a state change is detected and cases where a state change and a spec change are detected simultaneously.

**When only a state change is detected**

1. On the dashboard, select the DB for which a state change was detected.
2. **State Change** Click the button.
3. In the modal window, review the before and after states.
4. Select the desired action from the following.

- **Apply** : Reflects the state change details in OwlDB and closes the modal.
- **Cancel** : Closes the modal without reflecting the changes.



**When a state change and a spec change are detected simultaneously**

1. On the dashboard, select the DB for which a state change was detected.
2. **State Change** Click the button.
3. In the modal window, review the before and after states.
4. Select the desired action from the following.

- **Apply** : Reflects only the state change in OwlDB and returns to the dashboard.
- **Cancel** : Closes the modal without reflecting the changes.
- **Go to Spec Change** : After reflecting the state change, navigates to the spec change page to reflect the spec change as well.

{% hint style="info" %}
**Note**

**Apply**When you select this to reflect only the state change, the spec change button is not displayed on the dashboard. If there are unreflected spec changes remaining, the DB will be displayed again as a spec change target when you run **Discover**again.
{% endhint %}
