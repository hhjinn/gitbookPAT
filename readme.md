OwlDB is a managed database platform that installs databases in cloud and on-premises environments and registers and integrates them as DB Services for centralized management. On this page, you can review OwlDB's operating environments, key features, and supported engines and topologies.

# Operating Environments

## Cloud Environment Support

This is a method of dynamically creating and operating databases by utilizing cloud infrastructure resources. Based on IaC (Infrastructure as Code), it automates infrastructure provisioning and database configuration, allowing users to build and scale database environments with the specifications they want from the console.

## On-Premises Environment Support

This is a method of operating databases based on physical infrastructure resources such as servers, networks, and storage that the customer owns. It is designed to efficiently utilize fixed infrastructure resources and supports closed-network environments isolated from external networks. Through OwlDB, you can install a new database on a host, or integrate an existing database already in operation as a managed target of OwlDB for unified control.

# Key Features

Based on common management features universally used in both environments, OwlDB provides dedicated features optimized for the infrastructure characteristics of cloud and on-premises environments respectively.

**Common Features**

<table><thead><tr><th>oeNNQWrBminc</th><th>b5kPa8lesNIR</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Status Inquiry</strong></td><td>Real-time verification of database and instance operating status</td></tr><tr><td><strong>Monitoring & Alerts</strong></td><td><ul><li>Monitoring of key performance indicators and operating status</li><li>Immediate alert dispatch upon occurrence of anomalies or events</li></ul></td></tr><tr><td><strong>Migration</strong></td><td>Pre-compatibility verification and guide-based migration support when switching between heterogeneous databases</td></tr><tr><td><strong>Account Management (RBAC)</strong></td><td>Per-user permission separation and security management through role-based access control</td></tr></tbody></table>

{% tabs %}
{% tab title="Cloud-Specific" %}
| yw1fBYaxgqoC | MTQwvqYzs5cF |
| --- | --- |
| **Feature** | **Description** |
| **Automated Provisioning** | Automation of the entire process, from cloud resource creation to database architecture configuration |
| **Resource Scaling/Modification** | Scaling and modification of instance specifications and storage in line with workload increases and decreases |
| **Cloud Snapshot Backup** | Backup and recovery based on CSP snapshot feature integration |
{% endtab %}
{% tab title="On-Premises-Specific" %}
<table><thead><tr><th>pKFXsoZlPV23</th><th>qk1seDReMy50</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Installation/Registration</strong></td><td><ul><li>Remote deployment of new DBs on customer hosts</li><li>Registration of existing external DBs in operation as managed targets</li></ul></td></tr><tr><td><strong>Infrastructure Resource Discovery</strong></td><td>Automatic collection of hardware specification and configuration information and status assessment via Agent</td></tr><tr><td><strong>Physical Backup/Recovery</strong></td><td>Backup and recovery based on the database's own utilities (Tibero RMGR, etc.)</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

## Database Engines and Topologies

The relational database (RDBMS) engine specifications supported by OwlDB and the architecture configurations for each environment are as follows.

{% tabs %}
{% tab title="Cloud" %}
### Azure

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.6</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="On-Premise" %}
### OwlDB Operation

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7 patchset and later</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

### OwlDB Automation

<table><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>Registration: 7 patchset and later</li><li>Installation: 7.2.6</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
