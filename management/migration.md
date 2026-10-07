A migration feature is provided to use an existing database in OwlDB. There is an Analyzer feature for compatibility assessment before migration proceeds, and a Migrator feature that performs the migration. The Analyzer and Migrator can only be run when the database operating status is `Running`. However, databases configured for Disaster Recovery (DR) do not support the migration feature. To proceed with migration, please disable the DR configuration or select a different database.

{% hint style="info" %}
**Note**

The compatibility analysis and migration features currently support only Oracle, limited to Tibero. Please refer to the '[Support Scope and Specifications](#JKEmRa7PUCKiF4VoFAVy)' page.
{% endhint %}

# Support Scope and Specifications <a href="#support-scope" id="support-scope"></a>

Check the support scope and detailed information of the migration feature provided by OwlDB.

## **Supported Databases** <a href="#supported-databases" id="supported-databases"></a>

| Source Database | Target Database |
| --- | --- |
| Oracle 11g, 12c, 18c, 19c | Tibero 7 |

{% hint style="info" %}
**Note**

Currently only CDB migration is supported, with PDB migration support planned for the future.
{% endhint %}

### **Supported Objects** <a href="#undefined" id="undefined"></a>

OwlDB migration transfers all Independent Objects and Dependent Objects at once. Selectively migrating or excluding only individual Objects is not supported.

<table><thead><tr><th>Oracle</th><th>Tibero</th><th>Remarks</th></tr></thead><tbody><tr><td>Constraint</td><td>Constraint</td><td>Supports migration of Primary Key, Foreign Key, Check, and Ref Constraints<ul><li>Primary Key indexes/constraints are all handled as constraints</li><li>For Check constraint expressions, DDL is generated using the statements stored in Oracle's DD</li></ul></td></tr><tr><td>Index</td><td>Index</td><td><ul><li>R-TREE not supported</li><li>Domain Index not supported</li></ul></td></tr><tr><td>Materialized</td><td>Materialized</td><td>-</td></tr><tr><td>Materialized View Log</td><td>Materialized View Log</td><td>-</td></tr><tr><td>Privilege</td><td>Privilege</td><td>-</td></tr><tr><td>PSM</td><td>PSM</td><td>-</td></tr><tr><td>Role</td><td>Role</td><td>-</td></tr><tr><td>Schema</td><td>Schema</td><td>-</td></tr><tr><td>Sequence</td><td>Sequence</td><td>-</td></tr><tr><td>Synonym</td><td>Synonym</td><td>-</td></tr><tr><td>Table</td><td>Table</td><td><ul><li>Nested Table not supported</li><li>XML Table not supported</li></ul></td></tr><tr><td>Tablespace</td><td>Tablespace</td><td>Tablespace size is increased by 20% during migration -> because the capacity may become larger in the TO-BE environment than before during data migration</td></tr><tr><td>View</td><td>View</td><td>-</td></tr></tbody></table>

### **Data Conversion Types** <a href="#undefined-1" id="undefined-1"></a>

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

# Analyzer <a href="#analyzer" id="analyzer"></a>

1. Go to the **OwlDB console screen > Management > Migration > Analyzer** menu.
2. Click the **DB Alias** dropdown button to select the database for which compatibility will be assessed.
3. Click the **Analyze** button.
4. Enter the information for the source database whose compatibility will be assessed (hereinafter, source database).

| Item | Description |
| --- | --- |
| Title* | Database compatibility assessment title |
| Type* | Engine type of the source database |
| ID* | User ID of the source database |
| Password* | User PW of the source database |
| Address* | IP address name of the source database |
| Port* | Port number of the source database |
| SID* | SID of the source database |

The * mark indicates a required input item.

1. Click the **Analyze** button.
2. When you click **OwlDB console screen > Management > Migration > Analyzer > Status**, you can check the progress information.

## **Analyzer Results** <a href="#analyzer-results" id="analyzer-results"></a>

When you click **OwlDB console screen > Management > Migration > Analyzer > Analyzer Title**, you can check the Analyzer results.

---

# Migrator <a href="#migrator" id="migrator"></a>

1. Go to the **OwlDB console screen > Management > Migration > Migrator** menu.
2. Click the **DB Alias** dropdown button to select the database on which migration will be performed.
3. Click the **Migrate** button.
4. Enter the information for the source database to be migrated.

{% tabs %}
{% tab title="Data Connection" %}
Connect to the source database that is the migration target.

| Item | Description |
| --- | --- |
| Title* | Database migration title |
| Type* | Engine type of the source database |
| ID* | User ID of the source database |
| Password* | User PW of the source database |
| Host* | IP address name of the source database |
| Port* | Port number of the source database |
| SID* | SID of the source database |
| Target Database* | Alias of the target database |

The * mark indicates a required input item.
{% endtab %}
{% tab title="Type Conversion" %}
Look up information about the data type conversion of the source database.

{% hint style="info" %}
**Note**

Data types whose types have been converted are emphasized with orange highlighting.
{% endhint %}
{% endtab %}
{% tab title="Summary" %}
Provides a summary of the information entered in the previous steps.
{% endtab %}
{% endtabs %}

5. Click the **Migrate** button.
6. When you click **OwlDB console screen > Management > Migration > Migrator > Status**, you can check the progress information.

## **Migrator Results** <a href="#migrator-results" id="migrator-results"></a>

When you click **OwlDB console screen > Management > Migration > Migrator > Migrator Title**, you can check the **Migrator results**.
