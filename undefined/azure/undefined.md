# Marketplace Subscription Guide

## Subscribing to OwlDB on Azure Marketplace

This page describes the procedure for subscribing to OwlDB on Azure Marketplace.

1. On [Azure Marketplace](https://azuremarketplace.microsoft.com/), search for OwlDB.
2. Select OwlDB from the search results.
3. Click **Get It Now**.
4. Click **Continue** to subscribe.
5. Click **Create** to begin configuring the OwlDB deployment.

***

## Deploying OwlDB via an ARM (Azure Resource Manager) Template

This page describes the parameters you enter to deploy OwlDB with an ARM Template.

### 1. Project details

Select the subscription and resource group for managing deployed resources and costs. A Managed Application resource containing the OwlDB deployment information is created in the resource group.

{% hint style="info" %}
**Note**

OwlDB service resources are deployed to a separate resource group automatically created by the Azure system, and are managed by the Publisher (operator).
{% endhint %}

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Subscription</td><td><ul><li><strong>Azure subscription account</strong></li><li>A collection of all resources; all resources in a subscription are billed together</li></ul></td></tr><tr><td>Resource Group</td><td><ul><li><strong>Azure resource group</strong></li><li>A collection of resources that share the same lifecycle, permissions, and policies</li></ul></td></tr></tbody></table>

### 2. Instance details

Directly specify the parameters of the ARM (Azure Resource Manager) Template for the OwlDB deployment.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Region</td><td>The region where OwlDB will be deployed</td><td>Check available regions</td></tr><tr><td>Project Name</td><td>The name of the project you want to deploy</td><td>Cannot duplicate a project name that is already in use</td></tr><tr><td>Vnet CIDR</td><td>The IP address range (CIDR block) of the new VNet to be created</td><td>-</td></tr><tr><td>Availability Zone</td><td>The target Availability Zone where the infrastructure resources will be deployed</td><td>An AZ within the selected Region</td></tr><tr><td>Public Subnet CIDR</td><td><ul><li>The CIDR range of the Public Subnet to be used within the specified VNet</li><li>Public Subnet: A network that can communicate externally through an internet gateway</li></ul></td><td>Must be a range included in the VNet CIDR</td></tr><tr><td>App Gateway Subnet CIDR</td><td>The CIDR range of the Azure Application Gateway to be used within the specified VNet</td><td><ul><li>Must be a range included in the VNet CIDR</li><li>Must be different from the Public Subnet CIDR</li></ul></td></tr><tr><td>OwlDB Ingress CIDR</td><td>The IP address range (CIDR) that will be allowed inbound access to the OwlDB instance</td><td>If access restriction is not required, enter <code>0.0.0.0/0</code></td></tr><tr><td>SSH Public Key</td><td>The name of the Key Pair used to access the OwlDB instance via SSH</td><td><ul><li>Must be created in advance as an SSH Key</li><li>The PEM file must be kept locally</li></ul></td></tr><tr><td>OwlDB Root Username</td><td>The ID of the default administrator account for logging in to OwlDB</td><td><ul><li><strong>Default: admin</strong></li><li>Cannot be changed after being set</li></ul></td></tr><tr><td>User Email</td><td>The email for service usage and account management</td><td>Consent to the use of personal information is required</td></tr></tbody></table>

### 3. Managed Application Details

Specify the application's unique identifier and the resource group for resource management.

| Item                   | Description                     |
| ---------------------- | ------------------------------- |
| Application Name       | Application unique identifier   |
| Managed Resource Group | A group for resource management |

***

## Required Tasks After Deploying OwlDB

This page describes the tasks that must be performed after deploying OwlDB.

### Creating an SSH Key

After deploying OwlDB, you must create an SSH Key resource within the automatically created OwlDB service resource group. The SSH Key is used during the OwlDB database provisioning process.

1. On [Azure Portal](https://portal.azure.com/#home), search for **SSH keys**.
2. From the search results, select **SSH keys**.
3. In the top-left corner, click **Create**.
4. In Project details, select the resource group that was automatically created after creating OwlDB.
5. In Instance details, select the desired Key pair name and type.
6. In Tags, set the Tags for resource management.
7. Click **Next** to complete the creation.
8. Download the SSH Key file.

{% hint style="warning" %}
**Caution**

For security reasons, the SSH Key file can be downloaded only once. Be sure to store the downloaded file in a safe location.
{% endhint %}

***

## OwlDB Connection Guide

This page describes how to connect to the deployed OwlDB.

### Initial Connection

Once the OwlDB deployment from the marketplace is complete, the [email account entered during OwlDB deployment](undefined.md#id-2.-instance-details) receives a guide email containing the OwlDB connection address and account information. You can connect to OwlDB through this email. If you do not receive the email, contact [the OwlDB support team](mailto:azure_owldb_support@tibero.com).

### URL Connection

The OwlDB connection URL follows the `https://<sub-domain>.owl-db.com` format, where `<sub-domain>` is a unique customer-specific domain value.

The customer-specific unique domain value can be verified using the following method.

* Verify it in **Azure portal > OwlDB resource group > Settings - Deployments > the Deployment corresponding to OwlDB > Outputs > Sub Domain Name**.
* In **DNS zones**, refer to the hosting zone name corresponding to OwlDB among the service resources.
