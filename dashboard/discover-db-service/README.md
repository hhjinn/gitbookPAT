## Discovery Process <a href="#discovery-process" id="discovery-process"></a>

On the OwlDB screen **Explore** When you click the button, the OwlDB Agent automatically collects configuration information from the customer infrastructure where it is installed, allowing you to understand the current status on the dashboard.

1. **OwlDB Console Screen** > **Dashboard**to navigate.
2. Click the Discovery button.
3. The discovery results are retrieved in four items, and for details on each item, refer to the **Discovery Results** section below.
4. Review the results and proceed with the follow-up tasks appropriate for each item.

{% hint style="info" %}
**Note**

The database discovery feature is available only for the Root account.
{% endhint %}

---

## Discovery Results <a href="#discovery-results" id="discovery-results"></a>

{% hint style="info" %}
**Note**

Items with no discovery results are not displayed as cards.
{% endhint %}

### **View Installable Hosts** <a href="#undefined" id="undefined"></a>

You can view the list of hosts where a database can be installed. They are displayed in separate cards by engine, such as Tibero and OpenSQL, and hosts of engines not found during the discovery process are not displayed as cards. The installable hosts item is view-only and does not require any follow-up tasks.

| Item | Description |
| --- | --- |
| Hostname | Installable host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registrable Databases** <a href="#undefined-1" id="undefined-1"></a>

Databases already in operation in the customer environment are automatically discovered and provided as a list. Select the desired database from the list and **Registered** Click the button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database name | Discovered cluster name |
| Engine | Database engine type |
| Configuration Information | Basic configuration information such as the number of nodes and roles |
| Action | Register button |

---

### Change Detection <a href="#undefined-2" id="undefined-2"></a>

If a database registered in OwlDB is modified directly outside of OwlDB, the changes are detected and notified.

Change detection detects the following two types of changes.

| Type | Description | Example | Follow-up Tasks |
| --- | --- | --- | --- |
| **Spec Change** | When the configuration information of the database has changed | Increase/decrease in the number of nodes | **Spec Change** Reflecting the changes after clicking the button |
| **Status Change** | When the role of a node has changed in the DR configuration | Primary ↔ Standby role switch | **Status Change** Reflecting the changes after clicking the button |

Card view display rules differ depending on the type of change.

- **Status Change**: Until the changes are applied, the card displays the existing state stored in OwlDB as is.
- **Spec Change**: Scale-in instances are not displayed in the card, and Scale-out instances are additionally displayed at the bottom of the list.

**Status Change** Clicking the button displays a modal that compares the state before and after the change.

- **Current (left)**: The existing state stored in OwlDB
- **Updated (right)**: The current state detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal **Apply**When you click it, the status change details are reflected in OwlDB, and **Go to Spec Change**When you click it, the status change is reflected and then you are navigated to the spec change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- When a spec change and a status change are detected simultaneously, the status change is processed first.
- If only the status change is applied in the modal, the spec change follow-up task disappears from the screen, and if there are unreflected spec changes remaining, they are displayed again as spec change targets upon re-discovery.
{% endhint %}

{% hint style="warning" %}
**Caution**

When a topology change is detected, it is treated as an unsupported target for change detection, and the reflection feature is not provided.
{% endhint %}

---

### Abnormal Node <a href="#undefined-3" id="undefined-3"></a>

Nodes whose configuration information OwlDB failed to properly identify during the discovery process are marked as abnormal nodes.

The abnormal node card displays the following information.

| Item | Description |
| --- | --- |
| Node identifier | Identifier value of the node or node group classified as abnormal |
| Abnormal type | Configuration unidentifiable / Partially identified |
| Description | Reason for classification as abnormal |
| Guide | Handling guide link or button |

Abnormal nodes occur in the following two cases.

| Case | Description |
| --- | --- |
| When the DB configuration cannot be identified at all | The single node is marked as an abnormal node |
| When the DB configuration is only partially identified | The identified nodes are grouped and displayed as a single abnormal node |

If an abnormal node occurs, refer to the separate handling guide to take action.
