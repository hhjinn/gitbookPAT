# Required Tasks Before Subscribing to OwlDB <a href="#pre-subscription-tasks" id="pre-subscription-tasks"></a>

This page describes the OS image subscription and SSH Key creation tasks that must be completed before subscribing to OwlDB.

## 1. OS Image Subscription <a href="#os-image-subscription" id="os-image-subscription"></a>

To build a database through OwlDB, `Rocky Linux 9 (Official) - x86_64` a prior subscription to the AMI is required. The subscription method is as follows.

1. [AWS Marketplace](https://aws.amazon.com/marketplace)In [Rocky Linux 9 (Official) - x86_64](https://aws.amazon.com/marketplace/pp/prodview-ygp66mwgbl2ii)Search for and select it.
2. **Continue to Subscribe**Click it to subscribe to the Subscription.

## 2. SSH Key Creation <a href="#create-ssh-key" id="create-ssh-key"></a>

Before subscribing to OwlDB, you must create an SSH Key pair resource. The SSH Key pair is used in the deployment of OwlDB and the database provisioning process.

1. [AWS Console](https://console.aws.amazon.com/console/home)Search for and select Key pairs.
2. Top right **Create key pair**Click it.
3. Enter the desired Key pair name in Name.
4. Select the Key pair type. For compatibility, the default setting RSA is recommended.
5. Select the Private key file format. For compatibility, the default setting .pem format is recommended.
6. In Tags, set Tags for efficient resource management.
7. **Create key pair**Click it to complete the creation and download the SSH Key pair file.

{% hint style="warning" %}
**Caution**

For security, the SSH Key pair file can be downloaded only once. Be sure to store the downloaded file in a secure location.
{% endhint %}

---

# Subscribing to OwlDB on AWS Marketplace <a href="#subscribe-aws-marketplace" id="subscribe-aws-marketplace"></a>

This page explains how to subscribe to OwlDB on AWS Marketplace and begin the deployment configuration.

1. [AWS Marketplace](https://aws.amazon.com/marketplace)Search for and select OwlDB in.
2. **Continue to Subscribe**Click it to subscribe to the Subscription.
3. **Continue to Configuration**Click to begin the configuration for deploying OwlDB.

---

# Deploying OwlDB via AWS CloudFormation <a href="#deploy-cloudformation" id="deploy-cloudformation"></a>

This page explains the process of deploying OwlDB using AWS CloudFormation step by step.

## 1. Configure this software

- Select the items below, and **Continue to Launch**Click it.

| Item | Option |
| --- | --- |
| Fulfillment option | OwlDB for Tibero7 |
| Software version | 1.2.0 (Dec 29, 2025) |
| Region | Selection |

## 2. Launch this software

- Review the Configuration details.
- Select Launch CloudFormation in Choose Action, and **Launch**Click it.

## 3. Creating a CloudFormation Stack <a href="#create-cloudformation-stack" id="create-cloudformation-stack"></a>

Creating a CloudFormation stack requires going through all four steps below in order.

### 3-1. Creating the Stack <a href="#id-3-1" id="id-3-1"></a>

{% hint style="warning" %}
**Caution**

In this step, you must use the default selected options as they are.
{% endhint %}

For the Prepare template item under prerequisites, use Choose an existing template.

**Specify template**

| Item | Option |
| --- | --- |
| Template source | Amazon S3 URL |
| Amazon S3 URL | Use the Default template as is |

### 3-2. Specify Stack Details <a href="#id-3-2" id="id-3-2"></a>

**Provide a stack name**

| Item | Description | Remarks |
| --- | --- | --- |
| Stack name | Unique identifier of the stack you want to deploy | Cannot duplicate a stack name already in use |

**Parameter [Fulfillment option : Deploy into new VPC]**

OwlDB Infra Setting

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>VPC CIDR</td><td>Defines the IP address range (CIDR block) of the VPC to be newly created</td><td>Cannot use a range that is identical to or overlaps with an existing VPC</td></tr><tr><td>Availability Zone 1</td><td>The target Availability Zone where infrastructure resources will be deployed</td><td>Must be an AZ within the selected Region</td></tr><tr><td>Public Subnet 1 CIDR</td><td><ul><li>The CIDR range of the Public Subnet to be used within the specified VPC</li><li>Public Subnet : a network capable of external communication through an internet gateway</li></ul></td><td>Must be a range included in the VPC CIDR</td></tr><tr><td>Availability Zone 2</td><td>The target Availability Zone where infrastructure resources will be deployed</td><td>Must be an AZ within the selected Region and must differ from Availability Zone 1</td></tr><tr><td>Public Subnet 2 CIDR</td><td>The CIDR range of Public Subnet2 to be used within the specified VPC</td><td>Must be a range included in the VPC CIDR and must differ from the Public Subnet 1 CIDR</td></tr><tr><td>CIDR Range for OwlDB Access</td><td>The IP address range (CIDR) that will be allowed inbound access to the OwlDB instance</td><td>If access restriction is not required, enter 0.0.0.0/0</td></tr><tr><td>KeyPair Name</td><td>The Key Pair name for accessing the OwlDB instance via SSH</td><td><ul><li>The key must be created in advance as an AWS EC2 Key Pair</li><li>The PEM file must be kept locally</li></ul></td></tr><tr><td>Image Id</td><td>The ID of the Image on which OwlDB will be built</td><td>Use the Default as is</td></tr></tbody></table>

OwlDB Setting

<table><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>OwlDB Root User Name</td><td>The ID of the default administrator account for logging in to OwlDB</td><td><ul><li>Default : admin</li><li>Cannot be changed after configuration</li></ul></td></tr></tbody></table>

Personal Information

| Item | Description | Remarks |
| --- | --- | --- |
| User Email | Email for service usage and account management | Consent to the use of personal information is required |
| Consent to Personal Data Use | Consent to the use of personal information | - |

### 3-3. Configure Stack Options <a href="#id-3-3" id="id-3-3"></a>

{% hint style="info" %}
**Note**

In this step, use the default selected options as they are.
{% endhint %}

| Item | Description |
| --- | --- |
| Tags (optional) | Add tags for configuring, identifying, and classifying resources (up to 50 per stack) |
| Permissions (optional) | Specify the role to be used by the stack via IAM |
| Stack failure options | Select the behavior on provisioning failure and the method for deleting resources created during rollback |
| Advanced settings (optional) | Configure additional options such as stack notification options and policies |
| Feature | Approve the creation of IAM resources by CloudFormation |

### 3-4. Review and Create <a href="#id-3-4" id="id-3-4"></a>

Confirm and review the specified template and stack details.

**Submit**Click to start provisioning.

---

# OwlDB Access Guide <a href="#access-owldb" id="access-owldb"></a>

This page explains how to make the initial connection and how to check the access URL after OwlDB deployment is complete.

## Initial Access <a href="#first-access" id="first-access"></a>

Once OwlDB deployment from the Marketplace is complete, a guide email containing the OwlDB access address and account information is sent to the email entered when creating the stack. Access OwlDB through that email. If the email is not received, [aws_owldb_support@tibero.com](mailto:aws_owldb_support@tibero.com)contact.

## URL Access <a href="#url-access" id="url-access"></a>

{% hint style="info" %}
**Note**

The access address is in the `https://<per-customer-unique-domain>.owl-db.com` format.
{% endhint %}

Check the per-customer unique domain value using the following method.

1. Check it in CloudFormation console > Stacks > Stack name > Outputs tab > ManagementPlaneStackId.
2. Refer to the hosting zone name corresponding to owl db among the hosting zones of the Route 53 service.
