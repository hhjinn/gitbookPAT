This is the common preparation procedure for database servers managed by OwlDB. For other procedures that differ depending on the configuration method, such as disk requirements, network settings, and deployment file configuration, refer to the [Installation DB Environment Preparation Guide](install-db-prerequisites.md) and the [Registration DB Environment Preparation Guide](register-db-prerequisites.md) respectively.

## OS User / SSH Key Configuration <a href="#os-user-ssh-key" id="os-user-ssh-key"></a>

{% hint style="info" %}
**Note**

This content is only required **when using a DR configuration**.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this, a dedicated OS user is created on each node, and a shared key pair is distributed to enable SSH access without a password.

The example values used in the procedure below are as follows. Please substitute them to match your actual environment.

- DB OS user: `opensql`
- SSH port: `22`
- Database nodes Node1 (Leader): `10.10.0.11` Node2 (Replica): `10.10.0.12` Node3 (Replica): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Create a Dedicated DB OS User

Create the user with **the same UID/GID** on all nodes.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash opensql
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched and the DB process will not be able to read the files. Create the user by explicitly specifying the same numbers.
{% endhint %}

### 2. Create an SSH Key Pair (Performed Only Once on Node 1)

Create the key pair just once on Node1 (`10.10.0.11`). **This key pair becomes the shared key for all nodes.**

```bash
# Run with the opensql account on node1
su - opensql
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519 -C "owldb-shared-key"
```

| Option | Description |
| --- | --- |
| `-t ed25519` | Recommended algorithm. When using RSA, `-t rsa -b 4096` |
| `-N ""` | No passphrase, for automated SCP |
| `-C` | Comment for key identification |

{% hint style="warning" %}
**Caution**

The private key must also be distributed to all nodes. Since every node must perform SCP to each other (node1 → {node2, node3}, node2 → {node1, node3}, …), **every node also acts as an SSH client.** For the SSH client side to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configure authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run with the opensql account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distribute the Key Set <a href="#id-4" id="id-4"></a>

From each Replica node, fetch node1's key file via SCP.

```bash
# Run with the opensql account on each Replica node (node2, node3)
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # private key
scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # public key

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

After distribution is complete, check the permission status of `~/.ssh/` on all nodes.

```bash
drwx------   opensql:dba  ~/.ssh
-rw-------   opensql:dba  ~/.ssh/id_ed25519
-rw-r--r--   opensql:dba  ~/.ssh/id_ed25519.pub
-rw-------   opensql:dba  ~/.ssh/authorized_keys
```

### 5. Pre-register known_hosts

To prevent host key prompts from occurring when running the automation script, collect the host public keys of all nodes in the cluster in advance on each node.

```bash
# Run with the opensql account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Harden sshd Configuration

Apply the sshd settings below to strengthen security.

This summarizes both the **required items** for the SSH shared key authentication in this manual to work properly and the **recommended items** for strengthening security. Please apply them identically on all nodes.

**1) Configuration Items**

| Item | Recommended value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allow public key-based authentication. This must be enabled as it is the authentication method of this manual. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | Path of the per-user public key list file. This is the sshd default; if changed, the file locations used in steps 3 and 4 must also be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Verification of user home directory and key file permissions. If permissions are loose, authentication is denied. This is the reason why compliance with the permission values specified in step 4 is required. |
| `PermitRootLogin` | `no` | Recommended | Reduce the attack surface by blocking SSH access for the root account. |
| `PasswordAuthentication` | `no` | Recommended | Allow only public key authentication by blocking password-based authentication. Blocks brute-force attacks. |
| `AllowUsers` | `opensql` | Recommended | Allow SSH access only for the dedicated DB OS user, blocking access by other accounts. |

**2) How to Apply (If Needed)**

Edit the relevant item under `/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/` and then reload sshd to apply it.

```bash
# Example of recommended /etc/ssh/sshd_config settings
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers opensql

# Reload configuration (re-reads only the settings without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — if Active: active (running), the reload was successful
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing sshd settings from a remote SSH session, if sshd fails to reload due to incorrect settings, additional SSH connections may be denied. When performing this work, please proceed with a separate console access method secured in advance.

Immediately after applying changes, it is recommended to always verify that sshd is operating normally with the `systemctl status` command. (If `Active: failed` or an error message is output, you must revert the settings immediately.)
{% endhint %}

### 7. Restrict Source IP

By adding the `from=` option in front of the `authorized_keys` entry, restrict the key so that it is valid only within the cluster internal network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only with the DB-dedicated OS user (`opensql`), and must never be shared as the SSH key of the **root** account. Organize the data directory ownership in advance so that recovery tasks and SCP do not require root privileges.
{% endhint %}
