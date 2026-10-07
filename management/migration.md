A migration feature is provided to use an existing database in OwlDB. Before proceeding with migration, there is an Analyzer for compatibility assessment and a Migrator feature for performing the migration. The Analyzer and Migrator can only be executed when the database operation status is `Running`. However, databases configured with Disaster Recovery (DR) do not support the migration feature. To proceed with migration, please disable the DR configuration or select a different database.

{% hint style="info" %}
**Note**

The compatibility analysis and migration features currently support only Oracle for Tibero. Please refer to the '[Support Scope and Specifications](#JKEmRa7PUCKiF4VoFAVy)' page.
{% endhint %}

# Support Scope and Specifications <a href="#support-scope" id="support-scope"></a>

Check the support scope and detailed information of the migration feature provided by OwlDB.

## **Supported Databases** <a href="#supported-databases" id="supported-databases"></a>

| Source Database | Target Database |
| --- | --- |
| Oracle 11g, 12c, 18c, 19c | Tibero 7 |

{% hint style="info" %}
**Note**

Currently, only CDB migration is supported, and PDB migration support is planned for the future.
{% endhint %}

### **Supported Objects** <a href="#undefined" id="undefined"></a>

OwlDB migration transfers all Independent Objects and Dependent Objects at once. Selectively transferring or excluding only individual Objects is not supported.

<table><thead><tr><th>Oracle</th><th>Tibero</th><th>Remarks</th></tr></thead><tbody><tr><td>Constraint</td><td>Constraint</td><td>Supports migration of Primary Key, Foreign Key, Check, and Ref Constraints<ul><li>Primary Key index/constraint are all handled as constraints</li><li>The Check constraint expression generates DDL using the statements stored in Oracle's DD</li></ul></td></tr><tr><td>Index</td><td>Index</td><td><ul><li>R-TREE not supported</li><li>Domain Index not supported</li></ul></td></tr><tr><td>Materialized</td><td>Materialized</td><td>-</td></tr><tr><td>Materialized View Log</td><td>Materialized View Log</td><td>-</td></tr><tr><td>Privilege</td><td>Privilege</td><td>-</td></tr><tr><td>PSM</td><td>PSM</td><td>-</td></tr><tr><td>Role</td><td>Role</td><td>-</td></tr><tr><td>Schema</td><td>Schema</td><td>-</td></tr><tr><td>Sequence</td><td>Sequence</td><td>-</td></tr><tr><td>Synonym</td><td>Synonym</td><td>-</td></tr><tr><td>Table</td><td>Table</td><td><ul><li>Nested Table not supported</li><li>XML Table not supported</li></ul></td></tr><tr><td>Tablespace</td><td>Tablespace</td><td>The tablespace size is increased by 20% during migration -> because the capacity may become larger than in the TO-BE during data migration</td></tr><tr><td>View</td><td>View</td><td>-</td></tr></tbody></table>

### **Data Conversion Type** <a href="#undefined-1" id="undefined-1"></a>

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

1. **OwlDB Console Screen > Management > Migration > Analyzer** Go to the menu.
2. **DB Alias** Click the dropdown button to select the database whose compatibility will be assessed.
3. **Analyze** Click the button.
4. Enter the information of the source database whose compatibility will be assessed (hereinafter referred to as the source database).

| Item | Description |
| --- | --- |
| Title* | Database Compatibility Assessment Title |
| Type* | The engine type of the source database |
| ID* | The user ID of the source database |
| Password* | The user PW of the source database |
| Address* | The IP address name of the source database |
| Port* | The port number of the source database |
| SID* | The SID of the source database |

The * notation indicates a required input item.

1. **Analyze** Click the button.
2. **OwlDB Console Screen > Management > Migration > Analyzer > Status** When clicked, you can check the progress information.

## **Analyzer Result** <a href="#analyzer-results" id="analyzer-results"></a>

**OwlDB Console Screen > Management > Migration > Analyzer > Analyzer Title** When clicked, you can check the Analyzer result.

---

# Migrator <a href="#migrator" id="migrator"></a>

1. **OwlDB Console Screen > Management > Migration > Migrator** Go to the menu.
2. **DB Alias** Click the dropdown button to select the database on which to perform the migration.
3. **Migrate** Click the button.
4. Enter the information of the source database on which the migration will be performed.

{% tabs %}
{% tab title="Data Connection" %}
Connect to the source database that is the target of the migration.

| Item | Description |
| --- | --- |
| Title* | Database Migration Title |
| Type* | The engine type of the source database |
| ID* | The user ID of the source database |
| Password* | The user PW of the source database |
| Host* | The IP address name of the source database |
| Port* | The port number of the source database |
| SID* | The SID of the source database |
| Target Database* | The alias of the target database |

The * notation indicates a required input item.
{% endtab %}
{% tab title="Type Conversion" %}
Look up information regarding the data type conversion of the source database.

{% hint style="info" %}
**Note**

Data types whose type has been converted are emphasized with orange highlighting.
{% endhint %}
{% endtab %}
{% tab title="Summary" %}
Provides a summary of the information entered in the previous steps.
{% endtab %}
{% endtabs %}

5. **Migrate** Click the button.
6. **OwlDB Console Screen > Management > Migration > Migrator > Status** When clicked, you can check the progress information.

## **Migrator Result** <a href="#migrator-results" id="migrator-results"></a>

**OwlDB Console Screen > Management > Migration > Migrator > Migrator Title** When clicked, **Migrator Result**can be checked.
