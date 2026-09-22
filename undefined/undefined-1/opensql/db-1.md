This section explains the environment preparation procedure, from placing the deployment file of the DB Service to be registered, up to installing and starting owlagent.

# **1. List of Required Files**

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)

# **2. File Placement**

Extract the DP binary `$OPENSQL_HOME`to.

```bash
# Extract the DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
```

After preparation is complete, the `$OPENSQL_HOME`structure is as follows

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*                   # OpenSQL binary
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# 3. owlagent Installation

1. Extract the agent binary.

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

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input Rule</th></tr></thead><tbody><tr><td>AGENT_TYPE*</td><td></td><td><code>pg</code> Enter</td></tr><tr><td>IP*</td><td>IP of OwlDB CP</td><td></td></tr><tr><td>PORT*</td><td>port of OwlDB CP</td><td></td></tr><tr><td>USERNAME*</td><td>Name of the opensql execution user</td><td></td></tr><tr><td>OPENSQL_HOME</td><td></td><td><ul><li>No entry required if already set</li><li>If not set, enter the OPENSQL_HOME used above</li></ul></td></tr><tr><td>DB_LOG_DIR</td><td>PG log path</td><td>No entry required if logs are not collected</td></tr><tr><td>DB_LOG_FILE_GLOB</td><td>PG log file format</td><td>Example: <code>postgresql*.log</code></td></tr><tr><td>PATRONI_CONFIG</td><td>patroni.yml path</td><td></td></tr><tr><td>PATRONI_MEMBER</td><td>patroni member name</td><td></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

At the time of DB Service registration, `DB_LOG_DIR` it is fine even if no log file exists in the path. Once a log file is created after registration, it can be viewed from the [Syslog](../../../undefined-5/undefined-2/syslog.md) menu.
{% endhint %}

1. owlagent Execution

```bash
sh owlagent_start.sh
```

`owlagent_start` The script registers the Agent as a systemd service and timer, and sudo privileges are used in this process.

# 4. Checking the patroni Service Name

In an environment where Patroni is started with systemd, `systemctl start|stop|status patroni` Patroni is controlled in the form of. If the unit name is not `patroni.service`, start/stop control and status collection will not work, so the unit name must be checked in advance.

a. Checking whether systemd is started

```bash
systemctl is-active --quiet patroni; echo $?
```

<table data-full-width="true"><thead><tr><th>Result</th><th>Determination</th></tr></thead><tbody><tr><td>0</td><td><code>patroni.service</code>Registered and started → check complete</td></tr><tr><td>Other than 0</td><td>The following two cases cannot be distinguished, so step b must be additionally performed<ul><li>When started with a different unit name</li><li>When not registered with systemd</li></ul></td></tr></tbody></table>

b. Checking the Unit Name

```bash
# 1) Check the Patroni process PID
pgrep -af patroni

# 2) Check the unit managing that PID (replace PID with the result value from step 1)
cat /proc/<PID>/cgroup
```

| Result | Determination |
| --- | --- |
| 0::/system.slice/xxxxxxxx.service | Unit name mismatch → step c must be additionally performed |
| 0::/user.slice/user-1000.slice/session-3.scope | Not registered with systemd → no additional action required<br>Stop/start is handled inside owldb |

c. How to Change the Service Name

Add an Alias to the `[Install]` section of the existing unit file, then re-register.

```bash
[Install]
WantedBy=multi-user.target
Alias=patroni.service
```

d. Re-registration (daemon-reload·reenable)

After adding the Alias, re-register the service with daemon-reload and reenable.

```bash
systemctl daemon-reload
systemctl reenable <existing-service-name>
systemctl is-active --quiet patroni; echo $?   # confirm 0
```
