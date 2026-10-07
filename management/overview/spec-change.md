Changes the configuration of a running Cloud DB service, such as instance type, DR configuration, and storage.

Spec changes start from Management > Overview and proceed through five steps: Engine Options → DR Configuration → AZ Configuration → Instance Configuration → Configuration Information Review. In each step, you review the current settings and modify the necessary items, and in the final step, you compare the configuration before and after the change along with the estimated cost and request to apply it. Depending on the changes, a DB service restart may be required.

The range of changeable items differs depending on the DB engine and license type (LI/BYOL). The LI license allows changing most items including instance type, DR configuration, availability zone, and storage settings, while the BYOL license is limited in change scope to storage items such as volume size, IOPS, and MBps.

{% hint style="info" %}
**Note**

In the Azure environment, only the BYOL license model is currently supported.
{% endhint %}

1. Go to **Management > Overview**.
2. Click **Spec Change**.
3. Go to each tab: **Engine Options**, **DR Configuration**, **AZ Configuration**, and **Instance Configuration**.
4. Set the items to change.
5. Go to the **Configuration Information Review** tab.
6. Review the configuration before and after the change and the estimated cost.
7. Click **Complete**.
8. Review the contents in the confirmation modal.
9. Click **Confirm**.

{% hint style="info" %}
**Note**

- The tabs for steps 1–4 can be navigated freely regardless of order.
- The **Configuration Information Review** tab can only be entered when there are no validation errors across all tabs for steps 1–4.
- In the **Configuration Information** floating box on the right side of the screen, you can review a summary of the content entered in each tab, and error items are displayed in red text.
{% endhint %}

## Changeable items <a href="#changeable-items" id="changeable-items"></a>

The items that can be changed differ depending on the engine type and license option.

| Item | Tibero LI | Tibero BYOL | OpenSQL LI | OpenSQL BYOL |
| --- | --- | --- | --- | --- |
| Topology | — | — | ✓¹ | — |
| Edition | ✓ | — | ✓ | — |
| Instance type (Scale Up/Down) | ✓ | — | ✓ | — |
| TAC node count (Scale In/Out) | ✓ | — | — | — |
| Replica Scale In/Out | — | — | ✓ | — |
| Enable DR | ✓ | — | ✓² | ✓² |
| Failover Automation Level | ✓ | ✓³ | ✓ | ✓⁴ |
| Volume Size | ✓ | ✓ | ✓ | ✓ |
| Volume IOPS / MBps | ✓ | ✓ | ✓ | ✓ |

- ¹ Can be changed between Single ↔ HA in OpenSQL LI
- ² Automatically determined by Topology in OpenSQL (HA → DR used, Single → DR not used)
- ³ Can be changed when initially configured to use DR
- ⁴ Can be changed when initially configured as HA

{% hint style="info" %}
**Note**

- Volume Size can only be changed to a value larger than the current setting.
- When SE (Standard Edition) is selected, the instance type is limited to a maximum of 8 vCPU.
- OpenSQL is not supported in the AWS environment.
{% endhint %}

## Engine Options <a href="#engine-options" id="engine-options"></a>

DB Service Name, DB Engine Type, License Option, and Node Count display the current settings and cannot be changed.

The changeable items are as follows.

- **Edition**: Select between Standard Edition (SE) and Enterprise Edition (EE). SE can use up to 8 vCPU, and EE has no vCPU limit. In TAC or HA Topology, EE is automatically applied, and it can be changed only with the LI license.
- **Topology**: Can be changed between Single ↔ HA only in OpenSQL LI.

### TAC Scale In/Out <a href="#tac-scale-in-out" id="tac-scale-in-out"></a>

In the Tibero TAC configuration of the LI license model, you can adjust the node count by directly adding or deleting TAC nodes in the instance Scale In/Out table. The minimum is 2 and the maximum is 4.

{% hint style="info" %}
**Note**

TAC node Scale In/Out is provided only by the Tibero engine. OpenSQL adjusts nodes with Replica Scale In/Out in the DR configuration step.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you delete (Scale In) a running TAC instance, all data recorded on that instance is deleted.
{% endhint %}

## DR Configuration <a href="#dr-configuration" id="dr-configuration"></a>

