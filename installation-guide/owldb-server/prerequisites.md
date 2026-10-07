{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

Before installing OwlDB, verify the system and network requirements that the server must meet, and prepare the environment in advance.

---

## System Requirements <a href="#system-requirements" id="system-requirements"></a>

Check whether the following requirements are met on the server where OwlDB will be installed.

### 1. Hardware Requirements <a href="#id-1" id="id-1"></a>

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 Cores or more | - |
| Memory | 8GB or more | - |
| Disk | 50GB or more | Includes storage space for logs and backup files |

### 2. Software Requirements <a href="#id-2" id="id-2"></a>

OwlDB runs on Docker, so the following software must be installed before installation.

**Docker**

| Item | Requirement |
| --- | --- |
| Docker | 29.3.1 or higher |
| Docker Compose | V2 (2.x) |

### 3. Disk Configuration <a href="#id-3" id="id-3"></a>

At least 50GB of disk space is required to install OwlDB, and it is recommended to configure it by purpose as shown below.

| Area | Purpose | Recommended Size |
| --- | --- | --- |
| Installation Path | OwlDB binary and configuration files | 10GB |
| Data Path | Metadata repository | 20GB |
| Log Path | OwlDB operation logs | 10GB |
| Backup Path | Backup file repository | 10GB |

---

## Network Requirements <a href="#network-requirements" id="network-requirements"></a>

The OwlDB server communicates with both users (web browsers) and each database server. Complete the port configuration and communication allow settings below in advance.

### 1. OwlDB Server Port Configuration

The following ports must be open on the OwlDB server.

| Port | Purpose | Direction |
| --- | --- | --- |
| 80/tcp<br>(port can be changed) | User web browser access | Allow inbound from user → OwlDB server |

### 2. Firewall Settings <a href="#id-2-1" id="id-2-1"></a>

According to the port configuration above, firewall allow settings are required between the OwlDB server and the database server. Set the inbound and outbound rules according to the customer environment's firewall policy.
