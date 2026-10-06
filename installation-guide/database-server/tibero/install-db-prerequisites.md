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
| OS | Rocky Linux 9.5 or higher |

### 3. Disk (volume) requirements <a href="#id-3" id="id-3"></a>

For TAC configurations, the following requirements must be met.

- All disks used in a TAC configuration **A shared volume accessible from all DB nodes**must be.

<table><thead><tr><th>Purpose</th><th>Requirements</th></tr></thead><tbody><tr><td>Data / Archive / Redo</td><td><ul><li>Prepare as a shared volume, raw device, or partitioning path</li><li>Do not create a file system</li></ul></td></tr><tr><td>Backup</td><td>Prepare as a shared volume, file system path</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Since OwlDB automatically configures a Tibero-dedicated file system (TAS) on the Data/Archive/Redo disks, you must not create partitions or file systems in advance.
{% endhint %}

### 4. Kernel parameter settings <a href="#id-4" id="id-4"></a>

The following kernel parameters must be set to run the Tibero database. Tibero **Installation Guide** Within [Kernel parameter settings](#id-4)Configure through.

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

### Database server required port configuration <a href="#undefined" id="undefined"></a>

The following ports are required for a new database installation.

<table><thead><tr><th>Port type</th><th>Port number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener port</strong></td><td>Example) 8629/tcp</td><td>Database connection port</td><td>Allow 8629 inbound from the OwlDB server</td></tr><tr><td><strong>Inter-node internal connection port</strong></td><td>Example) 8630~8679/tcp</td><td><ul><li>Inter-node internal communication port for TAC configuration</li><li>+50 range based on the DB Listener port</li></ul></td><td>Allow both inbound/outbound between nodes (required only for TAC, DR configurations)</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

The database, based on the designated DB Listener port, **Ports in the +50 range**is used for internal connections between nodes.

**Example**: When the DB Listener port is 8629

- DB Listener port: 8629/tcp
- Inter-node internal connection ports: 8630/tcp ~ 8679/tcp (50 ports total)

Therefore, when setting the DB Listener port, the entire +50 range of that port must be free. No other application or service may be using that port range, and if a port conflict occurs, the database installation or TAC configuration may fail.
{% endhint %}

### Firewall settings <a href="#undefined-1" id="undefined-1"></a>

Firewall settings are required for communication between the OwlDB server and the database server.

---

# Preparing and placing deployment files <a href="#prepare-deployment-files" id="prepare-deployment-files"></a>

### 1. List of required files <a href="#id-1-1" id="id-1-1"></a>

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- tibero binary (`tibero.tar.gz`)
- License file (`license.xml`)

### 2. Creating the installation directory <a href="#id-2-1" id="id-2-1"></a>

The path where the database will be installed (hereafter `installation directory`) is created. (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/tibero
chmod 755 {installation directory}/tibero
```

`{installation directory}/tibero` The path, in subsequent procedures, `$TB_HOME`Used as.

### 3. File Placement <a href="#id-3-1" id="id-3-1"></a>

Decompress the DP binary `$TB_HOME`to this location, and place the tibero binary and license file.

```bash
# Decompress the DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place the tibero binary
mv {tibero binary file} $TB_HOME/tibero.tar.gz

# Place the license file
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

# Running the infrastructure validation script <a href="#run-infra-check-script" id="run-infra-check-script"></a>

Validates whether the infrastructure settings of the installation DB environment are configured correctly.

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

If there are any items that did not pass, be sure to re-validate after completing the necessary actions. Proceed with package installation and Agent installation only after all items have passed.
{% endhint %}

# Package Installation <a href="#install-packages" id="install-packages"></a>

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

Afterwards, [Database Server Agent Installation document](agent-installation.md)move to and proceed.
