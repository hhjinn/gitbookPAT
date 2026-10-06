Describes the environment preparation procedure, from placing the deployment files of the DB Service to be registered to installing and starting owlagent.

# **1. List of required files** <a href="#required-files" id="required-files"></a>

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)

# **2. File Placement** <a href="#place-files" id="place-files"></a>

Decompress the DP binary `$OPENSQL_HOME`Extract to

```bash
# Decompress the DP binary
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

# 3. Install owlagent <a href="#install-owlagent" id="install-owlagent"></a>

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

<table><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td>AGENT_TYPE*</td><td></td><td><code>pg</code> Input</td></tr><tr><td>IP*</td><td>IP of the OwlDB CP</td><td></td></tr><tr><td>PORT*</td><td>port of the OwlDB CP</td><td></td></tr><tr><td>USERNAME*</td><td>opensql execution user name</td><td></td></tr><tr><td>OPENSQL_HOME</td><td></td><td><ul><li>No input required if already configured</li><li>If not configured, enter the OPENSQL_HOME used above</li></ul></td></tr><tr><td>DB_LOG_DIR</td><td>PG log path</td><td>No input required if logs are not collected</td></tr><tr><td>DB_LOG_FILE_GLOB</td><td>PG log file format</td><td>e.g.: <code>postgresql*.log</code></td></tr><tr><td>PATRONI_CONFIG</td><td>patroni.yml path</td><td></td></tr><tr><td>PATRONI_MEMBER</td><td>patroni member name</td><td></td></tr></tbody></table>

*표기는 필수 입력 항목을 의미합니다.

The * notation indicates a required input field.

{% hint style="info" %}
**Note**

At the time of DB Service registration `DB_LOG_DIR` It is acceptable even if no log file exists at the path. Once a log file is created after registration, [Syslog](../../../monitoring/log/syslog.md) Look it up from the menu.
{% endhint %}

1. Run owlagent

```bash
sh owlagent_start.sh
```

`owlagent_start` The script registers the Agent as a systemd service and timer, and sudo privileges are used during this process.

# 4. Check Patroni Service Name <a href="#check-patroni-service-name" id="check-patroni-service-name"></a>

In an environment where Patroni is started via systemd, `systemctl start|stop|status patroni` Patroni is controlled in this form. If the unit name `patroni.service`is not this, start/stop control and status collection will not work, so the unit name must be verified in advance.

a. Check systemd startup status

```bash
systemctl is-active --quiet patroni; echo $?
```

<table><thead><tr><th>Result</th><th>Determination</th></tr></thead><tbody><tr><td>0</td><td><code>patroni.service</code>Registered and started as → check complete</td></tr><tr><td>Other than 0</td><td>The following two cases cannot be distinguished, so step b must be additionally performed<ul><li>When started with a different unit name</li><li>When not registered in systemd</li></ul></td></tr></tbody></table>

b. Check unit name

```bash
# 1) Check Patroni process PID
pgrep -af patroni

# 2) Check the unit managing that PID (replace PID with the result value from step 1)
cat /proc/<PID>/cgroup
```

| Result | Determination |
| --- | --- |
| 0::/system.slice/xxxxxxxx.service | Unit name mismatch → step c must be additionally performed |
| 0::/user.slice/user-1000.slice/session-3.scope | Not registered in systemd → no additional action required<br>Handle stop/start processing inside owldb |

c. How to change the service name

Of the existing unit file `[Install]` Add an Alias to the section, then re-register.

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
systemctl is-active --quiet patroni; echo $?   # confirm 0
```
