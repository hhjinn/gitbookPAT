---
hidden: true
---

# Release Notes Guide

> #### **Release Highlights**
>
> * New support for Source Database PostgreSQL
> * New support for Target Database PostgreSQL
> * Stabilization of Source Oracle

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

#### Synchronization with strings missing after whitespace

When the character sets of Oracle and Tibero differ, strings after whitespace could be truncated during synchronization. This occurred when a VARCHAR column contained whitespace after the string. This issue has been resolved.

### Feature Improvements

#### Synchronization with strings missing after whitespace

When the character sets of Oracle and Tibero differ, strings after whitespace are no longer truncated when a VARCHAR column contains whitespace after the string.

### Usability Improvements

#### Added Skip processing feature

A feature has been added that allows a record to be skipped when an uninterpretable record is found during the Source Oracle extraction process.

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

The following feature is deprecated in ProSync 4.5.

* The metatable containing the deprecated term (`TOP`) has changed:
  * The **PRS\_IMPORTED\_ARCHIVE\_LOG** table was deleted.
  * The **PRS\_INSTALL\_TOP** table was renamed to **PRS\_INSTALL\_INSTANCE**.
  * The **TOP\_ID** column was renamed to **INSTANCE\_ID**.

###
