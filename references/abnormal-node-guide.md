During the database discovery process, there may be cases where the relationships between some nodes cannot be accurately identified. In this case, perform manual configuration using the method below.

1. Move to the Agent installation path of all related DB nodes.
2. Create the `db_scan.info` file on each node.
3. Enter the same TSC ID in the file.

```properties
tsc_id={unique numeric value}
```

For example, in the case of a TSC cluster consisting of 4 nodes, configure it as follows.

```
Node 1's Agent path/db_scan.info → tsc_id=262
Node 2's Agent path/db_scan.info → tsc_id=262
Node 3's Agent path/db_scan.info → tsc_id=262
Node 4's Agent path/db_scan.info → tsc_id=262
```

{% hint style="info" %}
**Note**

- The TSC ID must use a unique value.
- All nodes belonging to the same DB configuration must use the same TSC ID.
- If you retry the DB scan after completing the configuration, the cluster configuration is recognized normally.
{% endhint %}
