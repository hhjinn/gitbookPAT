# Service Overview

OwlDB is a managed database platform that installs databases in cloud and on-premises environments and registers and integrates them as DB Services for centralized management. On this page, you can review OwlDB's operating environments, key features, and supported engines and topologies.

## Operating Environments

### Cloud Environment Support

This is a method of dynamically creating and operating databases by utilizing cloud infrastructure resources. Based on IaC (Infrastructure as Code), it automates infrastructure provisioning and database configuration, allowing users to build and scale database environments with the specifications they want from the console.

### On-Premises Environment Support

This is a method of operating databases based on physical infrastructure resources such as servers, networks, and storage that the customer owns. It is designed to efficiently utilize fixed infrastructure resources and supports closed-network environments isolated from external networks. Through OwlDB, you can install a new database on a host, or integrate an existing database already in operation as a managed target of OwlDB for unified control.

### Coverage by License

OwlDB differs in its provided environments and features depending on the license.

| Category                               | OwlDB Operation                                                                      | OwlDB Automation | OwlDB DBaaS                                     |
| -------------------------------------- | ------------------------------------------------------------------------------------ | ---------------- | ----------------------------------------------- |
| **Provided Environment**               | <ul><li>On-Premise</li><li>Private Cloud</li><li>Public Cloud (self-built)</li></ul> |                  | <p>AWS, Azure<br>(Marketplace subscription)</p> |
| **Registered DB Operation Management** | ○                                                                                    | ○                | X (_BYOL new build_\*)                          |
| **Installation Automation**            | X                                                                                    | ○                | ○                                               |

{% hint style="info" %}
**Note**

OwlDB DBaaS can only manage DBs newly built through OwlDB, and does not support registering DBs already in operation. **DB licenses you already own can be transferred via the BYOL (Bring Your Own License) method** and applied to DBs newly built in OwlDB DBaaS.
{% endhint %}

## Key Features

Based on common management features universally used in both environments, OwlDB provides dedicated features optimized for the infrastructure characteristics of cloud and on-premises environments respectively.

### **Common Features**

| oeNNQWrBminc                  | b5kPa8lesNIR                                                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feature**                   | **Description**                                                                                                                                         |
| **Database Status Inquiry**   | Real-time verification of database and instance operating status                                                                                        |
| **Monitoring & Alerts**       | <ul><li>Monitoring of key performance indicators and operating status</li><li>Immediate alert dispatch upon occurrence of anomalies or events</li></ul> |
| **Migration**                 | Pre-compatibility verification and guide-based migration support when switching between heterogeneous databases                                         |
| **Account Management (RBAC)** | Per-user permission separation and security management through role-based access control                                                                |

{% tabs %}
{% tab title="Cloud-Specific" %}
| yw1fBYaxgqoC                      | MTQwvqYzs5cF                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Feature**                       | **Description**                                                                                               |
| **Automated Provisioning**        | Automation of the entire process, from cloud resource creation to database architecture configuration         |
| **Resource Scaling/Modification** | Scaling and modification of instance specifications and storage in line with workload increases and decreases |
| **Cloud Snapshot Backup**         | Backup and recovery based on CSP snapshot feature integration                                                 |
{% endtab %}

{% tab title="On-Premises-Specific" %}
| pKFXsoZlPV23                           | qk1seDReMy50                                                                                                                                   |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feature**                            | **Description**                                                                                                                                |
| **Database Installation/Registration** | <ul><li>Remote deployment of new DBs on customer hosts</li><li>Registration of existing external DBs in operation as managed targets</li></ul> |
| **Infrastructure Resource Discovery**  | Automatic collection of hardware specification and configuration information and status assessment via Agent                                   |
| **Physical Backup/Recovery**           | Backup and recovery based on the database's own utilities (Tibero RMGR, etc.)                                                                  |
{% endtab %}
{% endtabs %}

### Database Engines and Topologies

The relational database (RDBMS) engine specifications supported by OwlDB and the architecture configurations for each environment are as follows.

{% tabs %}
{% tab title="Cloud" %}
#### Azure

| Database Engine | Version                                      | Topology                                                                  |
| --------------- | -------------------------------------------- | ------------------------------------------------------------------------- |
| Tibero          | 7.2.6                                        | <ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul> |
| OpenSQL         | <ul><li>3.16.12.5</li><li>3.17.8.5</li></ul> | <ul><li>Single</li><li>HA</li></ul>                                       |
{% endtab %}

{% tab title="On-Premise" %}
#### OwlDB Operation

| Database Engine | Version              | Topology                                                                  |
| --------------- | -------------------- | ------------------------------------------------------------------------- |
| Tibero          | 7 patchset and later | <ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul> |
| OpenSQL         | 3.0(PostgreSQL 17.9) | <ul><li>Single</li><li>HA</li></ul>                                       |

#### OwlDB Automation

| Database Engine | Version                                                                          | Topology                                                                  |
| --------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Tibero          | <ul><li>Registration: 7 patchset and later</li><li>Installation: 7.2.6</li></ul> | <ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul> |
| OpenSQL         | 3.0(PostgreSQL 17.9)                                                             | <ul><li>Single</li><li>HA</li></ul>                                       |
{% endtab %}
{% endtabs %}
