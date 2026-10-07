Check the system and network requirements for installing the OwlDB database, and prepare the deployment files and infrastructure.

{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

---

# System Requirements <a href="#system-requirements" id="system-requirements"></a>

### 1. Hardware Requirements <a href="#id-1" id="id-1"></a>

| Item | Minimum Specification | Remarks |
| --- | --- | --- |
| CPU | 2 Cores or more | - |
| Memory | 4GB or more | - |
| Disk | - | 3. Checking disk (volume) requirements |

### 2. Operating system requirements <a href="#id-2" id="id-2"></a>

| Item | Requirement |
| --- | --- |
| OS | Rocky Linux 9.5 or later |

### 3. Disk (volume) requirements <a href="#id-3" id="id-3"></a>

For a TAC configuration, the following requirements must be met.

- All disks used in a TAC configuration must be **a shared volume accessible from all DB nodes.**.

<table><thead><tr><th>Purpose</th><th>Requirements</th></tr></thead><tbody><tr><td>Data / Archive / Redo</td><td><ul><li>Prepare as a shared volume, raw device, or partitioning path</li><li>Do not create a file system</li></ul></td></tr><tr><td>Backup</td><td>Prepare as a shared volume or file system path</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Because OwlDB automatically configures a Tibero-dedicated file system (TAS) on the Data/Archive/Redo disks, you must not create partitions or file systems in advance.
{% endhint %}

### 4. Kernel parameter settings <a href="#id-4" id="id-4"></a>

To run the Tibero database, the following kernel parameters must be set. Tibero **Installation Guide** within [Kernel parameter settings](#id-4)through which they are set.

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

### Required port configuration for the database server <a href="#undefined" id="undefined"></a>

The following ports are required to install a new database.

<table><thead><tr><th>Port type</th><th>Port number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener port</strong></td><td>Example) 8629/tcp</td><td>Database connection port</td><td>Allow inbound 8629 from the OwlDB server</td></tr><tr><td><strong>Inter-node internal connection port</strong></td><td>Example) 8630~8679/tcp</td><td><ul><li>Inter-node internal communication port for TAC configuration</li><li>Range of +50 from the DB Listener port</li></ul></td><td>Allow both inbound and outbound between nodes (required only for TAC and DR configurations)</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Based on the designated DB Listener port, the database uses **ports in the +50 range**for inter-node internal connections.

**Example**: When the DB Listener port is 8629

- DB Listener port: 8629/tcp
- Inter-node internal connection ports: 8630/tcp ~ 8679/tcp (50 ports in total)

Therefore, when setting the DB Listener port, the entire +50 range from that port must be free. No other application or service may be using that port range, and a port conflict may cause the database installation or TAC configuration to fail.
{% endhint %}

### Firewall settings <a href="#undefined-1" id="undefined-1"></a>

Firewall settings are required for communication between the OwlDB server and the database server.

---

# Preparing and placing deployment files <a href="#prepare-deployment-files" id="prepare-deployment-files"></a>

### 1. List of required files <a href="#id-1-1" id="id-1-1"></a>

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- tibero binary (`tibero.tar.gz`)
- license file (`license.xml`)

### 2. Creating the installation directory <a href="#id-2-1" id="id-2-1"></a>

Create the path where the database will be installed (hereafter `installation directory`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/tibero
chmod 755 {installation directory}/tibero
```

`{installation directory}/tibero` This path is used in subsequent procedures `$TB_HOME`is used as.

### 3. File Placement <a href="#id-3-1" id="id-3-1"></a>

The DP binary `$TB_HOME`decompress it to, and place the tibero binary and license file.

```bash
# Decompress DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place tibero binary
mv {tibero binary file} $TB_HOME/tibero.tar.gz

# Place license file
mv {license file} $TB_HOME/license.xml
```

After preparation is complete `$TB_HOME` the structure is as follows.

```bash
$TB_HOME/
 ├── tibero.tar.gz               # tibero binary
 ├── license.xml                 # license file
 ├── install/                    # Tibero installation script
 ├── validate_infra.sh           # infrastructure validation script
 ├── install_pkg.sh              # Tibero package installation script
 └── tbagent_dist_latest.tar.gz  # tbagent binary
```

# Run the infrastructure validation script <a href="#run-infra-check-script" id="run-infra-check-script"></a>

Validate whether the infrastructure settings of the installation DB environment are correctly configured.

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

If there are items that did not pass, be sure to re-validate after completing the necessary actions. Proceed with package installation and Agent installation only after passing all items.
{% endhint %}

# Package installation <a href="#install-packages" id="install-packages"></a>

Install the package after passing all infrastructure validation.

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

Afterwards [Database server Agent installation document](agent-installation.md)go to and proceed.
