## Discovery process <a href="#discovery-process" id="discovery-process"></a>

When you click the **Discover** button on the OwlDB screen, the OwlDB Agent automatically collects the configuration information of the customer infrastructure where it is installed, allowing you to grasp the status on the dashboard.

1. Go to **OwlDB Console screen** > **Dashboard**.
2. Click the Discover button.
3. The discovery results are retrieved as a total of 4 items, and for details on each item, refer to the **Discovery Results** section below.
4. Check the results and proceed with the follow-up tasks appropriate for each item.

{% hint style="info" %}
**Note**

The database discovery feature is available only from the Root account.
{% endhint %}

---

## Discovery Results <a href="#discovery-results" id="discovery-results"></a>

{% hint style="info" %}
**Note**

For items with no discovery results, no card is displayed.
{% endhint %}

### **Lookup of installable hosts** <a href="#undefined" id="undefined"></a>

You can check the list of hosts where a database can be installed. They are displayed as separate cards by engine, such as Tibero and OpenSQL, and hosts of engines not found during the discovery process are not displayed as cards. The installable hosts item is view-only and requires no separate follow-up tasks.

| Item | Description |
| --- | --- |
| Hostname | Installable host name |
| CPU | Host CPU information |
| Memory | Host Memory information |
| OS | Host OS information |

---

### **Registrable Databases** <a href="#undefined-1" id="undefined-1"></a>

Databases already in operation in the customer environment are automatically discovered and provided as a list. You can select the desired database from the list and click the **Register** button to proceed with the registration process.

| Column | Description |
| --- | --- |
| Database Name | Discovered cluster name |
| Engine | Database engine type |
| Configuration Information | Basic configuration information such as number of nodes and roles |
| Action | Register button |

---

### Change Detection <a href="#undefined-2" id="undefined-2"></a>

When a database registered in OwlDB is modified directly outside of OwlDB, the changes are detected and notified.

Change detection detects the following two types of changes.

| Type | Description | Example | Follow-up Tasks |
| --- | --- | --- | --- |
| **Spec Change** | When the database configuration information has changed | Increase/decrease in the number of nodes | Changes are applied after clicking the **Spec Change** button |
| **Status Change** | When a node's role has changed in a DR configuration | Primary ↔ Standby role switchover | Changes are applied after clicking the **Status Change** button |

Card view display rules differ depending on the type of change.

- **Status Change**: Until the changes are applied, the card continues to display the existing status stored in OwlDB.
- **Spec Change**: Scaled-in instances are not displayed on the card, while scaled-out instances are additionally displayed at the bottom of the list.

When you click the **Status Change** button, a modal appears comparing the before and after states.

- **Current (left)**: The existing status stored in OwlDB
- **Updated (right)**: The current status detected through discovery (however, detailed information such as Eventlog, Status, Health, CPU, Memory, and Active Session is not displayed.)

In the modal, clicking **Apply** reflects the status change in OwlDB, and clicking **Go to Spec Change** applies the status change and then moves to the Spec Change page.

{% hint style="info" %}
**Note**

- **Databases installed through OwlDB are excluded from change detection.**
- When a spec change and a status change are detected simultaneously, the status change is processed first.
- If only the status change is applied in the modal, the spec change follow-up task disappears from the screen, and if any unapplied spec changes remain, they will again be marked as spec change targets upon re-discovery.
{% endhint %}

{% hint style="warning" %}
**Caution**

If a topology change is detected, it is treated as a target not supported by change detection, and the apply function is not provided.
{% endhint %}

---

### Abnormal Nodes <a href="#undefined-3" id="undefined-3"></a>

Nodes whose configuration information OwlDB could not properly identify during the discovery process are marked as abnormal nodes.

The abnormal node card displays the following information.

| Item | Description |
| --- | --- |
| Node Identifier | Identifier value of the node or node group classified as abnormal |
| Abnormality Type | Configuration unidentifiable / partially identified |
| Description | Reason classified as abnormal |
| Guide | Handling guide link or button |

Abnormal nodes occur in the following two cases.

| Case | Description |
| --- | --- |
| When the DB configuration cannot be identified at all | That single node is marked as an abnormal node |
| When the DB configuration is only partially identified | The identified nodes are grouped and displayed as a single abnormal node |

When an abnormal node occurs, take action by referring to the separate handling guide.
