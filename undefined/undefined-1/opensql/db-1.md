This document explains the environment preparation procedure, from placing the deployment files for the DB Service to be registered, to installing and starting owlagent.

# **1. List of Required Files**

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)

# **2. File Placement**

The DP binary `$OPENSQL_HOME`Extract it to.

```bash
# Extract the DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
```

After preparation is complete, `$OPENSQL_HOME`The structure is as follows

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*                   # OpenSQL binary
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# 3. Installing owlagent

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

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input rule</th></tr></thead><tbody><tr><td>AGENT_TYPE*</td><td></td><td><code>pg</code> Input</td></tr><tr><td>IP*</td><td>IP of OwlDB CP</td><td></td></tr><tr><td>PORT*</td><td>port of OwlDB CP</td><td></td></tr><tr><td>USERNAME*</td><td>opensql execution user name</td><td></td></tr><tr><td>OPENSQL_HOME</td><td></td><td><ul><li>No input required if already configured</li><li>If not configured, enter the OPENSQL_HOME used above</li></ul></td></tr><tr><td>DB_LOG_DIR</td><td>PG log path</td><td>No input required if logs are not collected</td></tr><tr><td>DB_LOG_FILE_GLOB</td><td>PG log file format</td><td>e.g.: <code>postgresql*.log</code></td></tr><tr><td>PATRONI_CONFIG</td><td>patroni.yml path</td><td></td></tr><tr><td>PATRONI_MEMBER</td><td>patroni member name</td><td></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input item.

{% hint style="info" %}
**Note**

At the time of DB Service registration, `DB_LOG_DIR` It is acceptable that the log file does not exist in the path. Once the log file is created after registration, [Syslog](../../../undefined-5/undefined-2/syslog.md) View it from the menu.
{% endhint %}

1. Running owlagent

```bash
sh owlagent_start.sh
```

`owlagent_start` The script registers the Agent as a systemd service and timer, and sudo privileges are used in this process.

# 4. Checking the patroni service name

In an environment where Patroni is started with systemd, `systemctl start|stop|status patroni` Patroni is controlled in this form. If the unit name is `patroni.service`If it is not, start/stop control and status collection will not work, so the unit name must be verified in advance.

a. Checking whether systemd is started

```bash
systemctl is-active --quiet patroni; echo $?
```

<table data-full-width="true"><thead><tr><th>Result</th><th>Determination</th></tr></thead><tbody><tr><td>0</td><td><code>patroni.service</code>Registered and started with → check complete</td></tr><tr><td>Other than 0</td><td>The following two cases cannot be distinguished, so step b must be additionally performed<ul><li>When started with a different unit name</li><li>When not registered with systemd</li></ul></td></tr></tbody></table>

b. Checking the unit name

```bash
# 1) Check the Patroni process PID
pgrep -af patroni

# 2) Check the unit managing that PID (replace PID with the result value from step 1)
cat /proc/<PID>/cgroup
```

| Result | Determination |
| --- | --- |
| 0::/system.slice/xxxxxxxx.service | Unit name mismatch → step c must be additionally performed |
| 0::/user.slice/user-1000.slice/session-3.scope | Not registered with systemd → no additional action required<br>Stop/start handling is done internally within owldb |

c. How to change the service name

Of the existing unit file `[Install]` After adding Alias to the section, re-register.

```bash
[Install]
WantedBy=multi-user.target
Alias=patroni.service
```

d. Re-registration (daemon-reload·reenable)

After adding Alias, re-register the service with daemon-reload and reenable.

```bash
systemctl daemon-reload
systemctl reenable <existing service name>
systemctl is-active --quiet patroni; echo $?   # confirm 0
```
