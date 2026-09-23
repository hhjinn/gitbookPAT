This guide explains how to create a database (hereafter referred to as provisioning) in OwlDB. Once provisioning is complete, you can use all the features provided by OwlDB.

{% hint style="info" %}
**Note**

- Database creation can be **performed only by users with Root privileges.** [T_5]
The OpenSQL engine is not supported in the AWS environment.
- AWS 환경에서는 OpenSQL 엔진을 지원하지 않습니다.
- **OwlDB console screen > Dashboard > Card view > + icon** or **GNB > DB Alias dropdown > Create DB Service button**You can navigate to the database creation page by clicking it.
- You can check the provisioning progress status by clicking the notification (bell) icon in the top-right corner of the console screen or from the Dashboard.
- For information on the database engines and instance types supported by OwlDB, refer to the '[AWS](#XDj4D6jZeLIG3hl9e9W4)', '[Azure](#azure)' pages.
{% endhint %}

# Creating a New DB Service

1. **OwlDB console screen** > **Dashboard** Navigate to the menu.
2. **Create** Click the button.
3. **Engine Options** In this step, select information related to the database engine and license.
4. **DR Configuration** In this step, set whether to use DR and the related options.
5. **AZ Configuration** In this step, set the availability zone.
6. **Instance Configuration** In this step, enter the instance and volume information.
7. **Database Configuration** In this step, enter the database information.
8. **Configuration Information Review** In this step, review all creation information.
9. Start provisioning by clicking the button according to the License Option. **LI**: **Create** Click the button **BYOL**: **Register License** Click the button

{% hint style="info" %}
**Note**

