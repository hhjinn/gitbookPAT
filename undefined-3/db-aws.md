This guide explains how to create a database (hereafter referred to as provisioning) in OwlDB. Once provisioning is complete, you can use all the features provided by OwlDB.

{% hint style="info" %}
**Note**

- Database creation **only users with Root privileges** can perform this.
- The OpenSQL engine is not supported in the AWS environment.
- **OwlDB Console Screen > Dashboard > Card View > + icon** or **GNB > DB Alias dropdown > Create DB Service button**by clicking, you can navigate to the database creation page.
- You can check the provisioning progress status by clicking the notification (bell) icon at the top right of the console screen or from the dashboard.
- For information on the database engines and instance types supported by OwlDB, please refer to the '[AWS](#XDj4D6jZeLIG3hl9e9W4)', '[Azure](#azure)' pages.
{% endhint %}

{% hint style="warning" %}
**Caution**

In the AWS environment, when you enter the database creation page, the user's **Rocky subscription status**is checked, and if not subscribed, **a guidance modal about the required subscription**appears.
{% endhint %}

# Creating a New DB Service

1. **OwlDB Console Screen > Dashboard** Navigate to the menu.
2. **Create** Click the button.
3. **Engine Options** In this step, select the database engine and license-related information.
4. **DR Configuration** In this step, set whether to use DR and the related options.
5. **AZ Configuration** In this step, configure the availability zone.
6. **Instance Configuration** In this step, enter the instance and volume information.
7. **Database Configuration** In this step, enter the database information.
8. **Configuration Review** In this step, review all the creation information.
9. Click the button according to the License Option to start provisioning. **LI**: **Create** Click the button **BYOL**: **Register License** Click the button

{% hint style="info" %}
**Note**

