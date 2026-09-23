Provides information for using OwlDB on the Azure Marketplace.

## Cloud environment and server specifications

| Item | Details |
| --- | --- |
| Compute specifications | 2 vCPU, Memory 8 GiB |
| Operating system | Rocky 9.3 |
| Storage engine | Azure Premium SSD LRS |
| Supported languages | Korean, English |
| Recommended browser | Google Chrome |
| Optimal resolution | Full HD (1920*1080) |

## Region availability

| Region name | Region |
| --- | --- |
| Brazil South | brazilsouth |
| Central India | centralindia |
| East Asia | eastasia |
| Germany West Central | germanywestcentral |
| Korea Central | koreacentral |
| Korea South | koreasouth |
| Spain Central | spaincentral |
| Indonesia Central | indonesiacentral |
| New Zealand North | newzealandnorth |
| Qatar Central | qatarcentral |
| Australia East | australiaeast |
| Canada Central | canadacentral |
| North Europe | northeurope |
| West Europe | westeurope |
| France Central | francecentral |
| Italy North | italynorth |
| Japan East | japaneast |
| Poland Central | polandcentral |
| South Africa North | southafricanorth |
| Southeast Asia | southeastasia |
| Sweden Central | swedencentral |
| Switzerland North | switzerlandnorth |
| UAE North | uaenorth |
| UK South | uksouth |
| Central US | centralus |
| East US | eastus |
| West US 2 | westus2 |
| West US 3 | westus3 |

## Instance type

OwlDB supports multiple instance types to match workload requirements. Refer to the table below to check the vCPU and memory configurations before making a selection.

| Instance type | vCPU (CNT) | Memory (GiB) |
| --- | --- | --- |
| Standard_B2ls_v2 | 2 | 4 |
| Standard_B2s_v2 | 2 | 8 |
| Standard_B4s_v2 | 4 | 16 |
| Standard_B8s_v2 | 8 | 32 |
| Standard_D2s_v5 | 2 | 8 |
| Standard_D4s_v5 | 4 | 16 |
| Standard_D8s_v5 | 8 | 32 |
| Standard_D16s_v5 | 16 | 64 |
| Standard_D32s_v5 | 32 | 128 |
| Standard_D48s_v5 | 48 | 192 |
| Standard_D64s_v5 | 64 | 256 |
| Standard_D96s_v5 | 96 | 384 |
| Standard_E2s_v5 | 2 | 16 |
| Standard_E4s_v5 | 4 | 32 |
| Standard_E8s_v5 | 8 | 64 |
| Standard_E16s_v5 | 16 | 128 |
| Standard_E32s_v5 | 32 | 256 |
| Standard_E48s_v5 | 48 | 384 |
| Standard_E64s_v5 | 64 | 512 |
| Standard_E96s_v5 | 96 | 672 |
| Standard_E2s_v6 | 2 | 16 |
| Standard_E4s_v6 | 4 | 32 |
| Standard_E8s_v6 | 8 | 64 |
| Standard_E16s_v6 | 16 | 128 |
| Standard_E32s_v6 | 32 | 256 |
| Standard_E48s_v6 | 48 | 384 |
| Standard_E64s_v6 | 64 | 512 |
| Standard_E96s_v6 | 96 | 768 |

{% hint style="info" %}
**Note**

- Tibero Single: 2vCPU, Memory 8GiB or higher is recommended.
- Tibero TAC: Only 4vCPU or higher can be used, and 8vCPU or higher is recommended.
{% endhint %}

## Storage/disk type

Check the storage/disk types and size/IOPS ranges available for selection according to the workload.

<table data-full-width="true"><thead><tr><th>Type</th><th>Suitable workload</th><th>Disk Size (GiB)</th><th>Disk IOPS (IOPS)</th></tr></thead><tbody><tr><td>Ultra Disk</td><td>Storage option for Azure Virtual Machines (VM)<ul><li>Data-intensive workloads such as SAP HANA</li><li>High-transaction workloads</li></ul></td><td>100 ~ 65,536</td><td>3,000 ~ 400,000</td></tr><tr><td>Premium SSD v2</td><td>High-performance storage option for virtual machines and containers<ul><li>Big data analytics</li><li>Game execution</li></ul></td><td>100 ~ 65,536</td><td>3,000 ~ 80,000</td></tr></tbody></table>

{% hint style="info" %}
**Note**

The volume size for the Tibero TAC topology must be at least 200GiB or higher.
{% endhint %}