- **OwlDB console screen > Dashboard > Card view > + icon** or **GNB > DB Alias dropdown > Create DB Service button**You can navigate to the database creation page by clicking it.
- In the Azure environment, the license option is fixed to BYOL (Bring Your Own License), so you must register your own license file to complete database creation. For details, refer to [BYOL License Registration](#byol-라이선스-등록)Please refer to it.
{% endhint %}

---

# **Creation Options**

You can check the estimated cost based on the options selected during database creation. This amount is calculated based on the Seoul region, and the actual amount may vary depending on various factors such as the region and actual usage.

### Step 1: Engine Options

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service Name*</td><td>A name to identify the DB Service<ul><li>Cannot be used more than once within an OwlDB account</li><li>Must be 6–30 characters, using only English uppercase and lowercase letters (a-z, A-Z), numbers (0-9), and hyphens (-); spaces are not allowed</li></ul></td></tr><tr><td>Database Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong></li><li><strong>OpenSQL</strong></li></ul></td></tr><tr><td>License Option*</td><td>The license option to use<ul><li><strong>LI</strong>(License Included)</li><li><strong>BYOL</strong> (Bring Your Own License)</li></ul></td></tr><tr><td>Topology*</td><td>The topology type that will determine the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li><li><strong>OpenSQL</strong>: Single, HA</li></ul></td></tr><tr><td>Edition*</td><td>The edition of the license<ul><li><strong>Standard Edition (SE)</strong>: For single-server configuration only, up to 8vCPU available</li><li><strong>Enterprise Edition (EE)</strong>: High availability and large-scale configuration support, no vCPU limit</li><li>Selecting TAC or HA for the Topology automatically applies the Enterprise Edition, which cannot be changed</li></ul></td></tr><tr><td>Node Count*</td><td>Number of nodes in the cluster configuration<ul><li><strong>Tibero</strong>: Single fixed at 1, TAC selectable from 2 to 4</li><li><strong>OpenSQL</strong>: Both Single and HA fixed at 1</li></ul></td></tr><tr><td>PostgreSQL Version</td><td>PostgreSQL version to use when OpenSQL is selected<ul><li>3.16.12.5 (default)</li><li>3.17.8.5</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- In the Azure environment, the License Option is fixed to BYOL, and you must register the license file you hold to complete database creation.
- If you select Standard Edition (SE) for the Edition, only instance types up to 8 vCPU can be selected during the instance configuration step.
{% endhint %}

### Step 2: DR Configuration

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration<ul><li><strong>Tibero</strong>: Selected directly by the user</li><li><strong>OpenSQL</strong>: Automatically determined by the Topology and cannot be modified (Single: DR not used / HA: DR used)</li></ul></td></tr><tr><td>Failover Automation Level*</td><td>Failover automation level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td></tr><tr><td>Standby/Replica Count*</td><td>Number of Standby (or Replica) DBs<ul><li><strong>Tibero</strong>: Up to 2 can be selected</li><li><strong>OpenSQL</strong>: Fixed at 1</li></ul></td></tr><tr><td>Standby Mode*</td><td>Standby Mode option (exposed only in the Tibero engine and can be set individually per Standby node)<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>The method of transmitting logs from the Primary (Leader) to the Standby (Replica)<ul><li><strong>LGWR ASYNC</strong>: A replication mode that immediately transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that, after a log switch occurs and an archive log file is created, collects and transmits those files</li><li>The OpenSQL engine is<strong>ASYNC mode</strong>is fixed and cannot be modified.</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- Failover Automation Level, Standby/Replica Count, Standby Mode, and Log Replication Type are exposed only when Enable DR is selected for use.
- In the Azure environment, since the License Option is fixed to BYOL, the Failover Automation Level can be selected from levels 0, 2, and 3. (Level 1 is not supported)
- The OpenSQL engine can only select Level 0 or Level 3 for the Failover Automation Level.
- Standby Mode and Log Replication Type can be set individually per Standby node.
{% endhint %}

### Step 3: AZ Configuration

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Availability Zone (AZ)* (disabled)</td><td>Availability zone of OwlDB</td></tr><tr><td>Primary (Leader) DB Availability Zone (AZ)*</td><td>Availability zone of the Primary (Leader) DB<br><strong>Default</strong><ul><li>DR not used: Same zone as OwlDB</li><li>DR used: Different zone from OwlDB</li></ul></td></tr><tr><td>Standby (Replica) DB Availability Zone (AZ)*</td><td>Availability zone of the Standby (Replica) DB<ul><li>Default: Placed in the same availability zone as OwlDB, then automatically placed in a different zone afterward</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- The Standby (Replica) DB Availability Zone is exposed only when Enable DR is selected for use.
- The availability zone of the Primary (Leader) DB can be selected by the user, but for stable failure response and Failover, it is recommended to place the Primary (Leader) DB in a different availability zone from OwlDB.
{% endhint %}

### Step 4: Instance Configuration

{% tabs %}
{% tab title="Tibero" %}
<table data-full-width="true"><thead><tr><th>Category</th><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Instance Setting</td><td>DB Virtual Machine Size*</td><td>Instance type that determines performance and specifications</td></tr><tr><td>Instance Access Setting</td><td>DB Instance SSH Key Name*</td><td>Settings for accessing the DB instance</td></tr><tr><td>Data Disk</td><td>Data Disk Type*</td><td>Type of disk for storing primary data</td></tr><tr><td></td><td>Data Disk Size*</td><td>Size of disk for storing primary data</td></tr><tr><td></td><td>Data Disk IOPS*</td><td>Input/output throughput of the disk for storing primary data</td></tr><tr><td></td><td>Data Disk MBps</td><td>Maximum throughput speed of the disk for storing primary data</td></tr><tr><td>Redo Log Disk</td><td>Redo Log Disk Type</td><td>Type of disk for storing Redo log</td></tr><tr><td></td><td>Redo Log Disk Size (disabled)</td><td>Size of disk for storing Redo log<ul><li>Automatically calculated based on the entered Redo Log File Size (GB)</li></ul></td></tr><tr><td></td><td>Redo Log Disk IOPS</td><td>Input/output throughput of the disk for storing Redo log</td></tr><tr><td></td><td>Redo Log Disk MBps</td><td>Maximum throughput speed of the disk for storing Redo log</td></tr><tr><td>Archive Log Volume</td><td>Archive Log Disk Type</td><td>Type of disk for storing Archive log</td></tr><tr><td></td><td>Archive Log Disk Size</td><td>Size of disk for storing Archive log</td></tr><tr><td></td><td>Archive Log Disk IOPS</td><td>Input/output throughput of the disk for storing Archive log</td></tr><tr><td></td><td>Archive Log Disk MBps</td><td>Maximum throughput speed of the disk for storing Archive log</td></tr><tr><td>Auto Scale</td><td>Enable/Disable*</td><td>Whether to automatically expand the data disk size according to data volume usage</td></tr><tr><td></td><td>Maximum expansion limit*</td><td>Maximum size of the data disk that can be increased when Auto Scale is enabled</td></tr></tbody></table>

The * mark indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Category | Item | Description |
| --- | --- | --- |
| Instance Setting | DB Virtual Machine Size* | Instance type that determines performance and specifications |
| Instance Access Setting | DB Instance SSH Key Name* | Settings for accessing the DB instance |
| Disk | Disk Type* | Type of disk for storing primary data |
|   | Disk Size* | Size of disk for storing primary data |
|   | Disk IOPS* | Input/output throughput of the disk for storing primary data |
|   | Disk MBps | Maximum throughput speed of the disk for storing primary data |
| Auto Scale | Enable/Disable* | Whether to automatically expand the data disk size according to data volume usage |
|   | Maximum expansion limit* | Maximum size of the data disk that can be increased when Auto Scale is enabled |

The * mark indicates a required input field.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**

- For a stable operating environment, all instances within the cluster are automatically configured with the same specifications.
- If the Edition is selected as **Standard Edition(SE)**, the DB Instance Type list will only display specifications up to a maximum of 8 vCPUs.
- Some availability zones may have DB Instance Types that are not supported; in this case, they are shown in the list but cannot be selected.
- The IOPS for each disk can only be entered within the range allowed by the selected disk type.
- Redo Log Disk and Archive Log Volume are exposed only in the Tibero engine.
{% endhint %}

### Step 5: Database Configuration

{% tabs %}
{% tab title="Tibero" %}
| Item | Description |
| --- | --- |
| Database Name* | Name of the database to use |
| SYS User Password* | Password of the database's highest-privilege administrator account (SYS user) |
| Character Set* | Character encoding to use for the database |
| Timezone* | OS timezone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Target Memory Ratio | Target memory ratio |
| Shared Memory Ratio | Shared memory ratio |
| Redo Log File Size (GB) | Redo log file size |
| System Data File Size (GB) | Size of the data file for storing system tables and key metadata |
| Syssub Data File Size (GB) | Size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (GB) | Size of the tablespace data file for storing user data |
| Temporary Tablespace Data File Size (GB) | Size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (GB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% tab title="OpenSQL" %}
| Item | Description |
| --- | --- |
| Database Name* | Name of the database to use |
| Postgres User Password* | Password for the highest-privilege administrator account (Postgres user) of the database |
| Character Set* | Character encoding to use for the database |
| Timezone* | OS timezone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Shared Buffers | Shared buffer size (recommended value calculated automatically) |
| Wal File Size | Size of a single WAL file |
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

# BYOL License Registration

In the Azure environment, the license option is fixed to BYOL (Bring Your Own License), so you must register the license file you hold to create a database.

1. **Check the configuration information** After reviewing the information entered in the step, **Register License** click the button.
2. In the license registration window, **Upload** click the button or drag and drop the file to upload the license file you hold.
3. Check the information in the uploaded license file list.
4. The file to validate **Select**after **Validate** click the button to verify the validity of the uploaded license file.
5. If validation succeeds, **Create** click the button to request database creation.

### Upload File List Items

| Item | Description |
| --- | --- |
| License File | Name of the uploaded license file |
| Edition | Edition information stated in the license file |
| CSP | CSP information stated in the license file |
| Topology | Topology information stated in the license file |
| Limit CPU | Maximum number of usable vCPUs stated in the license file |
| Expired Date | License Expiration Date |
| Signature | License Signature Information |

{% hint style="info" %}
**Note**

- Up to 6 license files can be uploaded.
- To delete an uploaded license file, select the file from the list and then **Delete** click the button.
{% endhint %}

{% hint style="warning" %}
**Caution**

License verification fails in the following cases.

- When the license file's expiration date has passed
- When the Signature values are duplicated among the uploaded license files
- When an already registered license file is uploaded again
- When the information (Edition, CSP, Limit CPU, Expired Date) differs among the uploaded license files
- When the selected database configuration information (Edition, CSP, number of nodes, instance type's vCPU) does not match the information in the license file
{% endhint %}

---

# Checking the Creation Result

Once the database creation request is received, you can check the progress status through system notifications.

- When creation starts, **DB Service Creation Started** a notification is sent.
- When creation completes normally, **DB Service Creation Completed** a notification is sent.
- If an error occurs during server environment configuration or database installation, **DB Service Creation Failed** a notification is sent, and you can retry according to the guidance.

{% hint style="info" %}
**Note**

Even if the installation of the selected Extension in the OpenSQL engine fails entirely or partially, the database service itself is created normally, and a separate guidance notification is sent along with it.
{% endhint %}
