## Discovery Process

On the OwlDB screen **Discover** When you click the button, OwlDB Agent automatically collects the configuration information of the customer infrastructure where it is installed, allowing you to check the current status on the dashboard.

1. **OwlDB console screen** > **Dashboard**Navigate to.
2. Click the Discover button.
3. Discovery results are displayed in a total of four categories. For details on each category, refer to the **Discovery Results** section below.
4. Review the results and proceed with the appropriate follow-up actions for each category.

{% hint style="info" %}
**Note**

The database discovery feature is only available with the Root account.
{% endhint %}

---

## Discovery Results

{% hint style="info" %}
**Note**

Categories with no discovery results are not displayed as cards.
{% endhint %}

### **Available Installation Hosts**

You can view the list of hosts on which databases can be installed. Each engine, such as Tibero and OpenSQL, is displayed as a separate card, and hosts of engines not found during the discovery process are not displayed as cards. The Available Installation Hosts category is view-only and does not require any separate follow-up actions.

| Item | Description |
| --- | --- |
| Hostname | Available installation host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registerable Databases**

Databases already in operation in the customer environment are automatically discovered and provided as a list. You can select the desired database from the list and click the **Register** button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database Name | Discovered cluster name |
| Engine | Database engine type |
| Configuration Information | Basic configuration information such as number of nodes and roles |
| Action | Register button |

---

### Change Detection

If a database registered in OwlDB is directly changed outside of OwlDB, the change history is detected and notified.

Change Detection detects the following two types of changes.

| Type | Description | Example | Follow-up Action |
| --- | --- | --- | --- |
| **Spec Change** | When the configuration information of a database is changed | Increase/decrease in number of nodes | **Spec Change** Reflect change history after clicking the button |
| **Status Change** | When the role of a node is changed in a DR configuration | Primary ↔ Standby role switch | **Status Change** Reflect change history after clicking the button |

The card view display rules differ depending on the type of change.

- **Status Change**: Until the changes are applied, the card displays the existing status stored in OwlDB as is.
- **Spec Change**: Scale-in instances are not displayed on the card, and Scale-out instances are additionally displayed at the bottom of the list.

**Status Change** When you click the button, a modal comparing the state before and after the change appears.

- **Current (left)**: Existing status stored in OwlDB
- **Updated (right)**: Current status detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal **Apply**When you click, the status change history is reflected in OwlDB, and **Go to Spec Change**When you click, the status change is reflected and then you are navigated to the Spec Change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- If a spec change and a status change are detected at the same time, the status change is processed first.
- When only a status change is applied in the modal, the spec change follow-up task disappears from the screen, and if unreflected spec changes remain, they are marked again as spec change targets upon re-scanning.
{% endhint %}

{% hint style="warning" %}
**Caution**

When a topology change is detected, it is handled as an unsupported target for change detection, and the reflection function is not provided.
{% endhint %}

---

### Abnormal Node

Nodes whose configuration information OwlDB could not properly identify during the scan process are marked as abnormal nodes.

The abnormal node card displays the following information.

| Item | Description |
| --- | --- |
| Node Identifier | Identifier value of the node or node group classified as abnormal |
| Abnormal Type | Configuration Unidentifiable / Partially Identified |
| Description | Reason for being classified as abnormal |
| Guide | Handling guide link or button |

Abnormal nodes occur in the following two cases.

| Case | Description |
| --- | --- |
| When the DB configuration cannot be identified at all | Marks that single node as an abnormal node |
| When the DB configuration is only partially identified | Groups the identified nodes into a single abnormal node for display |

When an abnormal node occurs, take action by referring to the separate handling guide.
