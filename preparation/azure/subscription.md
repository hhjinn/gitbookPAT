# Subscribe to OwlDB on Azure Marketplace <a href="#subscribe-azure-marketplace" id="subscribe-azure-marketplace"></a>

This page describes the procedure for subscribing to OwlDB on Azure Marketplace.

1. [Azure Marketplace](https://azuremarketplace.microsoft.com/)Search for OwlDB in.
2. Select OwlDB from the search results.
3. **Get It Now**Click it.
4. **Continue**Click it to subscribe to the Subscription.
5. **Create**Click to begin the configuration for deploying OwlDB.

---

# Deploying OwlDB via ARM (Azure Resource Manager) Template <a href="#deploy-arm-template" id="deploy-arm-template"></a>

This page describes the parameters you enter to deploy OwlDB with an ARM Template.

## 1. Project details

Select the subscription and resource group for managing the deployed resources and costs. A Managed Application resource containing the OwlDB deployment information is created in the resource group.

{% hint style="info" %}
**Note**

The OwlDB service resources are deployed in a separate resource group automatically created by the Azure system, and are managed by the Publisher (operator).
{% endhint %}

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Subscription</td><td><ul><li><strong>Azure subscription account</strong></li><li>A collection of all resources, where all resources in a subscription are billed together</li></ul></td></tr><tr><td>Resource Group</td><td><ul><li><strong>Azure resource group</strong></li><li>A collection of resources that share the same lifecycle, permissions, and policies</li></ul></td></tr></tbody></table>

## 2. Instance details

Directly specify the parameters of the ARM (Azure Resource Manager) Template for OwlDB deployment.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Region</td><td>The region where OwlDB will be deployed</td><td>Check available regions</td></tr><tr><td>Project Name</td><td>The name of the project you want to deploy</td><td>Cannot duplicate a project name already in use</td></tr><tr><td>Vnet CIDR</td><td>The IP address range (CIDR block) of the VNet to be newly created</td><td>-</td></tr><tr><td>Availability Zone</td><td>The target Availability Zone where infrastructure resources will be deployed</td><td>AZ within the selected Region</td></tr><tr><td>Public Subnet CIDR</td><td><ul><li>The CIDR range of the Public Subnet to be used within the specified VNet</li><li>Public Subnet : a network capable of external communication through an internet gateway</li></ul></td><td>Must be a range included in the VNet CIDR</td></tr><tr><td>App Gateway Subnet CIDR</td><td>The CIDR range of the Azure Application Gateway to be used within the specified VNet</td><td><ul><li>Must be a range included in the VNet CIDR</li><li>Must be different from the Public Subnet CIDR</li></ul></td></tr><tr><td>OwlDB Ingress CIDR</td><td>The IP address range (CIDR) that will be allowed inbound access to the OwlDB instance</td><td>If access restriction is not required, <code>0.0.0.0/0</code> enter</td></tr><tr><td>SSH Public Key</td><td>The Key Pair name for accessing the OwlDB instance via SSH</td><td><ul><li>Must be created in advance as an SSH Key</li><li>The PEM file must be kept locally</li></ul></td></tr><tr><td>OwlDB Root Username</td><td>The ID of the default administrator account for logging in to OwlDB</td><td><ul><li><strong>Default : admin</strong></li><li>Cannot be changed after configuration</li></ul></td></tr><tr><td>User Email</td><td>Email for service usage and account management</td><td>Consent to the use of personal information is required</td></tr></tbody></table>

## 3. Managed Application Details

Specify the application's unique identifier and the resource group for resource management.

| Item | Description |
| --- | --- |
| Application Name | Application unique identifier |
| Managed Resource Group | Group for resource management |

---

# Required tasks after OwlDB deployment <a href="#post-deployment-tasks" id="post-deployment-tasks"></a>

This page describes the tasks that must be performed after OwlDB deployment.

## Create SSH Key <a href="#create-ssh-key" id="create-ssh-key"></a>

After OwlDB deployment, you must create an SSH Key resource within the automatically created OwlDB service resource group. The SSH Key is used in the OwlDB database provisioning process.

1. [Azure Portal](https://portal.azure.com/#home)In **SSH keys**Search for.
2. From the search results, **SSH keys**select.
3. Top left **Create**Click.
4. In Project details, select the resource group automatically created after OwlDB creation.
5. In Instance details, select the desired Key pair name and type.
6. In Tags, set the Tag for resource management.
7. **Next**Click to complete the creation.
8. Download the SSH Key file.

{% hint style="warning" %}
**Caution**

For security reasons, the SSH Key file can only be downloaded once. Be sure to store the downloaded file in a safe location.
{% endhint %}

---

# OwlDB Access Guide <a href="#access-owldb" id="access-owldb"></a>

This page explains how to connect to the deployed OwlDB.

## Initial Access <a href="#first-access" id="first-access"></a>

Once OwlDB deployment from the Marketplace is complete, [**The email account entered during OwlDB deployment**](#id-2.-instance-details)A notification email containing the OwlDB connection address and account information is sent to. You can connect to OwlDB through this email. If the email is not received, [OwlDB support team](mailto:azure_owldb_support@tibero.com)contact.

## URL Access <a href="#url-access" id="url-access"></a>

The OwlDB connection URL is in the format `https://<sub-domain>.owl-db.com` and `<sub-domain>`contains a unique domain value for each customer.

The customer-specific unique domain value can be checked using the following method.

- **Azure Portal > OwlDB Resource Group > Settings - Deployments > Deployment corresponding to OwlDB > Outputs > Sub Domain Name**Check in.
- **DNS zones** Refer to the hosting zone name corresponding to OwlDB among the service's resources.
