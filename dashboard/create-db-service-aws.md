This guide explains how to create a database (hereinafter referred to as provisioning) in OwlDB. Once provisioning is complete, you can use all the features provided by OwlDB.

{% hint style="info" %}
**Note**

- Database creation can only be performed by **users with Root privileges**.
- The OpenSQL engine is not supported in the AWS environment.
- You can move to the database creation page by clicking **OwlDB Console Screen > Dashboard > Card View > + icon** or **GNB > DB Alias dropdown > Create DB Service button**.
- You can check the provisioning progress status by clicking the notification (bell) icon in the upper right corner of the console screen or on the dashboard.
- For information about the database engines and instance types supported by OwlDB, please refer to the '[AWS](#XDj4D6jZeLIG3hl9e9W4)' and '[Azure](#azure)' pages.
{% endhint %}

{% hint style="warning" %}
**Caution**

In the AWS environment, when you enter the database creation page, the user's **Rocky subscription status** is checked, and if not subscribed, **a guidance modal about the subscription requirement** appears.
{% endhint %}

# Create a new DB Service <a href="#create-new" id="create-new"></a>

1. Navigate to the **OwlDB Console Screen > Dashboard** menu.
2. Click the **Create** button.
3. In the **Engine Options** step, select the database engine and license-related information.
4. In the **DR Configuration** step, set whether to use DR and the related options.
5. In the **AZ Configuration** step, set the availability zone.
6. In the **Instance Configuration** step, enter the instance and volume information.
7. In the **Database Configuration** step, enter the database information.
8. In the **Configuration Information Review** step, review all the creation information.
9. Start provisioning by clicking the button according to the License Option. **LI**: Click the **Create** button **BYOL**: Click the **Register License** button

{% hint style="info" %}
**Note**

