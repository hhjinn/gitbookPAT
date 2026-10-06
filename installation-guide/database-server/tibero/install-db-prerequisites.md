Check the system and network requirements for installing the OwlDB database, and prepare the deployment files and infrastructure.

{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

---

# System Requirements

### 1. Hardware Requirements

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 cores or more | - |
| Memory | 4GB or more | - |
| Disk | - | 3. Checking Disk (Volume) Requirements |

### 2. Operating System Requirements

| Item | Requirements |
| --- | --- |
| OS | Rocky Linux 9.5 or later |

### 3. Disk (Volume) Requirements

For a TAC configuration, the following requirements must be met.

- All disks used in a TAC configuration must be **shared volumes accessible from all DB nodes**.

<table><thead><tr><th>Purpose</th><th>Requirements</th></tr></thead><tbody><tr><td>Data / Archive / Redo</td><td><ul><li>Prepared as a shared volume, raw device, or partitioning path</li><li>Do not create a file system</li></ul></td></tr><tr><td>Backup</td><td>Prepared as a shared volume or file system path</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Since OwlDB automatically configures the Tibero-dedicated file system (TAS) on the Data/Archive/Redo disks, you must not create partitions or file systems in advance.
{% endhint %}

### 4. Kernel Parameter Settings

To run the Tibero database, the following kernel parameters must be set. Tibero **Installation Guide** within the [Kernel Parameter Settings](#id-4)Configure them through this.

# Network Requirements

### Database Server Required Port Configuration

The following ports are required to install a new database.

<table><thead><tr><th>Port Type</th><th>Port Number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener Port</strong></td><td>Example) 8629/tcp</td><td>Database connection port</td><td>Allow inbound 8629 from the OwlDB server</td></tr><tr><td><strong>Inter-node internal connection port</strong></td><td>Example) 8630–8679/tcp</td><td><ul><li>Inter-node internal communication port for TAC configuration</li><li>+50 range based on the DB Listener port</li></ul></td><td>Allow both inter-node inbound/outbound (required only for TAC or DR configurations)</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Based on the designated DB Listener port, the database uses **ports in the +50 range**for internal connections between nodes.

**Example**: When the DB Listener port is 8629

- DB Listener port: 8629/tcp
- Inter-node internal connection ports: 8630/tcp ~ 8679/tcp (50 ports in total)

Therefore, when setting the DB Listener port, the entire +50 range from that port must be free. No other application or service may be using that port range, and if a port conflict occurs, database installation or TAC configuration may fail.
{% endhint %}

### Firewall Configuration

Firewall configuration is required for communication between the OwlDB server and the database server.

---

# Preparing and Placing Deployment Files

### 1. List of Required Files

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- tibero binary (`tibero.tar.gz`)
- license file (`license.xml`)

### 2. Creating the Installation Directory

Create the path where the database will be installed (hereafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/tibero
chmod 755 {installation directory}/tibero
```

`{설치 디렉터리}/tibero` This path is used as `$TB_HOME`in the subsequent procedures.

### 3. File Placement

Extract the DP binary to `$TB_HOME`, then place the tibero binary and the license file.

```bash
# Extract the DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place the tibero binary
mv {tibero binary file} $TB_HOME/tibero.tar.gz

# Place the license file
mv {license file} $TB_HOME/license.xml
```

Once preparation is complete, `$TB_HOME` the structure is as follows.

```bash
$TB_HOME/
 ├── tibero.tar.gz               # tibero binary
 ├── license.xml                 # License file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # Infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

# Run the infrastructure validation script

Validates whether the infrastructure settings of the installation DB environment are correctly configured.

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

If there are any items that did not pass, be sure to re-run the validation after completing the corrective actions. Proceed with the package installation and Agent installation only after all items have passed.
{% endhint %}

# Package installation

Install the package after passing all infrastructure validations.

{% tabs %}
{% tab title="When an external internet connection is available" %}
```bash
cd $TB_HOME
sudo bash install_pkg.sh
```
{% endtab %}
{% tab title="When an internet connection is not available" %}
Manually install the following packages in advance.

- Tibero package: Refer to the Tibero package installation guide
- OwlDB package: `openssh-server`, `openssh-clients`, `sshpass`, `udev`, `xfsprogs`, `iproute`
{% endtab %}
{% endtabs %}

Afterwards [Database server Agent installation document](agent-installation.md)and proceed.
