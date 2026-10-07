This describes the environment preparation procedure, from placing the deployment file of the DB Service to be registered through installing and starting owlagent.

# **1. List of required files** <a href="#required-files" id="required-files"></a>

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)

# **2. File Placement** <a href="#place-files" id="place-files"></a>

Decompress the DP binary into `$OPENSQL_HOME`.

```bash
# Extract the DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
```

Once preparation is complete, the `$OPENSQL_HOME` structure is as follows.

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*                   # OpenSQL binary
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# 3. Install owlagent <a href="#install-owlagent" id="install-owlagent"></a>

1. Decompress the agent binary.

```bash
tar -zxvf owlagent_dist_latest.tar.gz -C $OPENSQL_HOME
```

```bash
owlagent_dist_latest.tar.gz
└── owlagent_dist/
    ├── config.json.description
    ├── manifest
    ├── owlagent
    ├── owlagent.env
    ├── owlagent_start
    └── owlagent_stop
```

2. Enter the configuration values in owlagent.env.

<table><thead><tr><th>Item</th><th>Description</th><th>Input rules</th></tr></thead><tbody><tr><td>AGENT_TYPE*</td><td></td><td>Enter <code>pg</code></td></tr><tr><td>IP*</td><td>IP of the OwlDB CP</td><td></td></tr><tr><td>PORT*</td><td>Port of the OwlDB CP</td><td></td></tr><tr><td>USERNAME*</td><td>Name of the opensql execution user</td><td></td></tr><tr><td>OPENSQL_HOME</td><td></td><td><ul><li>No input required if already configured</li><li>If not configured, enter the OPENSQL_HOME used above</li></ul></td></tr><tr><td>DB_LOG_DIR</td><td>PG log path</td><td>No input required if logs are not collected</td></tr><tr><td>DB_LOG_FILE_GLOB</td><td>PG log file format</td><td>Example: <code>postgresql*.log</code></td></tr><tr><td>PATRONI_CONFIG</td><td>patroni.yml path</td><td></td></tr><tr><td>PATRONI_MEMBER</td><td>patroni member name</td><td></td></tr></tbody></table>

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

It is acceptable for the log file not to exist in the `DB_LOG_DIR` path at the time of DB Service registration. Once the log file is created after registration, you can view it from the [Syslog](../../../monitoring/log/syslog.md) menu.
{% endhint %}

1. Run owlagent

```bash
sh owlagent_start.sh
```

The `owlagent_start` script registers the Agent as a systemd service and timer, and sudo privileges are used during this process.

# 4. Check the patroni service name <a href="#check-patroni-service-name" id="check-patroni-service-name"></a>

In an environment where Patroni is started with systemd, you control Patroni in the form of `systemctl start|stop|status patroni`. If the unit name is not `patroni.service`, start/stop control and status collection will not work, so you must verify the unit name in advance.

a. Check whether started with systemd

```bash
systemctl is-active --quiet patroni; echo $?
```

<table><thead><tr><th>Result</th><th>Determination</th></tr></thead><tbody><tr><td>0</td><td>Registered and started as <code>patroni.service</code> → check complete</td></tr><tr><td>Other than 0</td><td>The following two cases cannot be distinguished, so step b must also be performed<ul><li>When started with a different unit name</li><li>When not registered with systemd</li></ul></td></tr></tbody></table>

b. Verify the unit name

```bash
# 1) Check the Patroni process PID
pgrep -af patroni

# 2) Check the unit managing that PID (replace PID with the result value from 1)
cat /proc/<PID>/cgroup
```

| Result | Determination |
| --- | --- |
| 0::/system.slice/xxxxxxxx.service | Unit name mismatch → step c must also be performed |
| 0::/user.slice/user-1000.slice/session-3.scope | Not registered with systemd → no additional action needed<br>Stop/start handled inside owldb |

c. How to change the service name

Add an Alias to the `[Install]` section of the existing unit file, then re-register.

```bash
[Install]
WantedBy=multi-user.target
Alias=patroni.service
```

d. Re-register (daemon-reload·reenable)

After adding the Alias, re-register the service with daemon-reload and reenable.

```bash
systemctl daemon-reload
systemctl reenable <existing service name>
systemctl is-active --quiet patroni; echo $?   # verify 0
```
