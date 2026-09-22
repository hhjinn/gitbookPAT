OwlDB is a managed database platform that installs databases in cloud and on-premises environments and registers, integrates, and manages them as a DB Service. This page describes OwlDB's operating environments, key features, and supported engines and topologies.

# Operating Environments

## Cloud Environment Support

This method leverages cloud infrastructure resources to dynamically create and operate databases. Based on IaC (Infrastructure as Code), it automates infrastructure provisioning and database configuration, allowing users to build and scale database environments with their desired specifications from the console.

## On-Premises Environment Support

This method operates databases based on physical infrastructure resources such as servers, networks, and storage owned by the customer. It is designed to efficiently utilize fixed infrastructure resources and supports closed-network environments isolated from external networks. Through OwlDB, you can install new databases on hosts or integrate existing databases already in operation as OwlDB management targets for unified control.

# Key Features

Based on common management features universally used across both environments, OwlDB provides dedicated features optimized for the respective infrastructure characteristics of cloud and on-premises.

**Common Features**

<table data-full-width="true"><thead><tr><th>oeNNQWrBminc</th><th>b5kPa8lesNIR</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Status Query</strong></td><td>Real-time verification of database and instance operational status</td></tr><tr><td><strong>Monitoring & Alerting</strong></td><td><ul><li>Monitoring of key performance metrics and operational status</li><li>Immediate alert dispatch when anomalies or events occur</li></ul></td></tr><tr><td><strong>Migration</strong></td><td>Pre-migration compatibility validation and guide-based migration support when transitioning between heterogeneous databases</td></tr><tr><td><strong>Account Management (RBAC)</strong></td><td>User-specific permission separation and security management through role-based access control</td></tr></tbody></table>

{% tabs %}
{% tab title="Cloud-Specific" %}
| yw1fBYaxgqoC | MTQwvqYzs5cF |
| --- | --- |
| **Feature** | **Description** |
| **Automated Provisioning** | Full-process automation from cloud resource creation to database architecture configuration |
| **Resource Scaling/Modification** | Instance specification and storage scaling and modification aligned with workload fluctuations |
| **Cloud Snapshot Backup** | Backup and recovery based on CSP snapshot feature integration |
{% endtab %}
{% tab title="On-Premises-Specific" %}
<table data-full-width="true"><thead><tr><th>pKFXsoZlPV23</th><th>qk1seDReMy50</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Database Installation/Registration</strong></td><td><ul><li>Remote deployment of new DBs to customer hosts</li><li>Registration of existing external DBs in operation as management targets</li></ul></td></tr><tr><td><strong>Infrastructure Resource Discovery</strong></td><td>Automatic collection of hardware specifications and configuration information through the Agent and status assessment</td></tr><tr><td><strong>Physical Backup/Recovery</strong></td><td>Backup and recovery based on the database's own utilities (Tibero RMGR, etc.)</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

## Database Engines and Topologies

The relational database (RDBMS) engine specifications supported by OwlDB and the architecture configurations for each environment are as follows.

{% tabs %}
{% tab title="Cloud" %}
### AWS

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr></tbody></table>

### Azure

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="On-Premise" %}
### OwlDB Operation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>After Patch Set 7</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0 (PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

### OwlDB Automation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>Registered: after 7 patch set</li><li>Installed: 7.2.5</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