If you select BYOL as the License Option, license file registration is required. For details, refer to [BYOL License Registration](#byol-라이선스-등록).
{% endhint %}

---

# **Creation Options**

You can check the estimated cost based on the options selected when creating the database. This amount is calculated based on the Seoul region, and the actual amount may vary depending on various factors such as the region and actual usage.

### Step 1: Engine Options

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>DB Service Name*</td><td>A name to identify the DB Service<ul><li>Cannot be duplicated within the OwlDB account</li><li>Must be 6–30 characters, only English letters (a-z, A-Z), numbers (0-9), and hyphens (-) are allowed, spaces are not allowed</li></ul></td></tr><tr><td>Database Engine Type*</td><td>The database engine to use<ul><li><strong>Tibero</strong></li><li><strong>OpenSQL</strong> (to be supported later)</li></ul></td></tr><tr><td>License Option*</td><td>The license option to use<ul><li><strong>LI</strong>(License Included)</li><li><strong>BYOL</strong> (Bring Your Own License)</li></ul></td></tr><tr><td>Topology*</td><td>The topology type that determines the database structure<ul><li><strong>Tibero</strong>: Single, TAC</li></ul></td></tr><tr><td>Edition*</td><td>The edition of the license<ul><li><strong>Standard Edition (SE)</strong>: For single-server configuration only, up to 8vCPU available</li><li><strong>Enterprise Edition (EE)</strong>: Supports high availability and large-scale configurations, no vCPU limit</li><li>Selecting TAC for Topology automatically applies Enterprise Edition, which cannot be changed.</li></ul></td></tr><tr><td>Node Count*</td><td>Number of nodes composing the cluster<ul><li><strong>Tibero</strong>: Single fixed at 1, TAC selectable from 2 to 4</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

If you select Standard Edition (SE) for Edition, only instance types up to 8 vCPU can be selected in the instance configuration step.
{% endhint %}

### Step 2: DR Configuration

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Enable DR*</td><td>Whether to use DR configuration<ul><li><strong>Tibero</strong>: Selected directly by the user</li></ul></td></tr><tr><td>Failover Automation Level*</td><td>Failover automation level<ul><li><strong>Level 0: Manual</strong></li><li><strong>Level 1: Automatic failover</strong></li><li><strong>Level 2: Automatic configuration recovery</strong></li><li><strong>Level 3: Full automation</strong></li></ul></td></tr><tr><td>Standby/Replica Count*</td><td>Number of Standby (or Replica) DBs<ul><li><strong>Tibero</strong>: Up to 2 can be selected</li></ul></td></tr><tr><td>Standby Mode*</td><td>Standby Mode option (can be configured individually per Standby node)<ul><li><strong>Recovery</strong></li><li><strong>Read Only</strong></li></ul></td></tr><tr><td>Log Replication Type</td><td>The method of transmitting the Primary's logs to the Standby<ul><li><strong>LGWR ASYNC</strong>: A replication mode that immediately transmits the Redo log generated in real time when a transaction occurs</li><li><strong>ARCH ASYNC</strong>: A replication mode that, after a log switch occurs and archive log files are generated, collects and transmits those files</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- Failover Automation Level, Standby (Replica) Count, Standby Mode, and Log Replication Type are displayed only when Enable DR is selected for use.
- The Failover Automation Level has different configurable levels depending on the license type. LI allows Levels 0 to 3, and BYOL allows Levels 0, 2, and 3.
- Standby Mode and Log Replication Type can be configured individually per Standby node.
{% endhint %}

### Step 3: AZ Configuration

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>OwlDB Availability Zone (AZ)* (disabled)</td><td>Availability zone of OwlDB</td></tr><tr><td>Primary (Leader) DB Availability Zone (AZ)*</td><td>Availability zone of the Primary (Leader) DB<br><strong>Default value</strong><ul><li>DR not used: Same zone as OwlDB</li><li>DR used: Different zone from OwlDB</li></ul></td></tr><tr><td>Standby (Replica) DB Availability Zone (AZ)*</td><td>Availability zone of the Standby (Replica) DB<ul><li>Default value: Placed in the same availability zone as OwlDB, then automatically placed in a different zone afterward</li></ul></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- The Standby (Replica) DB Availability Zone is displayed only when Enable DR is selected for use.
- The availability zone of the Primary (Leader) DB can be selected by the user, but for stable fault response and Failover, it is recommended to place the Primary (Leader) DB in a different availability zone from OwlDB.
{% endhint %}

### Step 4: Instance Configuration

<table data-full-width="true"><thead><tr><th>Category</th><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Instance Setting</td><td>DB Virtual Machine Size*</td><td>The instance type that will determine performance and specifications</td></tr><tr><td>Instance Access Setting</td><td>DB Instance SSH Key Name*</td><td>Settings for accessing the DB instance</td></tr><tr><td>Data Disk</td><td>Data Disk Type*</td><td>The type of disk that will store the main data</td></tr><tr><td></td><td>Data Disk Size*</td><td>The size of the disk that will store the main data</td></tr><tr><td></td><td>Data Disk IOPS*</td><td>The I/O throughput of the disk that will store the main data</td></tr><tr><td></td><td>Data Disk MBps</td><td>The maximum processing speed of the disk that will store the main data</td></tr><tr><td>Redo Log Disk</td><td>Redo Log Disk Type</td><td>The type of disk that will store the Redo log</td></tr><tr><td></td><td>Redo Log Disk Size (disabled)</td><td>The size of the disk that will store the Redo log<ul><li>Automatically calculated based on the entered Redo Log File Size (GB)</li></ul></td></tr><tr><td></td><td>Redo Log Disk IOPS</td><td>I/O throughput of the disk that stores the Redo log</td></tr><tr><td></td><td>Redo Log Disk MBps</td><td>Maximum processing speed of the disk that stores the Redo log</td></tr><tr><td>Archive Log Volume</td><td>Archive Log Disk Type</td><td>Type of the disk that stores the Archive log</td></tr><tr><td></td><td>Archive Log Disk Size</td><td>Size of the disk that stores the Archive log</td></tr><tr><td></td><td>Archive Log Disk IOPS</td><td>I/O throughput of the disk that stores the Archive log</td></tr><tr><td></td><td>Archive Log Disk MBps</td><td>Maximum processing speed of the disk that stores the Archive log</td></tr><tr><td>Auto Scale</td><td>Enable/Disable*</td><td>Whether to automatically expand the data disk size based on data volume usage</td></tr><tr><td></td><td>Maximum Expansion Limit*</td><td>Maximum size the data disk can grow to when Auto Scale is enabled</td></tr></tbody></table>

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- For a stable operating environment, all instances within the cluster are automatically configured with the same specifications.
- When Edition is selected as **Standard Edition (SE)**, the DB Instance Type list displays only specifications up to 8 vCPU.
- Some availability zones may have DB Instance Types that are not supported, in which case they appear in the list but cannot be selected.
- The IOPS of each disk can only be entered within the range allowed by the selected disk type.
- Redo Log Disk and Archive Log Volume are exposed only in the Tibero engine.
{% endhint %}

### Step 5: Database Configuration

| Item | Description |
| --- | --- |
| Database Name* | Name of the database to use |
| SYS User Password* | Password of the database's highest-privilege administrator account (SYS user) |
| Character Set* | Character encoding to use for the database |
| Timezone* | OS time zone where the database will be installed |
| Database Listener Port | Database listener port for network communication |
| Max Session Count | Maximum number of concurrently allowed sessions |
| Target Memory Ratio | Target memory ratio |
| Shared Memory Ratio | Shared memory ratio |
| Redo Log File Size (GB) | Redo log file size |
| System Data File Size (GB) | Size of the data file that stores system tables and key metadata |
| Syssub Data File Size (GB) | Size of the sub data file for storing system operation-related data |
| User Tablespace Data File Size (GB) | Size of the tablespace data file that stores user data |
| Temporary Tablespace Data File Size (GB) | Size of the temporary tablespace data file used for large-scale operations |
| Undo Tablespace Data File Size (GB) | Undo tablespace size |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

{% hint style="warning" %}
**Caution**

Database Name, Character Set, Timezone, and Database Listener Port cannot be modified after the initial setup.
{% endhint %}

---

# BYOL License Registration

License Option **BYOL**When selected, you must register a license file in the configuration information review step before you can request database creation.

1. **Configuration Information Review** After reviewing the information entered in the step, **Register License** Click the button.
2. In the license registration window, **Upload** Click the button or drag and drop the file to upload the license file you have.
3. Check the information in the uploaded license file list.
4. Select the license file to validate.
5. **Validate** Click the button to verify the validity of the uploaded license file.
6. When validation succeeds, **Create** Click the button to request database creation.

{% hint style="info" %}
**Note**

- Up to 9 license files can be uploaded.
- To delete an uploaded license file, select the file from the list and then **Delete** click the button.
{% endhint %}

### Upload File List Items

| Item | Description |
| --- | --- |
| License File | The name of the uploaded license file |
| Edition | The Edition information specified in the license file |
| CSP | The CSP information specified in the license file |
| Topology | The Topology information specified in the license file |
| Limit CPU | The maximum number of usable vCPUs specified in the license file |
| Expired Date | The license expiration date |
| Signature | The license signature information |

{% hint style="warning" %}
**Caution**

License validation fails in the following cases.

- When the expiration date of the license file has passed
- When the Signature values are duplicated among the uploaded license files
- When an already registered license file is uploaded again
- When the information (Edition, CSP, Limit CPU, Expired Date) differs among the uploaded license files
- When the selected database configuration information (Edition, CSP, number of nodes, vCPU of the instance type) does not match the information in the license file
{% endhint %}

---

# Checking the Creation Result

Once the database creation request is received, you can check the progress status through system notifications.

- When creation starts, **DB Service Creation Started** a notification is sent.
- When creation completes successfully, **DB Service Creation Completed** a notification is sent.
- If an error occurs during the server environment configuration or database installation process, **DB Service Creation Failed** a notification is sent, and you can retry according to the guidance.
