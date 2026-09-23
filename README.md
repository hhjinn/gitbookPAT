# Service Overview

OwlDB is a managed database platform that deploys databases in cloud and on-premises environments. It registers and integrates them as managed database services. This page describes OwlDB's operating environments, key features, and supported engines and topologies.

## Operating Environments

### Cloud Environment Support

OwlDB dynamically creates and operates databases using cloud infrastructure resources. Using infrastructure as code (IaC), it automates infrastructure provisioning and database configuration. You can build and scale database environments to your required specifications from the console.

### On-Premises Environment Support

This method operates databases on customer-owned physical infrastructure, including servers, networks, and storage. It efficiently uses fixed infrastructure resources and supports closed networks isolated from external networks. Through OwlDB, you can install a new database on a host or register an existing database as a managed target.

## Key Features

OwlDB provides common management features across both environments. It also provides features optimized for cloud and on-premises infrastructure.

**Common Features**

| Feature                       | Description                                                                                                          |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Database Status Query**     | Real-time verification of database and instance status                                                               |
| **Monitoring & Alerts**       | <p>- Monitors key performance metrics and operational status<br>- Sends immediate alerts for anomalies or events</p> |
| **Migration**                 | Pre-migration compatibility validation and guided migration support between heterogeneous databases                  |
| **Account Management (RBAC)** | Per-user privilege separation and security management through role-based access control                              |

{% tabs %}
{% tab title="Cloud-Specific" %}
| Feature                               | Description                                                                               |
| ------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Automated Provisioning**            | End-to-end automation from cloud resource creation to database architecture configuration |
| **Resource Scaling and Modification** | Instance and storage scaling aligned with workload changes                                |
| **Cloud Snapshot Backup**             | Backup and recovery by integrating CSP snapshot features                                  |
{% endtab %}

{% tab title="On-Premises-Specific" %}
| Feature                                    | Description                                                                                                             |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| **Database Installation and Registration** | <p>- Remotely deploys new databases to customer hosts<br>- Registers existing external databases as managed targets</p> |
| **Infrastructure Resource Discovery**      | Automatically collects hardware specifications and configuration data, and identifies status through the agent          |
| **Physical Backup and Recovery**           | Backup and recovery using native database utilities, such as Tibero RMGR                                                |
{% endtab %}
{% endtabs %}

### Database Engines and Topologies

The following tables list the RDBMS engine specifications and topologies supported in each environment.

{% tabs %}
{% tab title="Cloud" %}
#### AWS

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr></tbody></table>

#### Azure

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}

{% tab title="On-Premises" %}
#### OwlDB Operation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td>Tibero 7 patch set or later</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0 (PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

#### OwlDB Automation

<table data-full-width="true"><thead><tr><th>Database Engine</th><th>Version</th><th>Topology</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>Registered: Tibero 7 patch set or later</li><li>Installed: 7.2.5</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0 (PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
