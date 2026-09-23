Changes the configuration of an operational Cloud DB service, such as the instance type, DR configuration, and storage.

Spec changes start from Management > Overview and proceed through five steps: Engine Options → DR Configuration → AZ Configuration → Instance Configuration → Review Configuration Details. In each step, you review the current settings and modify the necessary items, and in the final step, you compare the configuration and estimated cost before and after the change and request that it be applied. Depending on the changes, a restart of the DB service may be required.

The range of items that can be changed varies depending on the DB engine and license type (LI/BYOL). The LI license allows you to change most items, including the instance type, DR configuration, availability zone, and storage settings, while the BYOL license limits the scope of changes to storage items such as volume size, IOPS, and MBps.

{% hint style="info" %}
**Note**

In the Azure environment, only the BYOL license model is currently supported.
{% endhint %}

1. **Management > Overview**Navigate to.
2. **Change Spec**Click.
3. **Engine Options**, **DR Configuration**, **AZ Configuration**, **Instance Configuration** Navigate to each tab.
4. Set the items to be changed.
5. **Review Configuration Details** Navigate to the tab.
6. Review the configuration and estimated cost before and after the change.
7. **Complete**Click.
8. Review the details in the confirmation modal.
9. **Confirm**Click.

{% hint style="info" %}
**Note**

- Tabs for steps 1 through 4 can be freely navigated in any order.
- **Review Configuration Details** The tab can only be entered when there are no validation errors across all of the step 1 through 4 tabs.
- On the right side of the screen, **Configuration Details** In the floating box, you can review a summary of the content entered in each tab, and error items are displayed in red text.
{% endhint %}

## Changeable Items

The items that can be changed vary depending on the engine type and license option.

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

- ¹ In OpenSQL LI, switching between Single ↔ HA is possible
- ² OpenSQL is determined automatically based on Topology (HA → DR enabled, Single → DR disabled)
- ³ Can be changed when initially configuring DR usage
- ⁴ Can be changed during initial HA configuration

{% hint style="info" %}
**Note**

- Volume Size can only be changed to a value larger than the current setting.
- When SE (Standard Edition) is selected, the instance type is limited to a maximum of 8 vCPU.
- OpenSQL is not supported in the AWS environment.
{% endhint %}

## Engine Options

DB Service Name, DB Engine Type, License Option, and Node Count display the current settings and cannot be changed.

The changeable items are as follows.

- **Edition**: Select between Standard Edition (SE) and Enterprise Edition (EE). SE supports up to 8 vCPU, while EE has no vCPU limit. In TAC or HA Topology, EE is applied automatically, and it can only be changed with an LI license.
- **Topology**: Switching between Single ↔ HA is only possible in OpenSQL LI.

### TAC Scale In/Out

In the Tibero TAC configuration of the LI license model, you can adjust the number of nodes by directly adding or deleting TAC nodes in the instance Scale In/Out table. The minimum is 2 and the maximum is 4.

{% hint style="info" %}
**Note**

TAC node Scale In/Out is provided only by the Tibero engine. OpenSQL adjusts nodes through Replica Scale In/Out in the DR configuration stage.
{% endhint %}

{% hint style="warning" %}
**Caution**

If you delete (Scale In) an operating TAC instance, all data recorded on that instance will be deleted.
{% endhint %}

## DR Configuration

- **Enable DR**: Select whether to use DR. It can be changed directly only in Tibero LI, while OpenSQL is determined automatically based on Topology (HA → DR enabled, Single → DR disabled). BYOL cannot be changed.
- **Failover Automation Level**: Select the failover automation level when DR is used. It is not displayed when DR is not used.

### Failover Automation Level Options

<table data-full-width="true"><thead><tr><th>Level</th><th>Name</th><th>Description</th></tr></thead><tbody><tr><td>Level 0</td><td>Manual</td><td>When a failure occurs, the user directly promotes the Standby/Replica to Primary/Leader</td></tr><tr><td>Level 1</td><td>Auto Failover</td><td><ul><li>The system switches over automatically</li><li>Recovery and resource optimization are performed manually</li></ul></td></tr><tr><td>Level 2</td><td>Auto Rebuild</td><td><ul><li>After failover, a new Standby/Replica is automatically created to maintain the configuration</li><li>Data recovery is performed manually</li></ul></td></tr><tr><td>Level 3</td><td>Full Automation</td><td>The entire process from failover to recovery and cleanup of unused resources is handled automatically</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Level 3 (Full Automation) prioritizes recovery speed, so some recent data may be lost.
{% endhint %}

{% hint style="info" %}
**Note**

The selectable levels vary depending on the license type.

- **Tibero LI**: All levels 0–3 are selectable
- **Tibero BYOL**: Levels 0, 2, and 3 are selectable
- **OpenSQL**: Levels 0 and 3 are selectable
{% endhint %}

### OpenSQL HA Scale In/Out

For OpenSQL in the LI license model, you adjust the configuration by adding or deleting Replica nodes in the Replica Scale In/Out table. Only Replica Node #2 and above can be deleted.

{% hint style="info" %}
**Note**

Replica Scale In/Out is provided only by the OpenSQL engine. Tibero adjusts nodes through TAC Scale In/Out in the engine options stage.
{% endhint %}

{% hint style="warning" %}
**Caution**

- If you complete the spec change after changing DR to disabled, all data on the existing Standby/Replica instances will be deleted.
- If an instance in the Retired state exists due to a Failover, changing DR to disabled will automatically delete that instance. Since data recovery through that instance becomes impossible, proceed only after completing data review and backup.
{% endhint %}

## AZ Configuration

Check and set the Availability Zone (AZ) of each instance. Configuration is possible only for newly added instances.

## Instance Configuration

Perform Scale Up/Down by changing the instance type. With a BYOL license, the instance type cannot be changed.

The storage-related settings are as follows.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Volume Size</td><td>Can only be changed to a value larger than the current setting</td></tr><tr><td>Volume IOPS / MBps</td><td>Can be set within the allowed range depending on the volume type in the Azure environment</td></tr><tr><td>Auto Scale</td><td>When enabled, the volume is automatically expanded when Data Volume usage reaches 90%</td></tr><tr><td>Maximum Expansion Limit</td><td><ul><li>Enter the maximum expandable size when Auto Scale is used</li><li>Must enter at least 110% of the current Data Volume Size</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Volume size cannot be reduced. Set the maximum expansion limit carefully.
{% endhint %}

## Verify configuration information

Compare and verify the configuration before the change (left) and after the change (right). Changed items are displayed in blue.

The estimated cost is displayed as hourly and monthly costs. The values are calculated based on the Seoul region, and actual costs may vary depending on the region and actual usage.

After verifying the content **Complete**When you click, the processing method varies depending on the type of change.

<table data-full-width="true"><thead><tr><th>Condition</th><th>Behavior</th></tr></thead><tbody><tr><td>Includes instance Scale Up/Down</td><td><ul><li>Displays a modal notifying that a restart is required</li><li>Service may be temporarily interrupted during the restart process</li></ul></td></tr><tr><td>Includes only TAC Scale In/Out or storage expansion</td><td>Applied immediately without a restart</td></tr><tr><td>Changing the DR configuration while a Retired instance exists</td><td>Displays a modal notifying of the Retired instance to be deleted</td></tr></tbody></table>

{% hint style="info" %}
**Note**

When instance Scale Up/Down and storage expansion are requested together, they are processed in the order of storage expansion → instance Scale Up/Down.
{% endhint %}
