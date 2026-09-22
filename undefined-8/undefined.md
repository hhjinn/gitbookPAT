During the database discovery process, there may be cases where the association between some nodes cannot be accurately identified. In this case, perform manual configuration using the method below.

1. Navigate to the Agent installation path of all associated DB nodes.
2. On each node, `db_scan.info` create the file.
3. Enter the same TSC ID in the file.

```properties
tsc_id={unique numeric value}
```

For example, for a TSC cluster consisting of 4 nodes, configure as follows.

```
Node 1's Agent path/db_scan.info → tsc_id=262
Node 2's Agent path/db_scan.info → tsc_id=262
Node 3's Agent path/db_scan.info → tsc_id=262
Node 4's Agent path/db_scan.info → tsc_id=262
```

{% hint style="info" %}
**Note**

- The TSC ID must be a unique value.
- All nodes belonging to the same DB configuration must use the same TSC ID.
- After completing the configuration, retry the DB scan and the cluster configuration will be recognized normally.
{% endhint %}
