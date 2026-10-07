# Subscribing to OwlDB in Azure Marketplace <a href="#subscribe-azure-marketplace" id="subscribe-azure-marketplace"></a>

This page describes the procedure for subscribing to OwlDB in Azure Marketplace.

1. Search for OwlDB in [Azure Marketplace](https://azuremarketplace.microsoft.com/).
2. Select OwlDB from the search results.
3. Click **Get It Now**.
4. Click **Continue** to subscribe to the Subscription.
5. Click **Create** to begin the configuration for deploying OwlDB.

---

# Deploying OwlDB via ARM (Azure Resource Manager) Template <a href="#deploy-arm-template" id="deploy-arm-template"></a>

This page describes the parameters to enter in order to deploy OwlDB with an ARM Template.

## 1. Project details

Select the subscription and resource group for managing the deployed resources and costs. A Managed Application resource that holds the OwlDB deployment information is created in the resource group.

{% hint style="info" %}
**Note**

The OwlDB service resources are deployed in a separate resource group that is automatically created by the Azure system, and are managed by the Publisher (operator).
{% endhint %}

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Subscription</td><td><ul><li><strong>Azure subscription account</strong></li><li>A collection of all resources; all resources in a subscription are billed together</li></ul></td></tr><tr><td>Resource Group</td><td><ul><li><strong>Azure resource group</strong></li><li>A collection of resources that share the same lifecycle, permissions, and policies</li></ul></td></tr></tbody></table>

## 2. Instance details

Directly specify the parameters of the ARM (Azure Resource Manager) Template for deploying OwlDB.

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>Region</td><td>The region where OwlDB will be deployed</td><td>Check the available regions</td></tr><tr><td>Project Name</td><td>The name of the project to deploy</td><td>Cannot be a duplicate of a project name already in use</td></tr><tr><td>Vnet CIDR</td><td>The IP address range (CIDR block) of the VNet to be newly created</td><td>-</td></tr><tr><td>Availability Zone</td><td>Target Availability Zone where the infrastructure resources will be deployed</td><td>The AZ within the selected Region</td></tr><tr><td>Public Subnet CIDR</td><td><ul><li>The CIDR range of the Public Subnet to be used within the specified VNet</li><li>Public Subnet: a network that can communicate externally through an internet gateway</li></ul></td><td>Must be a range included in the VNet CIDR</td></tr><tr><td>App Gateway Subnet CIDR</td><td>The CIDR range of the Azure Application Gateway to be used within the specified VNet</td><td><ul><li>Must be a range included in the VNet CIDR</li><li>Must be different from the Public Subnet CIDR</li></ul></td></tr><tr><td>OwlDB Ingress CIDR</td><td>IP address range (CIDR) that will be allowed inbound access to the OwlDB instance</td><td>If access restriction is not required, enter <code>0.0.0.0/0</code></td></tr><tr><td>SSH Public Key</td><td>Name of the Key Pair for accessing the OwlDB instance via SSH</td><td><ul><li>Must be created in advance as an SSH Key</li><li>The PEM file must be kept locally</li></ul></td></tr><tr><td>OwlDB Root Username</td><td>ID of the default administrator account for logging in to OwlDB</td><td><ul><li><strong>Default : admin</strong></li><li>Cannot be changed after it is set</li></ul></td></tr><tr><td>User Email</td><td>Email for service use and account management</td><td>Consent to the use of personal information is required</td></tr></tbody></table>

## 3. Managed Application Details

Specify the application's unique identifier and the resource group for resource management.

| Item | Description |
| --- | --- |
| Application Name | Application unique identifier |
| Managed Resource Group | Group for resource management |

---

# Required tasks after deploying OwlDB <a href="#post-deployment-tasks" id="post-deployment-tasks"></a>

This page describes the tasks that must be performed after deploying OwlDB.

## Creating an SSH Key <a href="#create-ssh-key" id="create-ssh-key"></a>

After deploying OwlDB, you must create an SSH Key resource within the automatically created OwlDB service resource group. The SSH Key is used in the OwlDB database provisioning process.

1. Search for **SSH keys** in the [Azure Portal](https://portal.azure.com/#home).
2. Select **SSH keys** from the search results.
3. Click **Create** in the upper left.
4. In Project details, select the resource group that was automatically created after OwlDB was created.
5. In Instance details, select the desired Key pair name and type.
6. In Tags, set the Tags for resource management.
7. Click **Next** to complete the creation.
8. Download the SSH Key file.

{% hint style="warning" %}
**Caution**

For security reasons, the SSH Key file can be downloaded only once. Be sure to store the downloaded file in a secure location.
{% endhint %}

---

# OwlDB Connection Guide <a href="#access-owldb" id="access-owldb"></a>

This page explains how to connect to the deployed OwlDB.

## First Connection <a href="#first-access" id="first-access"></a>

Once OwlDB deployment from the Marketplace is complete, a notification email containing the OwlDB connection address and account information is sent to [**the email account entered when deploying OwlDB**](#id-2.-instance-details). You can connect to OwlDB through this email. If you do not receive the email, contact the [OwlDB support team](mailto:azure_owldb_support@tibero.com).

## URL Connection <a href="#url-access" id="url-access"></a>

The OwlDB connection URL is in the format `https://<sub-domain>.owl-db.com`, where `<sub-domain>` contains a domain value unique to each customer.

The domain value unique to each customer can be checked using the following methods.

- Check it in **Azure Portal > OwlDB resource group > Settings - Deployments > the Deployment corresponding to OwlDB > Outputs > Sub Domain Name**.
- Refer to the hosting zone name corresponding to OwlDB among the resources of the **DNS zones** service.
