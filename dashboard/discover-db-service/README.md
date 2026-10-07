## Discovery process <a href="#discovery-process" id="discovery-process"></a>

On the OwlDB screen **Explore** Clicking the button automatically collects the configuration information of the customer infrastructure where the OwlDB Agent is installed, allowing you to monitor the status on the dashboard.

1. **OwlDB console screen** > **Dashboard**Navigate to it.
2. Click the discovery button.
3. Discovery results are retrieved in a total of 4 items, and detailed information about each item is provided in the **Discovery results** Refer to the section.
4. Review the results and proceed with the appropriate follow-up actions for each item.

{% hint style="info" %}
**Note**

The database discovery feature is available only on the Root account.
{% endhint %}

---

## Discovery results <a href="#discovery-results" id="discovery-results"></a>

{% hint style="info" %}
**Note**

Items with no discovery results do not display a card.
{% endhint %}

### **View installable hosts** <a href="#undefined" id="undefined"></a>

You can view the list of hosts on which a database can be installed. They are displayed as separate cards by engine, such as Tibero and OpenSQL, and hosts of engines not found during discovery are not displayed as cards. The installable hosts item is view-only and does not require any separate follow-up action.

| Item | Description |
| --- | --- |
| Hostname | Installable host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registrable databases** <a href="#undefined-1" id="undefined-1"></a>

Automatically discovers databases already in operation in the customer environment and provides them as a list. Select the desired database from the list and **registered** Click the button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database name | Discovered cluster name |
| Engine | Database engine type |
| Configuration information | Basic configuration information such as node count and role |
| Action | Register button |

---

### Change detection <a href="#undefined-2" id="undefined-2"></a>

When a database registered in OwlDB is changed directly outside of OwlDB, the change is detected and a notification is issued.

Change detection detects the following two types of changes.

| Type | Description | Example | Follow-up action |
| --- | --- | --- | --- |
| **Spec change** | When the configuration information of a database is changed | Increase/decrease in node count | **Spec change** Apply the changes after clicking the button |
| **Status change** | When the role of a node is changed in the DR configuration | Primary ↔ Standby role switch | **Status change** Apply the changes after clicking the button |

The card view display rules differ depending on the type of change.

- **Status change**: Until the changes are applied, the card displays the existing status stored in OwlDB as is.
- **Spec change**: Scale-in instances are not displayed on the card, and Scale-out instances are additionally displayed at the bottom of the list.

**Status change** Clicking the button brings up a modal that compares before and after the change.

- **Current (left)**: The existing status stored in OwlDB
- **Updated (right)**: The current status detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal **Apply**Clicking it applies the status change to OwlDB, and **Go to spec change**Clicking it applies the status change and then navigates to the spec change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- When a spec change and a status change are detected simultaneously, the status change is processed first.
- If only the status change is applied in the modal, the spec change follow-up action disappears from the screen, and if any unapplied spec changes remain, they are displayed again as spec change targets upon re-discovery.
{% endhint %}

{% hint style="warning" %}
**Caution**

When a topology change is detected, it is treated as a target not supported by change detection, and the apply function is not provided.
{% endhint %}

---

### Abnormal node <a href="#undefined-3" id="undefined-3"></a>

Nodes whose configuration information OwlDB failed to properly identify during discovery are marked as abnormal nodes.

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
| When the DB configuration cannot be identified at all | The relevant single node is marked as an abnormal node |
| When the DB configuration is only partially identified | The identified nodes are grouped and displayed as a single abnormal node |

When an abnormal node occurs, take action by referring to the separate handling guide.
