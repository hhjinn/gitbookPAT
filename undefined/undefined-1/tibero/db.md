Verify the system and network requirements for OwlDB database installation, and prepare the deployment files and infrastructure.

{% hint style="info" %}
**Note**

This guide is intended for customer infrastructure administrators.
{% endhint %}

---

## System Requirements

### 1. Hardware Requirements

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 Cores or more | - |
| Memory | 4GB or more | - |
| Disk | - | 3. Disk (Volume) Requirements Verification |

### 2. Operating System Requirements

| Item | Requirement |
| --- | --- |
| OS | Rocky Linux 9.5 or higher |

### 3. Disk (Volume) Requirements

For TAC configurations, the following requirements must be met.

- All disks used in a TAC configuration must be **shared volumes accessible from all DB nodes**.

<table data-full-width="true"><thead><tr><th>Purpose</th><th>Requirement</th></tr></thead><tbody><tr><td>Data / Archive / Redo</td><td><ul><li>Prepare as shared volumes, raw devices, or partitioning paths</li><li>Do not create a file system</li></ul></td></tr><tr><td>Backup</td><td>Prepare as a shared volume or file system path</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Since OwlDB automatically configures a Tibero-dedicated file system (TAS) on the Data/Archive/Redo disks, you must not create partitions or file systems in advance.
{% endhint %}

### 4. Kernel Parameter Settings

To run the Tibero database, the following kernel parameters must be set. Tibero **Installation Guide** within the [Kernel Parameter Settings](#id-4)Configure through the section.

## Network Requirements

### Database Server Required Port Configuration

The following ports are required for a new database installation.

<table data-full-width="true"><thead><tr><th>Port Type</th><th>Port Number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener Port</strong></td><td>Example) 8629/tcp</td><td>Database connection port</td><td>Allow 8629 inbound from the OwlDB server</td></tr><tr><td><strong>Inter-node internal connection port</strong></td><td>Example) 8630~8679/tcp</td><td><ul><li>Inter-node internal communication port for TAC configuration</li><li>+50 range based on the DB Listener port</li></ul></td><td>Allow both inbound/outbound between nodes (only required for TAC and DR configurations)</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

The database uses, based on the specified DB Listener port, **a +50 range of ports**for inter-node internal connections.

**Example**: When the DB Listener port is 8629

- DB Listener port: 8629/tcp
- Inter-node internal connection ports: 8630/tcp ~ 8679/tcp (50 ports total)

Therefore, when setting the DB Listener port, the entire +50 range from that port must be free. No other application or service may be using that port range, and if a port conflict occurs, the database installation or TAC configuration may fail.
{% endhint %}

### Firewall Settings

Firewall settings are required for communication between the OwlDB server and the database server.

---

## Deployment File Preparation and Placement

### 1. List of Required Files

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- tibero binary (`tibero.tar.gz`)
- License file (`license.xml`)

### 2. Create Installation Directory

Create the path where the database will be installed (hereinafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/tibero
chmod 755 {installation directory}/tibero
```

`{설치 디렉터리}/tibero` This path, in the subsequent procedures, `$TB_HOME`is used as.

### 3. File Placement

Decompress the DP binary `$TB_HOME`into, then place the tibero binary and license file.

```bash
# Decompress DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place tibero binary
mv {tibero binary file} $TB_HOME/tibero.tar.gz

# Place license file
mv {license file} $TB_HOME/license.xml
```

After preparation is complete, `$TB_HOME` the structure is as follows.

```bash
$TB_HOME/
 ├── tibero.tar.gz               # tibero binary
 ├── license.xml                 # license file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

## Run the infrastructure validation script

Verify that the infrastructure settings of the installation DB environment are configured correctly.

```bash
cd $TB_HOME
sh validate_infra.sh --mode DP
```

The validation items are as follows.

- CPU, Memory size
- Disk size
- Whether the Tibero package is installed
- Network connection status
- Firewall port settings
- Required file list

{% hint style="warning" %}
**Caution**

If there are any items that did not pass, be sure to re-run the validation after completing the corrective actions. Proceed with package installation and Agent installation only after all items have passed.
{% endhint %}

## Package Installation

Install the package after passing all infrastructure validations.

{% tabs %}
{% tab title="When an external internet connection is available" %}
```bash
cd $TB_HOME
sudo bash install_pkg.sh
```
{% endtab %}
{% tab title="When an internet connection is not available" %}
Manually install the packages below in advance.

- Tibero package: Refer to the Tibero Package Installation Guide
- OwlDB package: `openssh-server`, `openssh-clients`, `sshpass`, `udev`, `xfsprogs`, `iproute`
{% endtab %}
{% endtabs %}

Afterward, [Database Server Agent Installation document](agent.md)Move to and proceed.
