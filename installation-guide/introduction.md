To use OwlDB on-premises, some preparation is required in the customer environment. This page first explains the configuration of OwlDB and then guides you through what preparation is needed for each component.

## System Configuration <a href="#system-architecture" id="system-architecture"></a>

OwlDB consists of two types of servers.

<table><thead><tr><th>Server</th><th>Role</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Server</td><td>Control Server</td><td><ul><li>The server on which the OwlDB application runs</li><li>Provides the web UI and backend services, and performs installation/monitoring by communicating with the Agent</li></ul></td></tr><tr><td>Database Server</td><td>Monitored Target Server</td><td><ul><li>The server on which the Tibero database and OwlDB Agent are installed</li><li>Installs/operates the DB via OwlDB server commands</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

The OwlDB server and the database server must be configured in separate, independent environments.
{% endhint %}

### Communication Structure <a href="#undefined" id="undefined"></a>

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

## Database Configuration Method <a href="#database-configuration-types" id="database-configuration-types"></a>

OwlDB on-premises supports two configurations depending on how the database is used.

- **Installed DB** : A method of installing and managing a new database through OwlDB.
- **Registered DB**: A method of registering and managing an already operating database in OwlDB.

| Category | Installed DB | Registered DB |
| --- | --- | --- |
| Target DB | Tibero 7.2.5, OpenSQL 3.16.14.7, 3.17.10.7 | Tibero 7 or later |
| Supported OS | Rocky Linux 9.5 or later | CentOS 7, Rocky Linux 8/9, RHEL-family 7/8/9 |

{% hint style="info" %}
**Note**

The installed DB is installed based on the **latest Tibero binary version provided by OwlDB**.

For detailed support conditions and preparation requirements for each configuration method, [Installed DB Environment Preparation Guide](#W2TxdEHwoC3mStsQfJdo) or [Registered DB Environment Preparation Guide](#pvI81bWJlNtie4nv4stI)please refer to.
{% endhint %}

### Required Patch List <a href="#undefined-1" id="undefined-1"></a>

To use a database in OwlDB, the following patches are required.

| Patch Name | Patch Content |
| --- | --- |
| FS02PS_318447b | Fix for the issue where stdout is not closed during tbboot |
| FS02PS_339919c | Added switchover immediate option during tbdown |

{% hint style="warning" %}
**Caution**

- If FS02PS_318447b is not applied **DB Startup** function does not work.
- If FS02PS_339919c is not applied **failover**/**switchover** function does not work.
{% endhint %}

## Installation Flow <a href="#installation-flow" id="installation-flow"></a>

This guide proceeds in the following order. Preparation of the OwlDB server and the database server can be done in parallel, and connection verification is performed after both are ready.

> 📷 **[Image]** Image

## Distribution File Configuration <a href="#deployment-files" id="deployment-files"></a>

Before installation, prepare the following two types of distribution files.

<table><thead><tr><th>Distribution File</th><th>Installation Target Server</th><th>Component</th></tr></thead><tbody><tr><td><code>owldb-cp-installer-*.tar.gz</code></td><td>OwlDB Server</td><td><ul><li>OwlDB backend/frontend Docker images</li><li>Installation Script</li></ul></td></tr><tr><td><code>owldb-dp-installer-*.tar.gz</code></td><td>Database Server</td><td><ul><li>Tibero installation script</li><li>tbagent binary</li><li>Infrastructure validation script</li></ul></td></tr></tbody></table>