If you select BYOL as the License Option, you need to register a license file. For more details, refer to [BYOL License Registration](#byol).
{% endhint %}

---

# **Creation Options** <a href="#creation-options" id="creation-options"></a>

You can check the estimated amount based on the options selected when creating the database. This amount is calculated based on the Seoul region, and the actual amount may vary depending on various factors such as the region and actual usage.

### Step 1: Engine Options <a href="#step-1-engine" id="step-1-engine"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service Name*</td><td>A name to identify the DB Service<ul><li>Cannot be used in duplicate within the OwlDB account</li><li>Within 6 to 30 characters, only uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) can be used, spaces cannot be used</li></ul></td></tr><tr><td>Database Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong></li><li><strong>OpenSQL</strong> (to be supported later)</li></ul></td></tr><tr><td>License Option*</td><td>The license option to use<ul><li><strong>LI</strong>(License Included)</li><li><strong>BYOL</strong> (Bring Your Own License)</li></ul></td></tr><tr><td>Topology*</td><td>The topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li></ul></td></tr><tr><td>Edition*</td><td>The edition of the license<ul><li><strong>Standard Edition (SE)</strong>: Dedicated to single server configuration, up to 8 vCPUs available</li><li><strong>Enterprise Edition (EE)</strong>: Supports high availability and large-scale configurations, no vCPU limit</li><li>If you select TAC for Topology, Enterprise Edition is automatically applied and cannot be changed</li></ul></td></tr><tr><td>Node Count*</td><td>Number of cluster configuration nodes<ul><li><strong>Tibero</strong>: 1 Single (fixed), select from 2 to 4 for TAC</li></ul></td></tr></tbody></table>

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

If you select Standard Edition (SE) in Edition, you can only select instance types with up to 8 vCPUs in the instance configuration step.
{% endhint %}

### Step 2: DR Configuration <a href="#step-2-dr" id="step-2-dr"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration<ul><li><strong>Tibero</strong>: Selected directly by the user</li></ul></td></tr><tr><td>Failover Automation Level*</td><td>Failover automation level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td></tr><tr><td>Standby/Replica Count*</td><td>Number of Standby (or Replica) DBs<ul><li><strong>Tibero</strong>: Up to 2 can be selected</li></ul></td></tr><tr><td>Standby Mode*</td><td>Standby Mode options (can be set individually for each Standby node)<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>The method of transmitting the Primary's logs to the Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that immediately transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that collects and transmits archive log files after a log switch occurs and the archive log files are generated</li></ul></td></tr></tbody></table>

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- Failover Automation Level, Standby(Replica) Count, Standby Mode, and Log Replication Type are only displayed when you select to use it in Enable DR.
- The levels that can be set for Failover Automation Level vary depending on the license type. LI can select levels 0 to 3, and BYOL can select levels 0, 2, and 3.
- Standby Mode and Log Replication Type can be set individually for each Standby node.
{% endhint %}

### Step 3: AZ Configuration <a href="#step-3-az" id="step-3-az"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Availability Zone(AZ)* (disabled)</td><td>OwlDB's availability zone</td></tr><tr><td>Primary(Leader) DB Availability Zone(AZ)*</td><td>Availability zone of the Primary (Leader) DB<br><strong>Default value</strong><ul><li>DR not used: Same zone as OwlDB</li><li>DR used: Different zone from OwlDB</li></ul></td></tr><tr><td>Standby(Replica) DB Availability Zone(AZ)*</td><td>Availability zone of the Standby (Replica) DB<ul><li>Default value: Placed in the same availability zone as OwlDB, then automatically placed in a different zone afterward</li></ul></td></tr></tbody></table>

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- The Standby(Replica) DB Availability Zone is only displayed when you select to use it in Enable DR.
- The availability zone of the Primary (Leader) DB can be selected by the user, but for stable failure response and Failover, it is recommended to place the Primary (Leader) DB in a different availability zone from OwlDB.
{% endhint %}

### Step 4: Instance Configuration <a href="#step-4-instance" id="step-4-instance"></a>

<table><thead><tr><th>Category</th><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Instance Setting</td><td>DB Virtual Machine Size*</td><td>The instance type that determines performance and specifications</td></tr><tr><td>Instance Access Setting</td><td>DB Instance SSH Key Name*</td><td>Settings for accessing the DB instance</td></tr><tr><td>Data Disk</td><td>Data Disk Type*</td><td>The type of disk for storing the main data</td></tr><tr><td></td><td>Data Disk Size*</td><td>The size of the disk for storing the main data</td></tr><tr><td></td><td>Data Disk IOPS*</td><td>The I/O throughput of the disk for storing the main data</td></tr><tr><td></td><td>Data Disk MBps</td><td>The maximum processing speed of the disk for storing the main data</td></tr><tr><td>Redo Log Disk</td><td>Redo Log Disk Type</td><td>The type of disk for storing the Redo log</td></tr><tr><td></td><td>Redo Log Disk Size (disabled)</td><td>The size of the disk for storing the Redo log<ul><li>Automatically calculated based on the entered Redo Log File Size (GB)</li></ul></td></tr><tr><td></td><td>Redo Log Disk IOPS</td><td>The I/O throughput of the disk for storing the Redo log</td></tr><tr><td></td><td>Redo Log Disk MBps</td><td>The maximum processing speed of the disk for storing the Redo log</td></tr><tr><td>Archive Log Volume</td><td>Archive Log Disk Type</td><td>The type of disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk Size</td><td>The size of the disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk IOPS</td><td>The I/O throughput of the disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk MBps</td><td>Maximum processing speed of the disk for storing archive logs</td></tr><tr><td>Auto Scale</td><td>Enabled*</td><td>Whether to automatically expand the data disk size based on data volume usage</td></tr><tr><td></td><td>Maximum expansion limit*</td><td>Maximum size the data disk can grow to when Auto Scale is enabled</td></tr></tbody></table>

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- For a stable operating environment, all instances within the cluster are automatically configured with the same specifications.
- If the Edition is set to **Standard Edition(SE)**, the DB Instance Type list displays only specifications up to 8 vCPU.
- Some availability zones may not support certain DB Instance Types; in this case, they appear in the list but cannot be selected.
- The IOPS for each disk can only be entered within the range allowed by the selected disk type.
- Redo Log Disk and Archive Log Volume are exposed only in the Tibero engine.
{% endhint %}

### Step 5: Database Configuration <a href="#step-5-database" id="step-5-database"></a>

| Item | Description |
| --- | --- |
| Database Name* | Name of the database to use |
| SYS User Password* | Password for the database's highest-privilege administrator account (SYS user) |
| Character Set* | Character encoding to use for the database |
| Timezone* | OS time zone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Target Memory Ratio | Target memory ratio |
| Shared Memory Ratio | Shared memory ratio |
| Redo Log File Size (GB) | Redo log file size |
| System Data File Size (GB) | Size of the data file for storing system tables and key metadata |
| Syssub Data File Size (GB) | Sub data file size for storing system operation-related data |
| User Tablespace Data File Size (GB) | Tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (GB) | Temporary tablespace data file size used for large-scale operations |
| Undo Tablespace Data File Size (GB) | Undo tablespace size |

The * mark indicates a required input item.

{% hint style="warning" %}
**Caution**

Database Name, Character Set, Timezone, and Database Listener Port cannot be modified after the initial configuration.
{% endhint %}

---

# BYOL License Registration <a href="#byol" id="byol"></a>

If the License Option is set to **BYOL**, you must register a license file in the configuration review step in order to request database creation.

1. After reviewing the information entered in the **Configuration Review** step, click the **Register License** button.
2. In the license registration window, click the **Upload** button or drag and drop a file to upload your license file.
3. Check the information in the uploaded license file list.
4. Select the license file to validate.
5. Click the **Validate** button to check the validity of the uploaded license file.
6. If validation succeeds, click the **Create** button to request database creation.

{% hint style="info" %}
**Note**

- You can upload up to 9 license files.
- To delete an uploaded license file, select the file from the list and click the **Delete** button.
{% endhint %}

### Upload File List Items <a href="#upload-files" id="upload-files"></a>

| Item | Description |
| --- | --- |
| License File | Name of the uploaded license file |
| Edition | Edition information listed in the license file |
| CSP | CSP information listed in the license file |
| Topology | Topology information listed in the license file |
| Limit CPU | Maximum usable vCPU count listed in the license file |
| Expired Date | License expiration date |
| Signature | License signature information |

{% hint style="warning" %}
**Caution**

License validation fails in the following cases.

- When the license file's expiration date has passed
- When the Signature value is duplicated among the uploaded license files
- When an already registered license file is uploaded again
- When the information (Edition, CSP, Limit CPU, Expired Date) differs among the uploaded license files
- When the selected database configuration information (Edition, CSP, number of nodes, instance type vCPU) does not match the license file information
{% endhint %}

---

# Checking the Creation Result <a href="#check-result" id="check-result"></a>

Once the database creation request is received, you can check the progress status through system notifications.

- When creation starts, a **DB Service Creation Started** notification is sent.
- When creation completes successfully, a **DB Service Creation Completed** notification is sent.
- If an error occurs during server environment configuration or database installation, a **DB Service Creation Failed** notification is sent, and you can retry according to the guidance.
