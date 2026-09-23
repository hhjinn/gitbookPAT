# Service Overview

OwlDB is a managed database service that installs databases in the cloud and on-premises environments. It also registers, integrates, and manages them as DB Services. This page describes OwlDB's operating environments, key features, and supported engines and topologies.

## Operating Environments

### Cloud Environment Support

This method dynamically creates and operates databases by using cloud infrastructure resources. Based on IaC (Infrastructure as Code), it automates infrastructure provisioning and database configuration. Users can build and scale database environments with their desired specifications through the console.

### On-Premises Environment Support

This method operates databases on customer-owned physical infrastructure, such as servers, networks, and storage. It efficiently uses fixed infrastructure resources and supports closed networks isolated from external networks. Through OwlDB, you can install a new database on a host or integrate an existing database as an OwlDB management target for unified control.

## Key Features

In addition to management features used in both environments, OwlDB provides dedicated features optimized for cloud and on-premises infrastructure.

**Common Features**

<table data-full-width="true"><thead><tr><th>Feature</th><th>Description</th></tr></thead><tbody><tr><td><strong>Database Status Inquiry</strong></td><td>Real-time verification of database and instance operational status</td></tr><tr><td><strong>Monitoring &#x26; Alerts</strong></td><td><ul><li>Monitoring of key performance indicators and operational status</li><li>Immediate alert delivery when anomalies or events occur</li></ul></td></tr><tr><td><strong>Migration</strong></td><td>Support for pre-migration compatibility validation and guided migration between heterogeneous databases</td></tr><tr><td><strong>Account Management (RBAC)</strong></td><td>Per-user privilege separation and security management through role-based access control</td></tr></tbody></table>

{% tabs %}
{% tab title="Cloud-Specific" %}
<table><thead><tr><th>Feature</th><th width="315">Description</th></tr></thead><tbody><tr><td><strong>Automated Provisioning</strong></td><td>Full automation from cloud resource creation through database architecture configuration</td></tr><tr><td><strong>Resource Scaling/Modification</strong></td><td>Instance and storage scaling aligned with workload changes</td></tr><tr><td><strong>Cloud Snapshot Backup</strong></td><td>Backup and recovery through CSP snapshot feature integration</td></tr></tbody></table>
{% endtab %}

{% tab title="On-Premises-Specific" %}
<table data-full-width="true"><thead><tr><th width="315">Feature</th><th>Description</th></tr></thead><tbody><tr><td><strong>Database Installation/Registration</strong></td><td><ul><li>Remote deployment of new databases to customer hosts</li><li>Registration of existing external databases as management targets</li></ul></td></tr><tr><td><strong>Infrastructure Resource Discovery</strong></td><td>Automatic collection of hardware specifications and configuration information, plus status assessment through the Agent</td></tr><tr><td><strong>Physical Backup/Recovery</strong></td><td>Backup and recovery using the database's own utilities (Tibero RMGR, etc.)</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### Database Engines and Topologies

The following relational database management system (RDBMS) engines and architecture configurations are supported by OwlDB.

{% tabs %}
{% tab title="Cloud" %}
#### AWS

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr></tbody></table>

#### Azure

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}

{% tab title="On-Premises" %}
#### OwlDB Operation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>Patch set 7 and later</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0 (PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

#### OwlDB Automation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>Registered: Patch set 7 and later</li><li>Installed: 7.2.5</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0 (PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
