This page explains how to install and start the Agent on a database server.

{% hint style="info" %}
**Note**

This guide [Installation DB Environment Preparation Guide](db.md) or [Registration DB Environment Preparation Guide](db-1.md)should be followed after completing.
{% endhint %}

### Agent Installation and Startup

**1. Extract the Agent binary**

```bash
tar -zxvf $TB_HOME/tbagent_dist_latest.tar.gz -C $TB_HOME
```

**2. Create the agent.env file**

Enter the information required to start the agent into the env file.

```bash
############################################################
#                      AGENT ENV FILE                      #
#----------------------------------------------------------#
# This file contains environment variables for the agent  #
# Do NOT add spaces around '='                            #
# Lines starting with '#' are comments                    #
############################################################

# ------------------------------
# Network Configuration (OwlDB)
# ------------------------------
IP=
PORT=

# ------------------------------
# User Configuration
# ------------------------------
USERNAME=

# ------------------------------
# Tibero Configuration
# ------------------------------
TB_HOME=

# ------------------------------
# Only Register Variables [OPTIONAL]
# ------------------------------
TB_SID=
TAS_SID=
CM_SID=
CM_HOME=

############################################################
# End of agent.env
############################################################
```

Whether a parameter needs to be entered varies depending on the configuration method.

{% tabs %}
{% tab title="Installation DB" %}
| Option | Description | Required |
| --- | --- | --- |
| `IP` | OwlDB server IP address | Required |
| `PORT` | OwlDB server port (SERVER_PORT) | Required |
| `USERNAME` | DB OS user name | Required |
| `TB_HOME` | Tibero home directory path | Required |
| `TB_SID` | TB_SID value | Not required during installation |
| `TAS_SID` | TAS_SID value | Not required during installation |
| `CM_SID` | CM_SID value | Not required during installation |
| `CM_HOME` | CM_HOME value | Not required during installation |

{% hint style="info" %}
**Note**

`TB_SID` DB identification values such as these are automatically set by OwlDB during the installation process. They do not need to be entered in the pre-installation stage.
{% endhint %}
{% endtab %}
{% tab title="Registration DB" %}
<table data-full-width="true"><thead><tr><th>Option</th><th>Description</th><th>Required</th></tr></thead><tbody><tr><td><code>IP</code></td><td>OwlDB server IP address</td><td>Required</td></tr><tr><td><code>PORT</code></td><td>OwlDB server port (SERVER_PORT)</td><td>Required</td></tr><tr><td><code>USERNAME</code></td><td>OS user name that installed Tibero</td><td>Required</td></tr><tr><td><code>TB_HOME</code></td><td>Tibero home directory path</td><td>Required</td></tr><tr><td><code>TB_SID</code></td><td>TB_SID value</td><td><strong>Required</strong></td></tr><tr><td><code>TAS_SID</code></td><td>TAS_SID value</td><td><ul><li>Required when configuring TAC</li><li>Not required when not in use</li></ul></td></tr><tr><td><code>CM_SID</code></td><td>CM_SID value</td><td><ul><li>Required when configuring CM</li><li>Not required when not in use</li></ul></td></tr><tr><td><code>CM_HOME</code></td><td>CM_HOME value</td><td><ul><li>Required when configuring CM</li><li>Not required when not in use</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**Caution**

Since the Registration DB targets an existing database that is already in operation, `TB_SID`must be set to the SID value of the existing Tibero instance.
{% endhint %}
{% endtab %}
{% endtabs %}

**3. Run the Agent installation script**

```bash
cd $TB_HOME/tbagent_dist
sh tbagent_start.sh
```

{% hint style="warning" %}
**Caution**

`tbagent_start.sh` The script registers the Agent as a systemd service and timer, and sudo privileges are used during this process.

When restarting the Agent [Reference Materials > Precautions when restarting the Agent](#nql9NMu6dh160KsnoVKa)be sure to check.
{% endhint %}

**4. Configure OS user sudoers**

Some commands are executed with sudo during the database installation and operation process. Since the task may be interrupted if a password is requested during script execution, set NOPASSWD for the OS user used for installation.

```bash
# 1. Create the sudoers configuration file
sudo tee /etc/sudoers.d/{username} << 'EOF'
{username} ALL=(ALL) NOPASSWD: \
        /usr/bin/systemctl \
        /usr/bin/timedatectl \
        /usr/sbin/udevadm \
        /usr/bin/dd \
        /usr/bin/tar \
        /usr/bin/mkdir \
        /usr/bin/rm \
        /usr/bin/unlink \
        /usr/bin/ln \
        /usr/bin/chown \
        /usr/bin/chmod \
        /usr/bin/tee \
        /usr/bin/sed \
        /usr/bin/bash \
        /usr/bin/su \
        /usr/lib/udev/scsi_id \
        /usr/bin/grep \
        /usr/bin/touch \
        /usr/bin/kill \
    )
EOF

# 2. Set file permissions
sudo chmod 440 /etc/sudoers.d/{username}

# 3. Verify application
sudo -l -U {username}
```
