## Discovery Process

On the OwlDB screen **Explore** clicking the button automatically collects the configuration information of the customer infrastructure where the OwlDB Agent is installed, allowing you to check the status on the dashboard.

1. **OwlDB console screen** > **Dashboard**to navigate to.
2. Click the Discovery button.
3. Discovery results are retrieved as a total of 4 items, and for details on each item, refer to the **Discovery Results** section below.
4. Check the results and proceed with the follow-up tasks appropriate for each item.

{% hint style="info" %}
**Note**

The database discovery feature is available only from the Root account.
{% endhint %}

---

## Discovery Results

{% hint style="info" %}
**Note**

Items with no discovery results are not displayed as cards.
{% endhint %}

### **Installable Host Lookup**

You can check the list of hosts where a database can be installed. They are displayed as separate cards by engine, such as Tibero and OpenSQL, and hosts of engines not found during the discovery process are not displayed as cards. The installable host item is view-only and does not require any separate follow-up tasks.

| Item | Description |
| --- | --- |
| Hostname | Installable host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registrable Databases**

It automatically discovers databases already running in the customer environment and provides them as a list. Select the desired database from the list and **Registered** click the button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database name | Discovered cluster name |
| Engine | Database engine type |
| Configuration Information | Basic configuration information such as number of nodes and roles |
| Action | Register button |

---

### Change Detection

When a database registered in OwlDB is changed directly outside of OwlDB, it detects the changes and notifies you.

Change detection detects the following two types of changes.

| Type | Description | Example | Follow-up Tasks |
| --- | --- | --- | --- |
| **Spec Change** | When the configuration information of a database is changed | Increase/decrease in number of nodes | **Spec Change** Changes are applied after clicking the button |
| **Status Change** | When the role of a node is changed in a DR configuration | Primary ↔ Standby role switch | **Status Change** Changes are applied after clicking the button |

The card view display rules differ depending on the type of change.

- **Status Change**: Until the changes are applied, the card displays the existing state stored in OwlDB as is.
- **Spec Change**: Scale-in instances are not displayed on the card, and Scale-out instances are additionally displayed at the bottom of the list.

**Status Change** Clicking the button brings up a modal comparing before and after the change.

- **Current (left)**: Existing state stored in OwlDB
- **Updated (right)**: Current state detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal **Apply**clicking it reflects the status change details in OwlDB, and **Go to Spec Change**clicking it reflects the status change and then navigates to the spec change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- When a spec change and a status change are detected simultaneously, the status change is processed first.
- If only the status change is applied in the modal, the spec change follow-up task disappears from the screen, and if there are unreflected spec changes remaining, they are displayed again as spec change targets upon re-discovery.
{% endhint %}

{% hint style="warning" %}
**Caution**

When a topology change is detected, it is treated as a target not supported by change detection, and the reflection feature is not provided.
{% endhint %}

---

### Abnormal Node

Nodes for which OwlDB failed to properly identify the configuration information during the discovery process are displayed as abnormal nodes.

The abnormal node card displays the following information.

| Item | Description |
| --- | --- |
| Node identifier | Identifier value of the node or node group classified as abnormal |
| Abnormal type | Configuration unidentifiable / Partially identified |
| Description | Reason classified as abnormal |
| Guide | Handling guide link or button |

Abnormal nodes occur in the following two cases.

| Case | Description |
| --- | --- |
| When the DB configuration cannot be identified at all | Mark that single node as an abnormal node |
| When the DB configuration is only partially identified | Group the identified nodes together and display them as a single abnormal node |

If an abnormal node occurs, take action by referring to the separate handling guide.
