Check the system and network requirements for installing the OwlDB database, and prepare the deployment files and infrastructure.

{% hint style="info" %}
**Note**

This guide is intended for the customer's infrastructure administrators.
{% endhint %}

---

# System requirements <a href="#system-requirements" id="system-requirements"></a>

### 1. Hardware requirements <a href="#id-1" id="id-1"></a>

| Item | Minimum specification | Remarks |
| --- | --- | --- |
| CPU | 2 cores or more | - |
| Memory | 4GB or more | - |
| Disk | - | 3. Check Disk (Volume) Requirements |

### 2. Operating System Requirements <a href="#id-2" id="id-2"></a>

| Item | Requirements |
| --- | --- |
| OS | Rocky Linux 9.5 or later |

### 3. Disk (Volume) Requirements <a href="#id-3" id="id-3"></a>

For a TAC configuration, the requirements below must be followed.

- All disks used in a TAC configuration must be **shared volumes accessible from all DB nodes**.

<table><thead><tr><th>Purpose</th><th>Requirements</th></tr></thead><tbody><tr><td>Data / Archive / Redo</td><td><ul><li>Prepare in the form of a shared volume, raw device, or partitioning path</li><li>No file system creation</li></ul></td></tr><tr><td>Backup</td><td>Prepare as a shared volume, file system path</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Since OwlDB automatically configures a Tibero-dedicated file system (TAS) on the Data/Archive/Redo disks, you must not create partitioning or a file system in advance.
{% endhint %}

### 4. Kernel Parameter Configuration <a href="#id-4" id="id-4"></a>

To run the Tibero database, the kernel parameters below must be configured. Configure them through [Kernel Parameter Configuration](#id-4) in the Tibero **Installation Guide**.

# Network Requirements <a href="#network-requirements" id="network-requirements"></a>

### Database Server Essential Port Configuration <a href="#undefined" id="undefined"></a>

The following ports are required for a new database installation.

<table><thead><tr><th>Port type</th><th>Port number</th><th>Purpose</th><th>Remarks</th></tr></thead><tbody><tr><td><strong>DB Listener port</strong></td><td>Example) 8629/tcp</td><td>Database connection port</td><td>Allow inbound 8629 from OwlDB server</td></tr><tr><td><strong>Internal connection port between nodes</strong></td><td>Example) 8630~8679/tcp</td><td><ul><li>Internal communication port between nodes during TAC configuration</li><li>+50 range from the DB Listener port</li></ul></td><td>Allow both inbound/outbound between nodes (required only for TAC, DR configuration)</td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

The database uses **a +50 range of ports** based on the designated DB Listener port for internal connections between nodes.

**Example**: When the DB Listener port is 8629

- DB Listener port: 8629/tcp
- Internal connection ports between nodes: 8630/tcp ~ 8679/tcp (50 ports in total)

Therefore, when setting the DB Listener port, the entire +50 range of that port must be free. No other application or service may be using that port range, and if a port conflict occurs, the database installation or TAC configuration may fail.
{% endhint %}

### Firewall configuration <a href="#undefined-1" id="undefined-1"></a>

Firewall configuration is required for communication between the OwlDB server and the database server.

---

# Preparing and placing deployment files <a href="#prepare-deployment-files" id="prepare-deployment-files"></a>

### 1. List of required files <a href="#id-1-1" id="id-1-1"></a>

- owldb dp binary (`owldb-dp-installer-*.tar.gz`)
- tibero binary (`tibero.tar.gz`)
- license file (`license.xml`)

### 2. Create the installation directory <a href="#id-2-1" id="id-2-1"></a>

Create the path where the database will be installed (hereafter the `installation directory`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/tibero
chmod 755 {installation directory}/tibero
```

The `{installation directory}/tibero` path is used as `$TB_HOME` in the subsequent procedures.

### 3. Place the files <a href="#id-3-1" id="id-3-1"></a>

Extract the DP binary into `$TB_HOME`, and place the tibero binary and the license file.

```bash
# Extract the DP binary
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# Place the tibero binary
mv {tibero binary file} $TB_HOME/tibero.tar.gz

# Place the license file
mv {license file} $TB_HOME/license.xml
```

After preparation is complete, the `$TB_HOME` structure is as follows.

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
- Firewall port configuration
- Required file list

{% hint style="warning" %}
**Caution**

If there are any items that did not pass, be sure to re-validate after completing the remediation. Proceed with the package installation and Agent installation only after all items have passed.
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
Manually install the packages below in advance.

- Tibero package: Refer to the Tibero package installation guide
- OwlDB packages: `openssh-server`, `openssh-clients`, `sshpass`, `udev`, `xfsprogs`, `iproute`
{% endtab %}
{% endtabs %}

Then, proceed by moving to the [Database server Agent installation document](agent-installation.md).
