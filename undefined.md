---
hidden: true
---

# Release Notes Guide

> #### **Release Highlights**
>
> * New support for Source Database PostgreSQL
> * New support for Target Database PostgreSQL
> * Stabilization for Source Oracle

## New Features

#### Added PostgreSQL to Source Database

For PostgreSQL-related constraints, refer to the document below.

#### Added PostgreSQL to Target Database

**Constraints**

* For PostgreSQL databases, bidirectional synchronization is not supported.
* Initial loading through ProSyncManager is not supported.
* DDL synchronization is not supported.
* Consistency checks using Flashback query are not supported.

***

## **Changes**

### Bug Fixes

#### Synchronization with strings after whitespace missing

When the Character Set of Oracle and Tibero differ, the issue where, if a VARCHAR type column contains data with whitespace after a string, the string after the whitespace was truncated during synchronization has been resolved.

### Feature Improvements

#### Synchronization with strings after whitespace missing

When the Character Set of Oracle and Tibero differ, if a VARCHAR type column contains data with whitespace after a string, the string after the whitespace is truncated

### Usability Improvements

#### Added Skip processing feature

A feature has been added that allows skipping a Record when an uninterpretable Record is found during the Source Oracle extraction process.

### Deletion Information

#### Deleted Parameter

**Extract Parameters**

| Parameter Name                  | Remarks |
| ------------------------------- | ------- |
| ORACLE10G\_LOG\_FILE\_BLOCKSIZE | Deleted |
| CLIENT\_DRIVER                  | Deleted |
| LISTENER\_PORT                  | Deleted |

**Apply Parameters**

| Parameter Name | Remarks |
| -------------- | ------- |
| CLIENT\_DRIVER | Deleted |
| LISTENER\_PORT | Deleted |

**LLOB Parameters**

| Parameter Name | Remarks |
| -------------- | ------- |
| CLIENT\_DRIVER | Deleted |
| LISTENER\_PORT | Deleted |

### Deprecated Features

This feature was deprecated in ProSync 4.5.

* The metatable containing the Deprecated term (TOP) has been changed. **PRS\_IMPORTED\_ARCHIVE\_LOG** Table deletion \*\*PRS\_INSTALL\_TOP \*\*The table name **PRS\_INSTALL\_INSTANCE**changed to **TOP\_ID** The column name **INSTANCE\_ID**changed to

###
