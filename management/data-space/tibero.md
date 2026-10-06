Query, create, modify, and delete the storage space (tablespaces and data files) of databases running in OwlDB. A single tablespace can contain multiple data files.

{% hint style="info" %}
**Note**

- When the database status is `Running`Only in this case are querying, modifying, and deleting tablespaces possible.
- When the DB engine is set to OpenSQL, the database management screen is displayed instead of this page.
{% endhint %}

<figure>
<img src="../../.gitbook/assets/image-b30dc34b.png" alt="">
<figcaption>Figure 1. Data Space - Tibero</figcaption>
</figure>

# Querying Tablespaces

1. **OwlDB Console Screen > Management > Tablespaces** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose tablespaces you want to query.
3. Query the list of tablespaces. You can filter the list by type. You can also search directly by name.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Name</td><td>Tablespace Name</td></tr><tr><td>Type</td><td>Tablespace Type<ul><li><strong>Permanent</strong>: Stores permanent data and is generally the most commonly used type</li><li><strong>Temporary</strong>: Stores temporary data; data is deleted when the session ends or the task is completed</li><li><strong>Undo</strong>: Stores modified data</li></ul></td></tr><tr><td>Status</td><td>Tablespace Status<ul><li><strong>ONLINE</strong>: A state in which it is normally connected to the database and available for use</li><li><strong>OFFLINE</strong>: A state in which the connection to the database is lost (objects stored in the tablespace cannot be accessed)</li></ul></td></tr><tr><td>Used/Total Size</td><td><ul><li><strong>Used Size</strong>: The total amount of space in use by the data files belonging to the tablespace</li><li><strong>Total Size</strong>: The total amount of space allocated to the data files belonging to the tablespace</li></ul></td></tr><tr><td>Total/Max Size</td><td><ul><li><strong>Total Size</strong>: The total amount of space allocated to the data files belonging to the tablespace</li><li><strong>Max Size</strong>: The total amount of the maximum space that can be allocated to the data files belonging to the tablespace</li></ul></td></tr><tr><td>Logging</td><td>Logging Status</td></tr><tr><td>Allocation Type</td><td>Extent Allocation Method<ul><li><strong>SYSTEM</strong>: A method that dynamically allocates extent size according to system requirements</li><li><strong>UNIFORM</strong>: A method that stores objects using Extents of the same size</li></ul></td></tr><tr><td>Next Extent</td><td>Next Allocated Extent Size</td></tr></tbody></table>

---

# Creating a Tablespace

1. **OwlDB Console Screen > Management > Tablespaces** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database in which to create the tablespace.
3. **Create** Click the button.

{% hint style="info" %}
**Note**

Based on a data block size of 8KB, even if UNIFORM SIZE is set smaller than 128KB, it is set to the minimum Extent size of 128KB.
{% endhint %}

---

# Modifying a Tablespace

1. **OwlDB Console Screen > Management > Tablespaces** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose tablespace you want to modify.
3. **Edit** Click the button.

---

# Deleting a Tablespace

1. **OwlDB Console Screen > Management > Tablespaces** Navigate to the menu.
2. **DB Alias** Click the dropdown button to select the database whose tablespace you want to delete.
3. **Delete** Click the button.

---

# Querying Data Files

1. '[Querying Tablespaces](#테이블스페이스-조회)' to select the database whose data files you want to query.
2. **Tablespace** Click the radio button to select the tablespace whose data files you want to query.
3. Query the list of data files. You can filter the list by whether auto-extend is enabled. You can also search directly by data file name.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>Tablespace Name</td><td>Tablespace Name</td></tr><tr><td>Name</td><td>Data File Name</td></tr><tr><td>Online Status</td><td>Data File Status<ul><li><strong>SYSOFF</strong>: System offline file</li><li><strong>SYSTEM</strong>: System online file</li><li><strong>OFFLINE</strong>: Offline state</li><li><strong>ONLINE</strong>: Online state</li><li><strong>RECOVER</strong>: A state that requires recovery</li><li><strong>AVAILABLE</strong>: An available state</li></ul></td></tr><tr><td>Used/Total Size</td><td><ul><li><strong>Used Size</strong>: The amount of space in use by the data file</li><li><strong>Total Size</strong>: The amount of space allocated to the data file</li></ul></td></tr><tr><td>Total/Max Size</td><td><ul><li><strong>Total Size</strong>: The amount of space allocated to the data file</li><li><strong>Max Size</strong>: The maximum amount of space that can be allocated to the data file</li></ul></td></tr><tr><td>Auto Extend</td><td>Auto-Extend Status</td></tr><tr><td>Next</td><td>Next extent size</td></tr><tr><td>Physical Reads</td><td>Number of physical reads</td></tr><tr><td>Physical Writes</td><td>Number of physical writes</td></tr><tr><td>Single Block Reads</td><td>Number of single block reads</td></tr></tbody></table>

---

# Create data file

1. '[Querying Data Files](#데이터-파일-조회)' to select the tablespace in which to create the data file.
2. **Create** Click the button.

---

# Modify data file

1. '[Querying Data Files](#데이터-파일-조회)' to select the tablespace in which to modify the data file.
2. **Edit** Click the button.

---

# Delete data file

1. '[Querying Data Files](#데이터-파일-조회)' to select the tablespace from which to delete the data file.
2. **Delete** Click the button.
