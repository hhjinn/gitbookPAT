## Discovery Process

On the OwlDB screen **Discover** When you click the button, OwlDB Agent automatically collects the configuration information of the customer infrastructure where it is installed, allowing you to view the status on the dashboard.

1. **OwlDB console screen** > **Dashboard**Navigate to.
2. Click the Discover button.
3. Discovery results are displayed across a total of four items. For details on each item, refer to the **Discovery Results** section below.
4. Review the results and proceed with the follow-up actions appropriate for each item.

{% hint style="info" %}
**Note**

The database discovery feature is only available with the Root account.
{% endhint %}

---

## Discovery Results

{% hint style="info" %}
**Note**

Items with no discovery results are not displayed as cards.
{% endhint %}

### **Installable Host Lookup**

You can view the list of hosts on which databases can be installed. They are displayed as separate cards by engine, such as Tibero and OpenSQL, and hosts of engines that were not found during the discovery process are not displayed as cards. The installable host item is read-only and does not require any follow-up action.

| Item | Description |
| --- | --- |
| Hostname | Installable host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registrable Databases**

Databases already in operation in the customer environment are automatically discovered and provided as a list. You can select the desired database from the list and **Register** click the button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database name | Discovered cluster name |
| Engine | Database engine type |
| Configuration information | Basic configuration information such as node count and role |
| Action | Register button |

---

### Change Detection

When a database registered in OwlDB is modified directly outside of OwlDB, the change is detected and reported.

Change detection detects the following two types of changes.

| Type | Description | Example | Follow-up action |
| --- | --- | --- | --- |
| **Spec Change** | When the configuration information of a database has changed | Node count increase/decrease | **Spec Change** Apply the changes after clicking the button |
| **Status Change** | When the role of a node has changed in a DR configuration | Primary ↔ Standby role switch | **Status Change** Apply the changes after clicking the button |

The card view display rules differ depending on the type of change.

- **Status Change**: Until the changes are applied, the card displays the existing status stored in OwlDB as is.
- **Spec Change**: Scale-in instances are not displayed on the card, and Scale-out instances are additionally displayed at the bottom of the list.

**Status Change** When you click the button, a modal comparing before and after the change appears.

- **Current (left)**: Existing status stored in OwlDB
- **Updated (right)**: Current status detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal, **Apply**When you click, the status change is reflected in OwlDB, and **Go to Spec Change**When you click, the status change is applied and then you are moved to the spec change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- When a spec change and a status change are detected at the same time, the status change is processed first.
- If only the status change is applied in the modal, the spec change follow-up task disappears from the screen, and if any unreflected spec changes remain, they will be marked as spec change targets again upon re-scanning.
{% endhint %}

{% hint style="warning" %}
**Caution**

If a topology change is detected, it is treated as an unsupported target in change detection, and the reflection feature is not provided.
{% endhint %}

---

### Abnormal Node

Nodes whose configuration information was not properly identified by OwlDB during the scanning process are marked as abnormal nodes.

The following information is displayed on the abnormal node card.

| Item | Description |
| --- | --- |
| Node Identifier | Identifier value of the node or node group classified as abnormal |
| Abnormal Type | Configuration Not Identifiable / Partially Identified |
| Description | Reason for being classified as abnormal |
| Guide | Handling guide link or button |

Abnormal nodes occur in the following two cases.

| Case | Description |
| --- | --- |
| When the DB configuration cannot be identified at all | The single node is marked as an abnormal node |
| When the DB configuration is only partially identified | The identified nodes are grouped and displayed as a single abnormal node |

If an abnormal node occurs, take action by referring to the separate handling guide.
