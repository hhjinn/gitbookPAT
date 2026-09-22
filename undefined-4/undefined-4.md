OwlDB provides a migration feature that allows you to use an existing database in OwlDB. It includes the Analyzer, which evaluates compatibility before migration, and the Migrator, which performs the migration. When the database operation status is `Running`, only then can the Analyzer and Migrator be executed. However, databases configured for Disaster Recovery (DR) do not support the migration feature. To proceed with migration, please disable the DR configuration or select a different database.

{% hint style="info" %}
**Note**

The compatibility analysis and migration features currently support only Oracle for Tibero. '[Supported Scope and Specifications](#JKEmRa7PUCKiF4VoFAVy)' Please refer to the page.
{% endhint %}

# Supported Scope and Specifications

Check the supported scope and detailed information of the migration feature provided by OwlDB.

## **Supported Databases**

| Source Database | Target Database |
| --- | --- |
| Oracle 11g, 12c, 18c, 19c | Tibero 7 |

{% hint style="info" %}
**Note**

Currently, only CDB migration is supported; PDB migration support is planned for the future.
{% endhint %}

### **Supported Objects**

OwlDB migration migrates all Independent Objects and Dependent Objects at once. Selectively migrating or excluding only individual Objects is not supported.

<table data-full-width="true"><thead><tr><th>Oracle</th><th>Tibero</th><th>Remarks</th></tr></thead><tbody><tr><td>Constraint</td><td>Constraint</td><td>Supports migration for Primary Key, Foreign Key, Check, and Ref Constraint<ul><li>Primary Key index/constraint are all processed as constraints</li><li>The expression of a Check constraint is generated as DDL using the statement stored in Oracle's DD</li></ul></td></tr><tr><td>Index</td><td>Index</td><td><ul><li>R-TREE not supported</li><li>Domain Index not supported</li></ul></td></tr><tr><td>Materialized</td><td>Materialized</td><td>-</td></tr><tr><td>Materialized View Log</td><td>Materialized View Log</td><td>-</td></tr><tr><td>Privilege</td><td>Privilege</td><td>-</td></tr><tr><td>PSM</td><td>PSM</td><td>-</td></tr><tr><td>Role</td><td>Role</td><td>-</td></tr><tr><td>Schema</td><td>Schema</td><td>-</td></tr><tr><td>Sequence</td><td>Sequence</td><td>-</td></tr><tr><td>Synonym</td><td>Synonym</td><td>-</td></tr><tr><td>Table</td><td>Table</td><td><ul><li>Nested Table not supported</li><li>XML Table not supported</li></ul></td></tr><tr><td>Tablespace</td><td>Tablespace</td><td>The tablespace size is increased by 20% during migration -> because the capacity may become larger in the TO-BE than during data migration</td></tr><tr><td>View</td><td>View</td><td>-</td></tr></tbody></table>

### **Data Conversion Types**

This describes the data types that are converted when migrating from Oracle to Tibero.

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

# Analyzer

1. **OwlDB console screen > Management > Migration > Analyzer** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose compatibility you want to evaluate.
3. **Analyze** Click the button.
4. Enter the information for the source database whose compatibility is to be evaluated (hereinafter referred to as the source database).

| Item | Description |
| --- | --- |
| Title* | Database compatibility evaluation title |
| Type* | Engine type of the source database |
| ID* | User ID of the source database |
| Password* | User PW of the source database |
| Address* | IP address name of the source database |
| Port* | Port number of the source database |
| SID* | SID of the source database |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.

1. **Analyze** Click the button.
2. **OwlDB console screen > Management > Migration > Analyzer > Status** When clicked, you can check the progress information.

## **Analyzer results**

**OwlDB console screen > Management > Migration > Analyzer > Analyzer Title** When clicked, you can check the Analyzer results.

---

# Migrator

1. **OwlDB console screen > Management > Migration > Migrator** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database on which to perform the migration.
3. **Migrate** Click the button.
4. Enter the information for the source database on which the migration will be performed.

{% tabs %}
{% tab title="Data Connection" %}
Connects to the source database that is the target of the migration.

| Item | Description |
| --- | --- |
| Title* | Database migration title |
| Type* | Engine type of the source database |
| ID* | Source database user ID |
| Password* | Source database user PW |
| Host* | Source database IP address name |
| Port* | Source database port number |
| SID* | Source database SID |
| Target Database* | Target database alias |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input field.
{% endtab %}
{% tab title="Type Conversion" %}
Retrieves information about the source database's data type conversion.

{% hint style="info" %}
**Note**

Data types that have been converted are emphasized with orange highlighting.
{% endhint %}
{% endtab %}
{% tab title="Summary" %}
Provides a summary of the information entered in the previous step.
{% endtab %}
{% endtabs %}

5. **Migrate** Click the button.
6. **OwlDB console screen > Management > Migration > Migrator > Status** When clicked, you can check the progress information.

## **Migrator Result**

**OwlDB console screen > Management > Migration > Migrator > Migrator Title** When clicked, **Migrator Result**you can check.
