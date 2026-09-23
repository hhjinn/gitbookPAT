> ### **Release Highlights**
> 
> - New support for Source Database PostgreSQL
> - New support for Target Database PostgreSQL
> - Stabilization of Source Oracle

# New Features

### Added PostgreSQL to Source Database

For PostgreSQL-related constraints, refer to the document below.



### Added PostgreSQL to Target Database

**Constraints**

- For PostgreSQL databases, bidirectional synchronization is not supported.
- Initial loading through ProSyncManager is not supported.
- DDL synchronization is not supported.
- Consistency checks using Flashback query are not supported.



---

# **Changes**

## Bug Fixes

### Synchronization with strings missing after whitespace 

When the Character Set of Oracle and Tibero differ, an issue where strings after a whitespace were truncated during synchronization when a VARCHAR type column contained data with whitespace after the string has been resolved.



## Feature Improvements

### Synchronization with strings missing after whitespace 

When the Character Set of Oracle and Tibero differ, if a VARCHAR type column contains data with whitespace after the string, the string after the whitespace is truncated 



## Usability Improvements

### Added Skip processing feature

A feature has been added that allows skipping a Record when an uninterpretable Record is found during the Source Oracle extraction process.





## Deletion Information

### Deleted Parameter



**Extract Parameters**

| Parameter Name | Remarks |
| --- | --- |
| ORACLE10G_LOG_FILE_BLOCKSIZE | Deleted |
| CLIENT_DRIVER | Deleted |
| LISTENER_PORT | Deleted |



**Apply Parameters**

| Parameter Name | Remarks |
| --- | --- |
| CLIENT_DRIVER | Deleted |
| LISTENER_PORT | Deleted |



**LLOB Parameters**

| Parameter Name | Remarks |
| --- | --- |
| CLIENT_DRIVER | Deleted |
| LISTENER_PORT | Deleted |





## Deprecated Features

This is a feature deprecated in ProSync 4.5.

- The metatable containing the deprecated term (TOP) has been changed. **PRS_IMPORTED_ARCHIVE_LOG** Table deletion **PRS_INSTALL_TOP **Table name **PRS_INSTALL_INSTANCE**changed to **TOP_ID** Column name **INSTANCE_ID**changed to

##
