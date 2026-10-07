{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

Before installing OwlDB, check the system and network requirements that the server must meet and prepare the environment in advance.

---

## System requirements <a href="#system-requirements" id="system-requirements"></a>

Check whether the following requirements are met on the server where OwlDB will be installed.

### 1. Hardware requirements <a href="#id-1" id="id-1"></a>

| Item | Minimum specification | Remarks |
| --- | --- | --- |
| CPU | 2 cores or more | - |
| Memory | 8GB or more | - |
| Disk | 50GB or more | Includes storage space for logs and backup files |

### 2. Software requirements <a href="#id-2" id="id-2"></a>

Since OwlDB runs on Docker, the following software must be installed before installation.

**Docker**

| Item | Requirements |
| --- | --- |
| Docker | 29.3.1 or later |
| Docker Compose | V2 (2.x) |

### 3. Disk Configuration <a href="#id-3" id="id-3"></a>

At least 50GB of disk space is required to install OwlDB, and it is recommended to organize it by purpose as shown below.

| Area | Purpose | Recommended Size |
| --- | --- | --- |
| Installation Path | OwlDB binaries and configuration files | 10GB |
| Data Path | Metadata store | 20GB |
| Log Path | OwlDB operational logs | 10GB |
| Backup Path | Backup file store | 10GB |

---

## Network Requirements <a href="#network-requirements" id="network-requirements"></a>

The OwlDB server communicates with both users (web browsers) and each database server. Complete the following port configuration and communication allowance settings in advance.

### 1. OwlDB Server Port Configuration

The following ports must be open on the OwlDB server.

| Port | Purpose | Direction |
| --- | --- | --- |
| 80/tcp<br>(port can be changed) | User web browser access | Allow inbound from user → OwlDB server |

### 2. Firewall Settings <a href="#id-2-1" id="id-2-1"></a>

Based on the port configuration above, firewall allowance settings are required between the OwlDB server and the database server. Set inbound and outbound rules in accordance with the customer environment's firewall policy.
