{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

Before installing OwlDB, verify the system and network requirements that the server must meet, and prepare the environment in advance.

---

## System Requirements

Verify that the following requirements are met on the server where OwlDB will be installed.

### 1. Hardware Requirements

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 Cores or more | - |
| Memory | 8GB or more | - |
| Disk | 50GB or more | Includes storage space for logs and backup files |

### 2. Software Requirements

Since OwlDB runs on Docker, the following software must be installed before installation.

**Docker**

| Item | Requirement |
| --- | --- |
| Docker | 29.3.1 or higher |
| Docker Compose | V2 (2.x) |

### 3. Disk Configuration

A minimum of 50GB or more of disk space is required to install OwlDB, and it is recommended to configure it by purpose as shown below.

| Area | Purpose | Recommended Size |
| --- | --- | --- |
| Installation Path | OwlDB binaries and configuration files | 10GB |
| Data Path | Metadata store | 20GB |
| Log Path | OwlDB operation logs | 10GB |
| Backup Path | Backup file store | 10GB |

---

## Network Requirements

The OwlDB server communicates with both users (web browsers) and each database server. Complete the following port configuration and communication permission settings in advance.

### 1. OwlDB Server Port Configuration

The following ports must be open on the OwlDB server.

| Port | Purpose | Direction |
| --- | --- | --- |
| 80/tcp<br>(port can be changed) | User web browser access | Allow inbound from user → OwlDB server |

### 2. Firewall Settings

According to the port configuration above, firewall permission settings between the OwlDB server and the database server are required. Configure inbound and outbound rules in accordance with the customer environment's firewall policy.
