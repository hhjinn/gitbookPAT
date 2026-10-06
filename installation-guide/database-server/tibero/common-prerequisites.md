This is the common preparation procedure for the database servers monitored by OwlDB. For procedures that differ by configuration method, such as disk requirements, network settings, and deployment file composition, refer respectively to [Installation DB Environment Preparation Guide](#W2TxdEHwoC3mStsQfJdo)and [Registration DB Environment Preparation Guide](#pvI81bWJlNtie4nv4stI).

## Timezone configuration <a href="#timezone-settings" id="timezone-settings"></a>

Configure the database server's Timezone correctly. For multi-node configurations such as TAC and DR, **the Timezone of all nodes must be identical**. If the Timezone differs, data consistency problems and log time mismatches may occur.

```bash
# Check current Timezone
timedatectl

# Set Timezone (e.g., Asia/Seoul)
sudo timedatectl set-timezone Asia/Seoul

# Verify the configuration
timedatectl
```

## OS user / SSH key configuration <a href="#os-user-ssh-key" id="os-user-ssh-key"></a>

{% hint style="info" %}
**Note**

This content **If a DR configuration is used**is required only.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this purpose, a dedicated OS user is created on each node, and a shared key pair is distributed so that SSH access is possible without a password.

The example values used in the following procedure are as follows. Please substitute them to match your actual environment.

- DB OS user: `tibero`
- SSH port: `22`
- Database node Node1 (Primary): `10.10.0.11` Node2 (Standby): `10.10.0.12` Node3 (Standby): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Creating a dedicated DB OS user

On all nodes, **the same UID/GID**create the user with.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash tibero
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched, and the DB process will not be able to read the files. Create the user by explicitly specifying the same numeric values.
{% endhint %}

### 2. Creating the SSH key pair (performed only once, on Node 1)

Node1(`10.10.0.11`) generate the key pair only once. **This key pair becomes the shared key for all nodes.**

```bash
# Run as the tibero account on node1
su - tibero
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519 -C "owldb-shared-key"
```

| option | Description |
| --- | --- |
| `-t ed25519` | Recommended algorithm. When using RSA, `-t rsa -b 4096` |
| `-N ""` | No passphrase for automated SCP |
| `-C` | Comment for key identification |

{% hint style="warning" %}
**Caution**

The private key must also be distributed to all nodes. Since every node must perform SCP to each other (node1 → {node2, node3}, node2 → {node1, node3}, …), **every node also acts as an SSH client.** For the SSH client side to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configuring authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run as the tibero account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distributing the key set <a href="#id-4" id="id-4"></a>

On each Standby node, fetch node1's key files via SCP.

```bash
# Run as the tibero account on each Standby node (node2, node3)
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 tibero@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # private key
scp -P 22 tibero@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # public key

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

After distribution is complete, on all nodes `~/.ssh/` verify the permission status.

```bash
drwx------   tibero:dba  ~/.ssh
-rw-------   tibero:dba  ~/.ssh/id_ed25519
-rw-r--r--   tibero:dba  ~/.ssh/id_ed25519.pub
-rw-------   tibero:dba  ~/.ssh/authorized_keys
```

### 5. Pre-registering known_hosts

To prevent a host key prompt from occurring when the automation script runs, collect the host public keys of all nodes in the cluster in advance on each node.

```bash
# Run as the tibero account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Hardening the sshd configuration

Apply the sshd settings below to strengthen security.

In order for the SSH shared key authentication in this manual to function correctly, **required items**and, for security hardening, **recommended items**are organized together. Please apply them identically on all nodes.

**1) Configuration items**

| Item | Recommended value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allow public key-based authentication. This is the authentication method used in this manual, so it must be enabled. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | Path to the per-user public key list file. This is the sshd default; if changed, the file location used in steps 3 and 4 must be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Verification of user home directory and key file permissions. If permissions are too loose, authentication is denied. This is why compliance with the permission values specified in step 4 is required. |
| `PermitRootLogin` | `no` | Recommended | Reduce the attack surface by blocking SSH access for the root account. |
| `PasswordAuthentication` | `no` | Recommended | Allow only public key authentication by blocking password-based authentication. Blocks brute-force attacks. |
| `AllowUsers` | `tibero` | Recommended | Allow SSH access only for the DB-dedicated OS user, blocking access from other accounts. |

**2) How to apply (if necessary)**

`/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/` Edit the relevant item under it, then reload sshd to apply.

```bash
# Example of recommended /etc/ssh/sshd_config settings
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers tibero
```

```bash
# Reload configuration (re-reads only the configuration without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — if Active: active (running), the reload succeeded
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing sshd settings from a remote SSH session, if sshd fails to reload due to incorrect settings, further SSH connections may be denied. When performing this work, proceed with a separate console access method secured in advance.

Immediately after applying changes, always `systemctl status` We recommend verifying that sshd is operating normally with the command. (`Active: failed` or if an error message is output, you must revert the settings immediately)
{% endhint %}

### 7. Source IP restriction

`authorized_keys` In front of the item `from=` By granting the option, restrict the key to be valid only within the cluster's internal network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only by the DB-dedicated OS user (`tibero`), and **root** It must never be shared as the SSH key for the account. Organize the data directory ownership in advance so that recovery operations and SCP do not require root privileges.
{% endhint %}
