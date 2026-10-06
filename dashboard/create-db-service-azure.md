This guide explains how to create a database (hereinafter provisioning) in OwlDB. Once provisioning is complete, you can use all the features provided by OwlDB.

{% hint style="info" %}
**Note**

- Database creation can be **only by users with Root permission** performed.
- The OpenSQL engine is not supported in the AWS environment.
- **OwlDB console screen > Dashboard > Card View > + icon** or **GNB > DB Alias dropdown > Create DB Service button**Click to navigate to the database creation page.
- You can check the provisioning progress status by clicking the notification (bell) icon at the top right of the console screen or on the dashboard.
- For information on the database engines and instance types supported by OwlDB, please refer to the '[AWS](#XDj4D6jZeLIG3hl9e9W4)', '[Azure](#azure)' page.
{% endhint %}

# Create a new DB Service <a href="#create-new" id="create-new"></a>

1. **OwlDB console screen** > **Dashboard** Navigate to the menu.
2. **Create** Click the button.
3. **Engine options** In this step, select the database engine and license-related information.
4. **DR configuration** In this step, configure whether to use DR and related options.
5. **AZ Configuration** In this step, configure the availability zone.
6. **Instance Configuration** In this step, enter the instance and volume information.
7. **Database Configuration** In this step, enter the database information.
8. **Review Configuration Information** In this step, review all creation information.
9. Click the button according to the License Option to start provisioning. **LI**: **Create** Click Button **BYOL**: **License Registration** Click Button

{% hint style="info" %}
**Note**

