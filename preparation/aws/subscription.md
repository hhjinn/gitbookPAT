# Marketplace Subscription Guide

## OwlDB Subscription Prerequisites

This page explains the OS image subscription and SSH Key creation tasks that must be completed before subscribing to OwlDB.

### 1. OS Image Subscription

To build a database through OwlDB, a prior subscription to the `Rocky Linux 9 (Official) - x86_64` AMI is required. The subscription method is as follows.

1. On [AWS Marketplace](https://aws.amazon.com/marketplace), search for and select [Rocky Linux 9 (Official) - x86\_64](https://aws.amazon.com/marketplace/pp/prodview-ygp66mwgbl2ii).
2. Click **Continue to Subscribe** to subscribe.

### 2. SSH Key Creation

Before subscribing to OwlDB, you must create an SSH Key pair resource. The SSH Key pair is used in the deployment of OwlDB and the database provisioning process.

1. On [AWS Console](https://console.aws.amazon.com/console/home), search for and select Key pairs.
2. In the upper-right corner, click **Create key pair**.
3. Enter the desired Key pair name in Name.
4. Select the Key pair type. For compatibility, the default RSA setting is recommended.
5. Select the Private key file format. For compatibility, the default `.pem` format is recommended.
6. In Tags, add tags for efficient resource management.
7. Click **Create key pair** to complete the creation and download the SSH Key pair file.

{% hint style="warning" %}
**Caution**

For security, the SSH Key pair file can only be downloaded once. Be sure to store the downloaded file in a secure location.
{% endhint %}

***

## Subscribing to OwlDB on AWS Marketplace

This page explains how to subscribe to OwlDB on AWS Marketplace and begin the deployment configuration.

1. On [AWS Marketplace](https://aws.amazon.com/marketplace), search for and select OwlDB.
2. Click **Continue to Subscribe** to subscribe.
3. Click **Continue to Configuration** to begin configuring the OwlDB deployment.

***

## Deploying OwlDB via AWS CloudFormation

This page explains the process of deploying OwlDB using AWS CloudFormation step by step.

### 1. Configure this software

* Select the items below, then click **Continue to Launch**.

| Item               | Option               |
| ------------------ | -------------------- |
| Fulfillment option | OwlDB for Tibero7    |
| Software version   | 1.2.0 (Dec 29, 2025) |
| Region             | Select               |

### 2. Launch this software

* Check the Configuration details.
* In Choose Action, select Launch CloudFormation, then click **Launch**.

### 3. Creating a CloudFormation Stack

Creating a CloudFormation stack requires going through all four steps below in order.

#### 3-1. Creating a Stack

{% hint style="warning" %}
**Caution**

In this step, you must use the options selected by default as they are.
{% endhint %}

In the template preparation item of the prerequisites, use Choose an existing template.

**Specify template**

| Item            | Option                         |
| --------------- | ------------------------------ |
| Template source | Amazon S3 URL                  |
| Amazon S3 URL   | Use the Default template as is |

#### 3-2. Specifying Stack Details

**Provide a stack name**

| Item       | Description                                   | Remarks                                      |
| ---------- | --------------------------------------------- | -------------------------------------------- |
| Stack name | Unique identifier of the stack to be deployed | Cannot duplicate a stack name already in use |

**Parameters \[Fulfillment option : Deploy into new VPC]**

OwlDB Infra Setting

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>VPC CIDR</td><td>Define the IP address range (CIDR block) of the VPC to be newly created</td><td>Cannot use a range identical to or overlapping with an existing VPC</td></tr><tr><td>Availability Zone 1</td><td>Target Availability Zone where infrastructure resources will be deployed</td><td>Must be an AZ within the selected Region</td></tr><tr><td>Public Subnet 1 CIDR</td><td><ul><li>CIDR range of the Public Subnet to be used within the specified VPC</li><li>Public Subnet: A network that can communicate externally through an Internet Gateway</li></ul></td><td>Must be a range included in the VPC CIDR</td></tr><tr><td>Availability Zone 2</td><td>Target Availability Zone where infrastructure resources will be deployed</td><td>Must be an AZ within the selected Region and must differ from Availability Zone 1</td></tr><tr><td>Public Subnet 2 CIDR</td><td>CIDR range of Public Subnet2 to be used within the specified VPC</td><td>Must be a range included in the VPC CIDR and must differ from Public Subnet 1 CIDR</td></tr><tr><td>CIDR Range for OwlDB Access</td><td>IP address range (CIDR) to allow inbound connections to the OwlDB instance</td><td>If access restriction is not required, enter 0.0.0.0/0</td></tr><tr><td>KeyPair Name</td><td>Key Pair name for accessing the OwlDB instance via SSH</td><td><ul><li>The key must be created in advance as an AWS EC2 Key Pair</li><li>The PEM file must be kept locally</li></ul></td></tr><tr><td>Image Id</td><td>Id of the Image on which OwlDB will be built</td><td>Use the Default as is</td></tr></tbody></table>

OwlDB Setting

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Remarks</th></tr></thead><tbody><tr><td>OwlDB Root User Name</td><td>ID of the default administrator account for logging in to OwlDB</td><td><ul><li>Default: admin</li><li>Cannot be changed after configuration</li></ul></td></tr></tbody></table>

Personal Information

| Item                         | Description                                    | Remarks                                  |
| ---------------------------- | ---------------------------------------------- | ---------------------------------------- |
| User Email                   | Email for service usage and account management | Consent to personal data use is required |
| Consent to Personal Data Use | Consent to personal data use                   | -                                        |

#### 3-3. Configuring Stack Options

{% hint style="info" %}
**Note**

In this step, use the default options as is.
{% endhint %}

| Item                         | Description                                                                                       |
| ---------------------------- | ------------------------------------------------------------------------------------------------- |
| Tags (Optional)              | Add tags for configuring, identifying, and classifying resources (up to 50 per stack)             |
| Permissions (Optional)       | Specify the role to be used by the stack with IAM                                                 |
| Stack Failure Options        | Select the behavior on provisioning failure and how resources created during rollback are deleted |
| Advanced Settings (Optional) | Configure additional options such as stack notification options and policies                      |
| Capabilities                 | Acknowledge the creation of IAM resources by CloudFormation                                       |

#### 3-4. Review and Create

Check and review the specified template and stack details.

Click **Submit** to start provisioning.

***

## OwlDB Connection Guide

This page explains how to connect for the first time after OwlDB deployment is complete and how to check the connection URL.

### First Connection

Once OwlDB deployment is complete in the Marketplace, a guide email containing the OwlDB connection address and account information is sent to the email entered during stack creation. Connect to OwlDB through that email. If you do not receive the email, contact us at [aws\_owldb\_support@tibero.com](mailto:aws_owldb_support@tibero.com).

### URL Connection

{% hint style="info" %}
**Note**

The connection address follows the `https://<고객별-고유-도메인>.owl-db.com` format.
{% endhint %}

The unique domain value for each customer can be checked using the following methods.

1. Check it in the CloudFormation console > Stacks > Stack name > Outputs tab > ManagementPlaneStackId.
2. Refer to the hosting zone name corresponding to owldb among the hosted zones in the Route 53 service.
