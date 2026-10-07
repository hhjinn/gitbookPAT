OwlDB is a managed database platform that installs databases in cloud and on-premises environments and registers, integrates, and manages them as DB Services. On this page, you can review OwlDB's operating environments, key features, and supported engines and topologies.

# Operating Environment <a href="#operating-environment" id="operating-environment"></a>

## Cloud Environment Support <a href="#cloud-support" id="cloud-support"></a>

This is a method of dynamically creating and operating databases by utilizing cloud infrastructure resources. Based on IaC (Infrastructure as Code), it automates infrastructure provisioning and database configuration, allowing users to build and scale database environments of the desired specifications from the console.

## On-Premises Environment Support <a href="#on-premise-support" id="on-premise-support"></a>

This is a method of operating databases based on physical infrastructure resources such as servers, networks, and storage that the customer owns. It is designed to efficiently utilize fixed infrastructure resources and supports closed-network environments isolated from external networks. Through OwlDB, you can install a new database on a host or integrate an existing database already in operation as a management target of OwlDB for unified control.

## Scope of Provision by License <a href="#scope-by-license" id="scope-by-license"></a>

OwlDB differs in the environments and features it provides depending on the license.

<table><thead><tr><th>Category</th><th>OwlDB Operation</th><th>OwlDB Automation</th><th>OwlDB DBaaS</th></tr></thead><tbody><tr><td><strong>Provided Environment</strong></td><td><ul><li>On-Premise</li><li>Private Cloud</li><li>Public Cloud (Build Type)</li></ul></td><td></td><td>AWS, Azure<br>(Marketplace Subscription)</td></tr><tr><td><strong>Registered DB Operation Management</strong></td><td>○</td><td>○</td><td>X (*BYOL New Build**)</td></tr><tr><td><strong>Installation Automation</strong></td><td>X</td><td>○</td><td>○</td></tr></tbody></table>

{% hint style="info" %}
**Note**

OwlDB DBaaS can only manage DBs newly built through OwlDB, and does not support the method of registering DBs already in operation. **Transfer DB licenses you already own via the BYOL (Bring Your Own License) method**so that they can be applied to DBs newly built in OwlDB DBaaS.
{% endhint %}

# Key Features <a href="#key-features" id="key-features"></a>

Based on common management features generally used in both environments, OwlDB provides dedicated features optimized for the infrastructure characteristics of cloud and on-premises environments respectively.

## **Common Features** <a href="#common-features" id="common-features"></a>

<table><thead><tr><th>oeNNQWrBminc</th><th>b5kPa8lesNIR</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Status Inquiry</strong></td><td>Real-time check of database and instance operating status</td></tr><tr><td><strong>Monitoring & Alerts</strong></td><td><ul><li>Monitoring of key performance indicators and operational status</li><li>Immediate alert dispatch upon occurrence of anomalies or events</li></ul></td></tr><tr><td><strong>Migration</strong></td><td>Support for pre-compatibility verification and guide-based migration when switching between heterogeneous databases</td></tr><tr><td><strong>Account Management (RBAC)</strong></td><td>Separation of user-specific permissions and security management through role-based access control</td></tr></tbody></table>

{% tabs %}
{% tab title="Cloud-Specialized" %}
| yw1fBYaxgqoC | MTQwvqYzs5cF |
| --- | --- |
| **Feature** | **Description** |
| **Automated Provisioning** | Full automation from cloud resource creation to database architecture configuration |
| **Resource Scaling/Changes** | Scaling and changing instance specifications and storage in line with workload increases and decreases |
| **Cloud Snapshot Backup** | Backup and recovery based on CSP snapshot feature integration |
{% endtab %}
{% tab title="On-Premises-Specialized" %}
<table><thead><tr><th>pKFXsoZlPV23</th><th>qk1seDReMy50</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Installation/Registration</strong></td><td><ul><li>Remote deployment of new DBs to customer hosts</li><li>Registration of existing external DBs in operation as management targets</li></ul></td></tr><tr><td><strong>Infrastructure Resource Discovery</strong></td><td>Automatic collection of hardware specifications and configuration information through an Agent, and status assessment</td></tr><tr><td><strong>Physical Backup/Recovery</strong></td><td>Backup and recovery based on the database's own utilities (Tibero RMGR, etc.)</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

## Database Engine and Topology <a href="#engines-and-topologies" id="engines-and-topologies"></a>

The relational database (RDBMS) engine specifications supported by OwlDB and the architecture configuration per environment are as follows.

{% tabs %}
{% tab title="Cloud" %}
### Azure <a href="#azure" id="azure"></a>

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.6</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="On-Premise" %}
### OwlDB Operation <a href="#owldb-operation" id="owldb-operation"></a>

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>After Patch Set 7</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

### OwlDB Automation <a href="#owldb-automation" id="owldb-automation"></a>

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>Registration: After Patch Set 7</li><li>Installation: 7.2.6</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
