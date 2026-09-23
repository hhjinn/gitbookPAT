# Installation Introduction

To use OwlDB on-premises, some preliminary preparations are required in the customer environment. This page first explains the configuration of OwlDB and then guides you through the preparations required for each component.

### System Configuration

OwlDB consists of two types of servers.

<table data-full-width="true"><thead><tr><th>Server</th><th>Role</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Server</td><td>Monitoring Server</td><td><ul><li>The server on which the OwlDB application runs</li><li>Provides a web UI and backend services, and performs installation and monitoring by communicating with the Agent</li></ul></td></tr><tr><td>Database Server</td><td>Monitored Server</td><td><ul><li>The server on which the Tibero database and OwlDB Agent are installed</li><li>Installs and operates the DB through OwlDB server commands</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Notes**

The OwlDB server and the database server must be configured in separate, independent environments.
{% endhint %}

#### Communication Structure

The OwlDB server and the database server communicate through the Agent installed on each server.

```
[ User Browser ]
        |  HTTP (UI_PORT)
        v
[ OwlDB Server ]
   - OwlDB Backend
   - OwlDB Frontend
        |  SERVER_PORT (outbound)
        v
[ Database Server ]
   - OwlDB Agent (tbagent)
   - Tibero DB
```

The Agent is installed on the database server, receives connections from the OwlDB server, and locally executes the tasks required for Tibero installation/startup/monitoring.

### Database Configuration Methods

OwlDB on-premises supports two configurations depending on how the database is used.

* **Installed DB** : A method of installing and managing a new database through OwlDB.
* **Registered DB**: A method of registering an existing database in OwlDB for management.

| Category     | Installed DB                               | Registered DB                                |
| ------------ | ------------------------------------------ | -------------------------------------------- |
| Target DB    | Tibero 7.2.5, OpenSQL 3.16.14.7, 3.17.10.7 | Tibero 7 or later                            |
| Supported OS | Rocky Linux 9.5 or later                   | CentOS 7, Rocky Linux 8/9, RHEL-family 7/8/9 |

{% hint style="info" %}
**Notes**

OwlDB provides **Installed DB** based on the latest Tibero binary version.

For detailed support conditions and preparations, refer to the [Installed DB Environment Preparation Guide](undefined.md#W2TxdEHwoC3mStsQfJdo) or [Registered DB Environment Preparation Guide](undefined.md#pvI81bWJlNtie4nv4stI).
{% endhint %}

#### Required Patch List

To use a database in OwlDB, the following patches are required.

| Patch Name      | Patch Contents                                          |
| --------------- | ------------------------------------------------------- |
| FS02PS\_318447b | Fixes an issue where stdout is not closed during tbboot |
| FS02PS\_339919c | Adds the switchover immediate option during tbdown      |

{% hint style="warning" %}
**Caution**

* If FS02PS\_318447b is not applied, **DB startup** functionality will not work.
* If FS02PS\_339919c is not applied, **failover**/**switchover** functionality will not work.
{% endhint %}

### Installation Flow

This guide proceeds in the following order. Preparation of the OwlDB server and the database server can be done in parallel, and once both are ready, connection verification is performed.

> 📷 **\[Image]** Image

### Distribution File Composition

Prior to installation, prepare the following two types of distribution files.

<table data-full-width="true"><thead><tr><th>Deployment File</th><th>Installation Target Server</th><th>Component</th></tr></thead><tbody><tr><td><code>owldb-cp-installer-*.tar.gz</code></td><td>OwlDB Server</td><td><ul><li>OwlDB backend/frontend Docker images</li><li>Installation script</li></ul></td></tr><tr><td><code>owldb-dp-installer-*.tar.gz</code></td><td>Database Server</td><td><ul><li>Tibero installation script</li><li>tbagent binary</li><li>Infrastructure verification script</li></ul></td></tr></tbody></table>
