A migration function is provided to use an existing database in OwlDB. Before proceeding with migration, there is an Analyzer for compatibility assessment and a Migrator function that performs the migration. Analyzer and Migrator can be performed only when the database operation status is `Running`. However, migration functions are not provided for databases with a Disaster Recovery (DR) configuration. To proceed with migration, please release the DR configuration or select another database.

{% hint style="info" %}
**Note**

Compatibility analysis and migration functions currently support only Oracle for Tibero. '[Support Scope and Specifications](#JKEmRa7PUCKiF4VoFAVy)' page.
{% endhint %}

# Support Scope and Specifications <a href="#support-scope" id="support-scope"></a>

Check the support scope and detailed information of the migration function provided by OwlDB.

## **Supported Databases** <a href="#supported-databases" id="supported-databases"></a>

| Source Database | Target Database |
| --- | --- |
| Oracle 11g, 12c, 18c, 19c | Tibero 7 |

{% hint style="info" %}
**Note**

Currently only CDB migration is supported, and PDB migration support is planned for the future.
{% endhint %}

### **Supported Objects** <a href="#undefined" id="undefined"></a>

OwlDB migration migrates all Independent Objects and Dependent Objects at once. Selectively migrating or excluding only individual Objects is not supported.

<table><thead><tr><th>Oracle</th><th>Tibero</th><th>Remarks</th></tr></thead><tbody><tr><td>Constraint</td><td>Constraint</td><td>Supports migration for Primary Key, Foreign Key, Check, and Ref Constraint<ul><li>Primary Key index/constraint are all handled as constraint</li><li>The expression of the Check constraint generates DDL using the statement stored in Oracle's DD</li></ul></td></tr><tr><td>Index</td><td>Index</td><td><ul><li>R-TREE not supported</li><li>Domain Index not supported</li></ul></td></tr><tr><td>Materialized</td><td>Materialized</td><td>-</td></tr><tr><td>Materialized View Log</td><td>Materialized View Log</td><td>-</td></tr><tr><td>Privilege</td><td>Privilege</td><td>-</td></tr><tr><td>PSM</td><td>PSM</td><td>-</td></tr><tr><td>Role</td><td>Role</td><td>-</td></tr><tr><td>Schema</td><td>Schema</td><td>-</td></tr><tr><td>Sequence</td><td>Sequence</td><td>-</td></tr><tr><td>Synonym</td><td>Synonym</td><td>-</td></tr><tr><td>Table</td><td>Table</td><td><ul><li>Nested Table not supported</li><li>XML Table not supported</li></ul></td></tr><tr><td>Tablespace</td><td>Tablespace</td><td>The size of the tablespace is increased by 20% during migration -> because the capacity may become larger than in the TO-BE during data migration</td></tr><tr><td>View</td><td>View</td><td>-</td></tr></tbody></table>

### **Data Conversion Types** <a href="#undefined-1" id="undefined-1"></a>

Provides guidance on the data types that are converted when migrating from Oracle to Tibero.

| Oracle | Tibero |
| --- | --- |
| blob | BLOB |
| binary_float | BINARY_FLOAT |
| binary_double | BINARY_DOUBLE |
| character | CHAR |
| clob | CLOB |
| date | DATE |
| interval day to second | INTERVAL DAY(2) TO SECOND(6) |
| interval year to month | INTERVAL YEAR(2) TO MONTH |
| long | LONG |
| long raw | LONG RAW |
| nchar | NCHAR |
| nclob | NCLOB |
| number | NUMBER |
| nvarchar2 | NVARCHAR2 |
| rowid | ROWID |
| time | TIME |
| timestamp | TIMESTAMP |
| timestamp with time zone | TIMESTAMP WITH TIME ZONE |
| timestamp with local time zone | TIMESTAMP(6) WITH LOCAL TIME ZONE |
| varchar | VARCHAR |
| varchar2 | VARCHAR2 |
| xmltype | XMLTYPE |

---

# Analyzer <a href="#analyzer" id="analyzer"></a>

1. **OwlDB console screen > Management > Migration > Analyzer** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database for which to assess compatibility.
3. **Analysis** Click the button.
4. Enter the information of the source database (hereinafter source database) for which to assess compatibility.

| Item | Description |
| --- | --- |
| Title* | Database compatibility assessment title |
| Type* | Engine type of the source database |
| ID* | User ID of the source database |
| Password* | User PW of the source database |
| Address* | IP address name of the source database |
| Port* | Port number of the source database |
| SID* | SID of the source database |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

1. **Analysis** Click the button.
2. **OwlDB console screen > Management > Migration > Analyzer > Status** When clicked, you can check the progress information.

## **Analyzer Results** <a href="#analyzer-results" id="analyzer-results"></a>

**OwlDB console screen > Management > Migration > Analyzer > Analyzer Title** When clicked, you can check the Analyzer results.

---

# Migrator <a href="#migrator" id="migrator"></a>

1. **OwlDB console screen > Management > Migration > Migrator** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database on which to perform the migration.
3. **Migration** Click the button.
4. Enter the information of the source database on which to perform the migration.

{% tabs %}
{% tab title="Data Connection" %}
Connects to the source database that is the target of the migration.

| Item | Description |
| --- | --- |
| Title* | Database Migration Title |
| Type* | Engine type of the source database |
| ID* | User ID of the source database |
| Password* | User PW of the source database |
| Host* | IP address name of the source database |
| Port* | Port number of the source database |
| SID* | SID of the source database |
| Target Database* | Alias of the target database |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% tab title="Type Conversion" %}
Retrieves information about data type conversion of the source database.

{% hint style="info" %}
**Note**

Data types that have been converted are emphasized with orange highlighting.
{% endhint %}
{% endtab %}
{% tab title="Summary" %}
Provides a summary of the information entered in the previous step.
{% endtab %}
{% endtabs %}

5. **Migration** Click the button.
6. **OwlDB Console Screen > Management > Migration > Migrator > Status** When clicked, you can check the progress information.

## **Migrator Results** <a href="#migrator-results" id="migrator-results"></a>

**OwlDB Console Screen > Management > Migration > Migrator > Migrator Title** When clicked, **Migrator Results**can be viewed.
