Changes the configuration of an operating Cloud DB service, such as instance type, DR configuration, and storage.

Spec changes start from Management > Overview and proceed in five steps: Engine Options → DR Configuration → AZ Configuration → Instance Configuration → Configuration Information Review. In each step, you check the current settings and modify the necessary items, and in the final step, you compare the configuration and estimated cost before and after the change and request to apply it. Depending on the changes, a DB service restart may be required.

The scope of changeable items varies depending on the DB engine and license type (LI/BYOL). With an LI license, most items can be changed, including instance type, DR configuration, availability zone, and storage settings, whereas with a BYOL license, the scope of changes is limited to storage items such as volume size, IOPS, and MBps.

{% hint style="info" %}
**Note**

In the Azure environment, only the BYOL license model is currently supported.
{% endhint %}

1. **Management > Overview**Navigate to
2. **Spec Change**Click.
3. **Engine options**, **DR configuration**, **AZ Configuration**, **Instance Configuration** Navigate to each tab.
4. Set the items to change.
5. **Review Configuration Information** Move to the tab.
6. Review the configuration before and after the change and the estimated cost.
7. **Done**Click it.
8. Review the details in the confirmation modal.
9. **Confirm**Click.

{% hint style="info" %}
**Note**

- The Step 1–4 tabs can be freely navigated in any order.
- **Review Configuration Information** This tab can only be entered when there are no validation errors across all Step 1–4 tabs.
- On the right side of the screen, **Configuration Information** In the floating box, you can review a summary of the content entered in each tab, and error items are displayed in red text.
{% endhint %}

## Changeable Items <a href="#changeable-items" id="changeable-items"></a>

The items that can be changed differ depending on the engine type and license option.

| Item | Tibero LI | Tibero BYOL | OpenSQL LI | OpenSQL BYOL |
| --- | --- | --- | --- | --- |
| Topology | — | — | ✓¹ | — |
| Edition | ✓ | — | ✓ | — |
| Instance Type (Scale Up/Down) | ✓ | — | ✓ | — |
| TAC Node Count (Scale In/Out) | ✓ | — | — | — |
| Replica Scale In/Out | — | — | ✓ | — |
| Enable DR | ✓ | — | ✓² | ✓² |
| Failover Automation Level | ✓ | ✓³ | ✓ | ✓⁴ |
| Volume Size | ✓ | ✓ | ✓ | ✓ |
| Volume IOPS / MBps | ✓ | ✓ | ✓ | ✓ |

- ¹ Can be changed between Single ↔ HA in OpenSQL LI
- ² OpenSQL is determined automatically based on Topology (HA → DR enabled, Single → DR disabled)
- ³ Can be changed when initially configured with DR enabled
- ⁴ Can be changed when initially configured with HA

{% hint style="info" %}
**Note**

- Volume Size can only be changed to a value larger than the current setting.
- When SE (Standard Edition) is selected, the instance type is limited to a maximum of 8vCPU.
- OpenSQL is not supported in AWS environments.
{% endhint %}

## Engine options <a href="#engine-options" id="engine-options"></a>

DB Service Name, DB Engine Type, License Option, and Node Count display the current settings and cannot be changed.

The changeable items are as follows.

- **Edition**: Select between Standard Edition (SE) and Enterprise Edition (EE). SE can use up to 8vCPU, while EE has no vCPU limit. In TAC or HA Topology, EE is applied automatically, and it can only be changed with the LI license.
- **Topology**: Can only be changed between Single ↔ HA in OpenSQL LI.

### TAC Scale In/Out <a href="#tac-scale-in-out" id="tac-scale-in-out"></a>

For the Tibero TAC configuration of the LI license model, you can adjust the node count by directly adding or deleting TAC nodes in the instance Scale In/Out table. The minimum is 2 and the maximum is 4.

{% hint style="info" %}
**Note**

TAC node Scale In/Out is provided only in the Tibero engine. OpenSQL adjusts nodes through Replica Scale In/Out in the DR configuration step.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you delete (Scale In) a TAC instance that is in operation, all data recorded on that instance is deleted.
{% endhint %}

## DR configuration <a href="#dr-configuration" id="dr-configuration"></a>

