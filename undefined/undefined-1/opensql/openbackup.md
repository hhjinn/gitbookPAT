This page describes the deployment file placement and owlagent installation procedure for preparing the OpenBackup server environment.

## 1. Install owlagent

1. Extract the agent binary.

```bash
tar -zxvf owlagent_dist_latest.tar.gz
```

```
owlagent_dist_latest.tar.gz
└── owlagent_dist/
    ├── config.json.description
    ├── manifest
    ├── owlagent
    ├── owlagent.env
    ├── owlagent_start
    └── owlagent_stop
```

2. `owlagent.env`Enter the configuration values in.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th><th>Input Rules</th></tr></thead><tbody><tr><td><code>AGENT_TYPE</code></td><td>barman</td><td></td></tr><tr><td><code>IP</code></td><td>IP of the OwlDB CP</td><td></td></tr><tr><td><code>PORT</code></td><td>port of the OwlDB CP</td><td></td></tr><tr><td><code>USERNAME</code></td><td>Name of the user that runs opensql</td><td></td></tr><tr><td><code>OPENSQL_HOME</code></td><td></td><td><ul><li><a href="#h-2-파일-배치">2. File Placement</a>Leave blank if the environment variable has already been set in the step</li><li>If the environment variable is not set, the one used above<code>OPENSQL_HOME</code> Input</li></ul></td></tr><tr><td><code>BARMAN_NAME</code></td><td>Name of the barman server</td><td></td></tr><tr><td><code>BARMAN_SSH_USER</code></td><td>SSH access account</td><td></td></tr><tr><td><code>BARMAN_SSH_KEYPATH</code></td><td>Path to the private key file used for SSH access</td><td></td></tr><tr><td><code>BARMAN_SSH_IP</code></td><td>IP address for SSH access to the barman host</td><td></td></tr><tr><td><code>BARMAN_SSH_PORT</code></td><td>Port number for SSH access to the barman host</td><td></td></tr><tr><td><code>BARMAN_CONF_DIR</code></td><td>Path to the conf directory of the barman host<br>e.g.)<code>/etc/barman.d/</code></td><td></td></tr></tbody></table>

3. Run owlagent.

```bash
sh owlagent_start.sh
```

{% hint style="info" %}
**Note**

`owlagent_start`registers the Agent as a systemd service and timer, and sudo privileges are used during this process.
{% endhint %}
