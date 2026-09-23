Provides information for using OwlDB on AWS Marketplace.

## Cloud Environment and Server Specifications

Check the cloud environment and server specifications in which OwlDB is provided.

| Item | Details |
| --- | --- |
| Compute Specifications | 2 vCPU, Memory 8 GiB |
| Operating System | Rocky 9.5 |
| Storage Engine | Amazon gp3 |
| Supported Languages | Korean, English |
| Recommended Browser | Google Chrome |
| Optimal Resolution | Full HD (1920*1080) |

## Region Availability

Check the supported regions and region codes.

| Region Name | Region |
| --- | --- |
| US West (Oregon) | us-west-2 |
| US West (N. California) | us-west-1 |
| US East (Ohio) | us-east-2 |
| US East (N. Virginia) | us-east-1 |
| South America (São Paulo) | sa-east-1 |
| Europe (Paris) | eu-west-3 |
| Europe (London) | eu-west-2 |
| Europe (Ireland) | eu-west-1 |
| Europe (Stockholm) | eu-north-1 |
| Europe (Frankfurt) | eu-central-1 |
| Canada (Central) | ca-central-1 |
| Asia Pacific (Sydney) | ap-southeast-2 |
| Asia Pacific (Singapore) | ap-southeast-1 |
| Asia Pacific (Mumbai) | ap-south-1 |
| Asia Pacific (Osaka) | ap-northeast-3 |
| Asia Pacific (Seoul) | ap-northeast-2 |
| Asia Pacific (Tokyo) | ap-northeast-1 |

## Instance Type

Check the vCPU and memory specifications for each selectable instance type.

| Instance Type | vCPU | Memory (GiB) |
| --- | --- | --- |
| t3.medium | 2 | 4 |
| t3.large | 2 | 8 |
| t3.xlarge | 4 | 16 |
| t3.2xlarge | 8 | 32 |
| m6i.large | 2 | 8 |
| m6i.xlarge | 4 | 16 |
| m6i.2xlarge | 8 | 32 |
| m6i.4xlarge | 16 | 64 |
| m6i.8xlarge | 32 | 128 |
| m6i.12xlarge | 48 | 192 |
| m6i.16xlarge | 64 | 256 |
| m6i.24xlarge | 96 | 384 |
| r6i.large | 2 | 16 |
| r6i.xlarge | 4 | 32 |
| r6i.2xlarge | 8 | 64 |
| r6i.4xlarge | 16 | 128 |
| r6i.8xlarge | 32 | 256 |
| r6i.12xlarge | 48 | 384 |
| r6i.16xlarge | 64 | 512 |
| r6i.24xlarge | 96 | 768 |
| r5.large | 2 | 16 |
| r5.xlarge | 4 | 32 |
| r5.2xlarge | 8 | 64 |
| r5.4xlarge | 16 | 128 |
| r5.8xlarge | 32 | 256 |
| r5.12xlarge | 48 | 384 |
| r5.16xlarge | 64 | 512 |
| r5.24xlarge | 96 | 768 |

{% hint style="info" %}
**Note**

- **Tibero Single**: The large or higher instance type is recommended.
- **Tibero TAC**: Only large or higher can be used, and xlarge or higher is recommended.
{% endhint %}

## Storage/Disk Type

Check the storage types available in OwlDB and the capacity and IOPS range of each type.

<table data-full-width="true"><thead><tr><th>Type</th><th>Characteristics</th><th>Volume size (GiB)</th><th>Volume IOPS (count)</th></tr></thead><tbody><tr><td>gp3</td><td><ul><li>SSD-based volume</li><li>High IOPS, low latency</li><li>Low cost per capacity</li></ul></td><td>100 ~ 65,536</td><td>3,000 ~ 80,000</td></tr><tr><td>gp2</td><td><ul><li>SSD-based volume</li><li>Suitable for storing and processing large volumes of data</li><li>Low cost per capacity</li><li>Consistency deviation present</li><li>IOPS changes based on allocated storage size; not user-configurable</li></ul></td><td>100 ~ 16,384</td><td>450 ~ 16,000</td></tr><tr><td>io2</td><td><ul><li>SSD-based volume</li><li>Very high IOPS, consistent performance</li><li>High cost per capacity</li><li>Relatively high latency</li></ul></td><td>100 ~ 65,536</td><td>3,000 ~ 256,000</td></tr></tbody></table>

{% hint style="info" %}
**Note**

The Tibero TAC topology can only use io2.
{% endhint %}