- **OwlDB console screen > Dashboard > Card View > + icon** or **GNB > DB Alias dropdown > Create DB Service button**Click to navigate to the database creation page.
- In the Azure environment, the license option is fixed to BYOL (Bring Your Own License), so to complete database creation you must register your license file. For details, [BYOL License Registration](#byol)please refer to.
{% endhint %}

---

# **Creation Options** <a href="#creation-options" id="creation-options"></a>

You can check the estimated cost based on the options selected when creating the database. This amount is calculated based on the Seoul region, and the actual cost may vary depending on various factors such as the region and actual usage.

### Step 1: Engine Options <a href="#step-1-engine" id="step-1-engine"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service Name*</td><td>Name to identify the DB Service<ul><li>Cannot be duplicated within an OwlDB account</li><li>6–30 characters, only uppercase and lowercase English letters (a-z, A-Z), numbers (0-9), and hyphens (-) are allowed; spaces are not allowed</li></ul></td></tr><tr><td>Database Engine Type*</td><td>Database engine to use<ul><li><strong>Tibero</strong></li><li><strong>OpenSQL</strong></li></ul></td></tr><tr><td>License Option*</td><td>License option to use<ul><li><strong>LI</strong>(License Included)</li><li><strong>BYOL</strong> (Bring Your Own License)</li></ul></td></tr><tr><td>Topology*</td><td>Topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong>: Single, HA</li></ul></td></tr><tr><td>Edition*</td><td>License edition<ul><li><strong>Standard Edition (SE)</strong>: For single-server configuration only, supports up to 8 vCPU</li><li><strong>Enterprise Edition (EE)</strong>: Supports high availability and large-scale configurations, no vCPU limit</li><li>If you select TAC or HA for Topology, Enterprise Edition is automatically applied and cannot be changed.</li></ul></td></tr><tr><td>Node Count*</td><td>Number of cluster configuration nodes<ul><li><strong>Tibero</strong>: 1 for Single (fixed), 2–4 selectable for TAC</li><li><strong>OpenSQL</strong>: Fixed to 1 for both Single and HA</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>The PostgreSQL version to use when OpenSQL is selected<ul><li>3.16.12.5 (default)</li><li>3.17.8.5</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- In the Azure environment, the License Option is fixed to BYOL, and to complete database creation you must register your license file.
- If you select Standard Edition (SE) for Edition, you can only select instance types with up to 8 vCPU in the instance configuration step.
{% endhint %}

### Step 2: DR Configuration <a href="#step-2-dr" id="step-2-dr"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration<ul><li><strong>Tibero</strong>: Selected directly by the user</li><li><strong>OpenSQL</strong>: Automatically determined by the Topology and cannot be modified (Single: DR not used / HA: DR used)</li></ul></td></tr><tr><td>Failover Automation Level*</td><td>Failover automation level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td></tr><tr><td>Standby/Replica Count*</td><td>Number of Standby (or Replica) DBs<ul><li><strong>Tibero</strong>: Up to 2 can be selected</li><li><strong>OpenSQL</strong>: Fixed to 1</li></ul></td></tr><tr><td>Standby Mode*</td><td>Standby Mode option (exposed only in the Tibero engine, and can be set individually for each Standby node)<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>The method of transmitting the Primary (Leader)'s logs to the Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that immediately transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that, after a log switch occurs and an archive log file is created, collects and transmits those files</li><li>The OpenSQL engine<strong>ASYNC method</strong>is fixed and cannot be modified.</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- Failover Automation Level, Standby/Replica Count, Standby Mode, and Log Replication Type are exposed only when Enable DR is selected for use.
- In the Azure environment, since the License Option is fixed to BYOL, the Failover Automation Level can be selected from levels 0, 2, and 3. (Level 1 is not supported)
- For the OpenSQL engine, only level 0 or level 3 can be selected for the Failover Automation Level.
- Standby Mode and Log Replication Type can be set individually for each Standby node.
{% endhint %}

### Step 3: AZ Configuration <a href="#step-3-az" id="step-3-az"></a>

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Availability Zone(AZ)* (disabled)</td><td>Availability zone of OwlDB</td></tr><tr><td>Primary(Leader) DB Availability Zone(AZ)*</td><td>Availability zone of the Primary (Leader) DB<br><strong>Default value</strong><ul><li>DR not used: Same zone as OwlDB</li><li>DR used: Different zone from OwlDB</li></ul></td></tr><tr><td>Standby(Replica) DB Availability Zone(AZ)*</td><td>Availability zone of the Standby (Replica) DB<ul><li>Default: Placed in the same availability zone as OwlDB, then automatically placed in a different zone afterward</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- Standby(Replica) DB Availability Zone is displayed only when you select to use Enable DR.
- The availability zone of the Primary (Leader) DB can be selected by the user, but for stable failure response and Failover, it is recommended to place the Primary (Leader) DB in a different availability zone from OwlDB.
{% endhint %}

### Step 4: Instance Configuration <a href="#step-4-instance" id="step-4-instance"></a>

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th>Category</th><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Instance Setting</td><td>DB Virtual Machine Size*</td><td>Instance type that determines performance and specifications</td></tr><tr><td>Instance Access Setting</td><td>DB Instance SSH Key Name*</td><td>Settings for accessing the DB instance</td></tr><tr><td>Data Disk</td><td>Data Disk Type*</td><td>Type of disk for storing primary data</td></tr><tr><td></td><td>Data Disk Size*</td><td>Size of disk for storing primary data</td></tr><tr><td></td><td>Data Disk IOPS*</td><td>I/O throughput of the disk for storing primary data</td></tr><tr><td></td><td>Data Disk MBps</td><td>Maximum processing speed of the disk for storing primary data</td></tr><tr><td>Redo Log Disk</td><td>Redo Log Disk Type</td><td>Type of disk for storing the Redo log</td></tr><tr><td></td><td>Redo Log Disk Size (disabled)</td><td>Size of disk for storing the Redo log<ul><li>Automatically calculated based on the entered Redo Log File Size (GB)</li></ul></td></tr><tr><td></td><td>Redo Log Disk IOPS</td><td>I/O throughput of the disk for storing the Redo log</td></tr><tr><td></td><td>Redo Log Disk MBps</td><td>Maximum processing speed of the disk for storing the Redo log</td></tr><tr><td>Archive Log Volume</td><td>Archive Log Disk Type</td><td>Type of disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk Size</td><td>Size of disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk IOPS</td><td>I/O throughput of the disk for storing the Archive log</td></tr><tr><td></td><td>Archive Log Disk MBps</td><td>Maximum processing speed of the disk for storing the Archive log</td></tr><tr><td>Auto Scale</td><td>Whether to use*</td><td>Whether to automatically expand the data disk size based on data volume usage</td></tr><tr><td></td><td>Maximum expansion limit*</td><td>Maximum size of the data disk that can be increased when Auto Scale is used</td></tr></tbody></table>

The * mark indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Category | Item | Description |
| --- | --- | --- |
| Instance Setting | DB Virtual Machine Size* | Instance type that determines performance and specifications |
| Instance Access Setting | DB Instance SSH Key Name* | Settings for accessing the DB instance |
| Disk | Disk Type* | Type of disk for storing primary data |
|   | Disk Size* | Size of disk for storing primary data |
|   | Disk IOPS* | I/O throughput of the disk for storing primary data |
|   | Disk MBps | Maximum processing speed of the disk for storing primary data |
| Auto Scale | Whether to use* | Whether to automatically expand the data disk size based on data volume usage |
|   | Maximum expansion limit* | Maximum size of the data disk that can be increased when Auto Scale is used |

The * mark indicates a required input field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- For a stable operating environment, all instances in the cluster are automatically configured with the same specifications.
- If you select **Standard Edition (SE)** for Edition, the DB Instance Type list displays only specifications up to 8 vCPU.
- Some availability zones may not support certain DB Instance Types; in such cases, they appear in the list but cannot be selected.
- The IOPS for each disk can only be entered within the range allowed by the selected disk type.
- Redo Log Disk and Archive Log Volume are exposed only in the Tibero engine.
{% endhint %}

### Step 5: Database Configuration <a href="#step-5-database" id="step-5-database"></a>

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name* | The name of the database to be used |
| SYS User Password* | The password for the database super-administrator account (SYS user) |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS timezone in which the database will be installed |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | The maximum number of concurrently allowed sessions |
| Target Memory Ratio | Target memory ratio |
| Shared Memory Ratio | Shared memory ratio |
| Redo Log File Size (GB) | Redo log file size |
| System Data File Size (GB) | The size of the data file for storing system tables and key metadata |
| Syssub Data File Size (GB) | The sub data file size for storing system operation-related data |
| User Tablespace Data File Size (GB) | The tablespace data file size for storing user data |
| Temporary Tablespace Data File Size (GB) | The temporary tablespace data file size used for large-scale operations |
| Undo Tablespace Data File Size (GB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name* | The name of the database to be used |
| Postgres User Password* | The password for the database super-administrator account (Postgres user) |
| Character Set* | The character encoding to be used for the database |
| Timezone* | The OS timezone in which the database will be installed |
| Database Listener Port | The database listener port for network communication |
| Max Session Count | The maximum number of concurrently allowed sessions |
| Shared Buffers | Shared buffer size (recommended value automatically calculated) |
| Wal File Size | The size of a single WAL file |
| Connection Pooler Port | Port on which OpenProxy accepts client connections |
| Extensions | Extension to install |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Caution**

Database Name, Character Set, Timezone, and Database Listener Port cannot be modified after the initial configuration.
{% endhint %}

---

# BYOL License Registration <a href="#byol" id="byol"></a>

In the Azure environment, the license option is fixed to BYOL (Bring Your Own License), so you must register a license file you own to create a database.

1. **Review Configuration Information** After reviewing the information entered in the step, **License Registration** Click the button.
2. In the license registration window, **Upload** click the button or drag and drop the file to upload your license file.
3. Check the information in the uploaded license file list.
4. The file to validate **Selection**After **Validate** Click the button to verify the validity of the uploaded license file.
5. If validation succeeds, **Create** click the button to request database creation.

### Upload file list items <a href="#upload-files" id="upload-files"></a>

| Item | Description |
| --- | --- |
| License file | The name of the uploaded license file |
| Edition | The Edition information stated in the license file |
| CSP | The CSP information stated in the license file |
| Topology | The Topology information stated in the license file |
| Limit CPU | The maximum number of usable vCPUs stated in the license file |
| Expired Date | License expiration date |
| Signature | License signature information |

{% hint style="info" %}
**Note**

- You can upload up to 6 license files.
- To delete an uploaded license file, select the file from the list and then **Delete** Click the button.
{% endhint %}

{% hint style="warning" %}
**Caution**

License validation fails in the following cases.

- When the license file's expiration date has passed
- When the Signature values among the uploaded license files are duplicated
- When an already registered license file is uploaded again
- When the information (Edition, CSP, Limit CPU, Expired Date) among the uploaded license files differs from one another
- When the selected database configuration information (Edition, CSP, number of nodes, vCPUs of the instance type) does not match the information in the license file
{% endhint %}

---

# Checking the creation result <a href="#check-result" id="check-result"></a>

Once the database creation request is received, you can check the progress status through system notifications.

- When creation starts, **DB Service creation started** a notification is sent.
- When creation completes successfully, **DB Service creation completed** a notification is sent.
- If an error occurs during server environment configuration or database installation, **DB Service creation failed** a notification is sent, and you can retry according to the guidance.

{% hint style="info" %}
**Note**

Even if all or some of the Extensions selected in the OpenSQL engine fail to install, the database service itself is created normally, and a separate notification is sent along with it.
{% endhint %}