- **Enable DR**: Select whether to use DR. It can be changed directly only in Tibero LI, while OpenSQL is automatically determined by Topology (HA → DR used, Single → DR not used). BYOL cannot be changed.
- **Failover Automation Level**: Selects the failover automation level when DR is used. It is not displayed when DR is not used.

### Failover Automation Level options <a href="#failover-automation-level" id="failover-automation-level"></a>

<table><thead><tr><th>Level</th><th>Name</th><th>Description</th></tr></thead><tbody><tr><td>Level 0</td><td>Manual</td><td>When a failure occurs, the user manually promotes the Standby/Replica to Primary/Leader</td></tr><tr><td>Level 1</td><td>Auto Failover</td><td><ul><li>The system switches automatically</li><li>Recovery and resource optimization are performed manually</li></ul></td></tr><tr><td>Level 2</td><td>Auto Rebuild</td><td><ul><li>After failover, a new Standby/Replica is automatically created to maintain the configuration</li><li>Data recovery is performed manually</li></ul></td></tr><tr><td>Level 3</td><td>Full Automation</td><td>The entire process from failover to recovery and cleanup of unused resources is handled automatically</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Stage 3 (Full Automation) prioritizes recovery speed, so some recent data may be lost.
{% endhint %}

{% hint style="info" %}
**Note**

The selectable stages vary depending on the license type.

- **Tibero LI**: Stages 0 through 3 are all selectable
- **Tibero BYOL**: Stage 0, Stage 2, and Stage 3 are selectable
- **OpenSQL**: Stage 0 and Stage 3 are selectable
{% endhint %}

### OpenSQL HA Scale In/Out <a href="#opensql-ha-scale-in-out" id="opensql-ha-scale-in-out"></a>

OpenSQL on the LI license model adjusts its configuration by adding or deleting Replica nodes in the Replica Scale In/Out table. Only Replica Node #2 and above can be deleted.

{% hint style="info" %}
**Note**

Replica Scale In/Out is provided only by the OpenSQL engine. Tibero adjusts nodes through TAC Scale In/Out in the engine options stage.
{% endhint %}

{% hint style="warning" %}
**Caution**

- If you change DR to disabled and then complete the spec change, all data on the existing Standby/Replica instances will be deleted.
- If an instance in the Retired state exists due to a Failover, changing DR to disabled will automatically delete that instance. Since data recovery through that instance will no longer be possible, complete your data review and backup before proceeding.
{% endhint %}

## AZ Configuration <a href="#az-configuration" id="az-configuration"></a>

Check and set the Availability Zone (AZ) for each instance. Settings can only be configured for newly added instances.

## Instance Configuration <a href="#instance-configuration" id="instance-configuration"></a>

Perform Scale Up/Down by changing the instance type. BYOL licenses cannot change the instance type.

The storage-related settings are as follows.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Volume Size</td><td>Can only be changed to a value larger than the current setting</td></tr><tr><td>Volume IOPS / MBps</td><td>Can be set within the allowed range depending on the volume type in the Azure environment</td></tr><tr><td>Auto Scale</td><td>When enabled, the volume is automatically expanded when Data Volume usage reaches 90%</td></tr><tr><td>Maximum Expansion Limit</td><td><ul><li>Enter the maximum expandable size when using Auto Scale</li><li>Must enter at least 110% of the current Data Volume Size</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Volume size cannot be reduced. Set the maximum expansion limit carefully.
{% endhint %}

## Configuration Information Review <a href="#review-configuration" id="review-configuration"></a>

Compare and review the configuration before the change (left) and the configuration after the change (right). Changed items are shown in blue.

The estimated cost is displayed as hourly and monthly costs. The values are calculated based on the Seoul region, and the actual cost may vary depending on the region and actual usage.

After reviewing the information, clicking **Done** will result in different processing methods depending on the type of change.

<table><thead><tr><th>Condition</th><th>Action</th></tr></thead><tbody><tr><td>Includes instance Scale Up/Down</td><td><ul><li>Displays a modal notifying that a restart is required</li><li>Service may be temporarily interrupted during the restart process</li></ul></td></tr><tr><td>Includes only TAC Scale In/Out or storage expansion</td><td>Applied immediately without a restart</td></tr><tr><td>Changing the DR configuration while a Retired instance exists</td><td>Displays a modal notifying of the Retired instance to be deleted</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When instance Scale Up/Down and storage expansion are requested together, they are processed in the order of storage expansion → instance Scale Up/Down.
{% endhint %}
