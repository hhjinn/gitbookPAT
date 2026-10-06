To use OwlDB on-premise, several prerequisites must be prepared in the customer environment. This page first explains the configuration of OwlDB and guides you through what preparations are needed for each component.

## System Configuration <a href="#system-architecture" id="system-architecture"></a>

OwlDB consists of two types of servers.

<table><thead><tr><th>Server</th><th>Role</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Server</td><td>Monitoring Server</td><td><ul><li>The server on which the OwlDB application runs</li><li>Provides web UI and backend services, communicates with the Agent to perform installation/monitoring</li></ul></td></tr><tr><td>Database Server</td><td>Monitoring Target Server</td><td><ul><li>The server on which the Tibero database and OwlDB Agent are installed</li><li>Installs/operates the DB via OwlDB server commands</li></ul></td></tr></tbody></table>

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

OwlDB on-premise supports two configurations depending on how the database is used.

- **Installation DB** : A method for installing and managing a new database through OwlDB.
- **Registration DB**: A method for registering and managing an already operating database in OwlDB.

| Category | Installation DB | Registration DB |
| --- | --- | --- |
| Target DB | Tibero 7.2.5, OpenSQL 3.16.14.7, 3.17.10.7 | Tibero 7 or higher |
| Supported OS | Rocky Linux 9.5 or higher | CentOS 7, Rocky Linux 8/9, RHEL-family 7/8/9 |

{% hint style="info" %}
**Note**

The Installation DB is provided by OwlDB **Installed based on the latest Tibero binary version**.

For detailed support conditions and prerequisites for each configuration method, [Installation DB Environment Preparation Guide](#W2TxdEHwoC3mStsQfJdo) or [Registration DB Environment Preparation Guide](#pvI81bWJlNtie4nv4stI)please refer to.
{% endhint %}

### Required Patch List <a href="#undefined-1" id="undefined-1"></a>

The following patches are required to use the database in OwlDB.

| Patch Name | Patch Content |
| --- | --- |
| FS02PS_318447b | Fix for the issue where stdout is not closed during tbboot |
| FS02PS_339919c | Added switchover immediate option during tbdown |

{% hint style="warning" %}
**Caution**

- If FS02PS_318447b is not applied **DB Startup** functionality does not work.
- If FS02PS_339919c is not applied **failover**/**switchover** functionality does not work.
{% endhint %}

## Installation Flow <a href="#installation-flow" id="installation-flow"></a>

This guide proceeds in the following order. Preparation of the OwlDB server and the database server can be done in parallel, and connection verification is performed after both are ready.

> 📷 **[Image]** Image

## Distribution File Composition <a href="#deployment-files" id="deployment-files"></a>

Prior to installation, prepare the following two types of distribution files.

<table><thead><tr><th>Distribution File</th><th>Installation Target Server</th><th>Component</th></tr></thead><tbody><tr><td><code>owldb-cp-installer-*.tar.gz</code></td><td>OwlDB Server</td><td><ul><li>OwlDB backend/frontend Docker images</li><li>Installation Script</li></ul></td></tr><tr><td><code>owldb-dp-installer-*.tar.gz</code></td><td>Database Server</td><td><ul><li>Tibero installation script</li><li>tbagent binary</li><li>Infrastructure validation script</li></ul></td></tr></tbody></table>