- **Enable DR**: Select whether to use DR. It can only be changed directly in Tibero LI, and OpenSQL is determined automatically based on Topology (HA → DR enabled, Single → DR disabled). BYOL cannot be changed.
- **Failover Automation Level**: Select the failover automation level when DR is used. It is not displayed when DR is not used.

### Failover Automation Level Options <a href="#failover-automation-level" id="failover-automation-level"></a>

<table><thead><tr><th>Level</th><th>Name</th><th>Description</th></tr></thead><tbody><tr><td>Level 0</td><td>Manual</td><td>When a failure occurs, the user manually promotes the Standby/Replica to Primary/Leader</td></tr><tr><td>Level 1</td><td>Auto Failover</td><td><ul><li>The system switches over automatically</li><li>Recovery and resource optimization are performed manually</li></ul></td></tr><tr><td>Level 2</td><td>Auto Rebuild</td><td><ul><li>After failover, a new Standby/Replica is automatically created to maintain the configuration</li><li>Data recovery is performed manually</li></ul></td></tr><tr><td>Level 3</td><td>Full Automation</td><td>The entire process from failover to recovery and cleanup of unused resources is handled automatically</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Level 3 (Full Automation) prioritizes recovery speed, so some recent data may be lost.
{% endhint %}

{% hint style="info" %}
**Note**

The selectable levels differ depending on the license type.

- **Tibero LI**: All of Levels 0–3 can be selected
- **Tibero BYOL**: Levels 0, 2, and 3 can be selected
- **OpenSQL**: Levels 0 and 3 can be selected
{% endhint %}

### OpenSQL HA Scale In/Out <a href="#opensql-ha-scale-in-out" id="opensql-ha-scale-in-out"></a>

For OpenSQL of the LI license model, you adjust the configuration by adding or deleting Replica nodes in the Replica Scale In/Out table. Only Replica Node #2 and above can be deleted.

{% hint style="info" %}
**Note**

Replica Scale In/Out is provided only in the OpenSQL engine. Tibero adjusts nodes through TAC Scale In/Out in the engine option step.
{% endhint %}

{% hint style="warning" %}
**Caution**

- If you complete the spec change after changing DR to disabled, all data on the existing Standby/Replica instances is deleted.
- If there is an instance in Retired status due to Failover, changing DR to disabled automatically deletes that instance. Since data recovery through that instance will become impossible, proceed only after completing data review and backup.
{% endhint %}

## AZ Configuration <a href="#az-configuration" id="az-configuration"></a>

Check and set the availability zone (AZ) of each instance. Settings can only be configured for newly added instances.

## Instance Configuration <a href="#instance-configuration" id="instance-configuration"></a>

Perform Scale Up/Down by changing the instance type. The BYOL license cannot change the instance type.

The storage-related settings are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Volume Size</td><td>Can only be changed to a value larger than the current setting</td></tr><tr><td>Volume IOPS / MBps</td><td>Can be set within the allowed range depending on the volume type in Azure environments</td></tr><tr><td>Auto Scale</td><td>When enabled, the volume is automatically expanded when Data Volume usage reaches 90%</td></tr><tr><td>Maximum Expansion Limit</td><td><ul><li>Enter the maximum expandable size when Auto Scale is used</li><li>Must enter at least 110% of the current Data Volume Size</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

The volume size cannot be reduced. Set the maximum expansion limit carefully.
{% endhint %}

## Review Configuration Information <a href="#review-configuration" id="review-configuration"></a>

Compare and review the configuration before the change (left) and after the change (right). Changed items are displayed in blue.

The estimated cost is displayed as hourly and monthly costs. The values are calculated based on the Seoul region, and the actual cost may vary depending on the region and actual usage.

After reviewing the details, **Done**clicking it, the processing method differs depending on the change type.

<table><thead><tr><th>Condition</th><th>Action</th></tr></thead><tbody><tr><td>Includes instance Scale Up/Down</td><td><ul><li>A modal is displayed notifying that a restart is required</li><li>Service may be temporarily interrupted during the restart process</li></ul></td></tr><tr><td>Includes only TAC Scale In/Out or storage expansion</td><td>Applied immediately without a restart</td></tr><tr><td>Changing the DR configuration while a Retired instance exists</td><td>A modal is displayed notifying of the Retired instance to be deleted</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you request instance Scale Up/Down and storage expansion together, they are processed in the order of storage expansion → instance Scale Up/Down.
{% endhint %}
