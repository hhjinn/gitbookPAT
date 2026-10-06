{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

Before installing OwlDB, check the system and network requirements that the server must meet and prepare the environment in advance.

---

## System Requirements

Verify that the server on which OwlDB will be installed meets the requirements below.

### 1. Hardware Requirements

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 cores or more | - |
| Memory | 8GB or more | - |
| Disk | 50GB or more | Includes storage space for logs and backup files |

### 2. Software Requirements

Since OwlDB runs on Docker, the following software must be installed before installation.

**Docker**

| Item | Requirements |
| --- | --- |
| Docker | 29.3.1 or later |
| Docker Compose | V2 (2.x) |

### 3. Disk Configuration

A minimum of 50GB or more of disk space is required to install OwlDB, and it is recommended to configure it separately by purpose as shown below.

| Area | Purpose | Recommended Size |
| --- | --- | --- |
| Installation Path | OwlDB binaries and configuration files | 10GB |
| Data Path | Metadata store | 20GB |
| Log Path | OwlDB operational logs | 10GB |
| Backup Path | Backup file store | 10GB |

---

## Network Requirements

The OwlDB server communicates with both users (web browsers) and each database server. Complete the following port configuration and communication allowance settings in advance.

### 1. OwlDB Server Port Configuration

The following ports must be open on the OwlDB server.

| Port | Purpose | Direction |
| --- | --- | --- |
| 80/tcp<br>(port can be changed) | User web browser access | Allow inbound from user → OwlDB server |

### 2. Firewall Settings

Based on the port configuration above, firewall allowance settings between the OwlDB server and the database server are required. Configure inbound and outbound rules according to the customer environment's firewall policy.
