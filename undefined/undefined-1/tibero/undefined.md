This is a common preparation procedure applicable to database servers monitored by OwlDB. For other procedures that differ by configuration method — such as disk requirements, network settings, and deployment file configuration — refer to [Installation DB Environment Preparation Guide](#W2TxdEHwoC3mStsQfJdo)and [Registration DB Environment Preparation Guide](#pvI81bWJlNtie4nv4stI)respectively.

## Timezone Configuration

Configure the database server's Timezone correctly. For multi-node configurations such as TAC and DR, **the Timezone of all nodes must be identical**. If the Timezone differs, data consistency issues and log time mismatches may occur.

```bash
# Check current Timezone
timedatectl

# Set Timezone (e.g., Asia/Seoul)
sudo timedatectl set-timezone Asia/Seoul

# Verify configuration
timedatectl
```

## OS User / SSH Key Configuration

{% hint style="info" %}
**Note**

This content is **only required when using a DR configuration**.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this purpose, create a dedicated OS user on each node and distribute a shared key pair to enable password-less SSH access.

The example values used in the following procedures are as shown below. Please substitute them to match your actual environment.

- DB OS user: `tibero`
- SSH port: `22`
- Database node Node1 (Primary): `10.10.0.11` Node2 (Standby): `10.10.0.12` Node3 (Standby): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Create Dedicated DB OS User

On all nodes, **the same UID/GID**create the user with.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash tibero
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched, and the DB process will be unable to read the files. Create the user by explicitly specifying the same numbers.
{% endhint %}

### 2. Create SSH Key Pair (performed only once on Node 1)

On Node1 (`10.10.0.11`), create the key pair only once. **This key pair becomes the shared key for all nodes.**

```bash
# Run as the tibero account on node1
su - tibero
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519 -C "owldb-shared-key"
```

| Option | Description |
| --- | --- |
| `-t ed25519` | Recommended algorithm. When using RSA, `-t rsa -b 4096` |
| `-N ""` | No passphrase for automated SCP |
| `-C` | Comment for key identification |

{% hint style="warning" %}
**Caution**

The private key must also be distributed to all nodes. Since all nodes must perform SCP to each other (node1 → {node2, node3}, node2 → {node1, node3}, …), **all nodes also serve as SSH clients.** For the SSH client to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configure authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run as the tibero account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distribute Key Set

On each Standby node, retrieve node1's key files via SCP.

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

### 5. Pre-register known_hosts

To prevent a host key prompt from occurring when running automation scripts, collect the host public keys of all cluster nodes in advance on each node.

```bash
# Run as the tibero account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Harden sshd Configuration

Apply the following sshd settings to strengthen security.

For the SSH shared key authentication in this manual to work correctly, **Required items**and, for enhanced security, **Recommended items**are organized together. Please apply them identically on all nodes.

**1) Configuration Items**

| Item | Recommended Value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allows public key-based authentication. Must be enabled as it is the authentication method used in this manual. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | File path for the per-user list of public keys. This is the sshd default; if changed, the file locations used in steps 3 and 4 must also be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Verifies permissions on the user's home directory and key files. Authentication is denied if permissions are too loose. This is the reason the permission values specified in step 4 must be observed. |
| `PermitRootLogin` | `no` | Recommended | Reduces the attack surface by blocking SSH access via the root account. |
| `PasswordAuthentication` | `no` | Recommended | Blocks password-based authentication and allows only public key authentication. Blocks brute-force attacks. |
| `AllowUsers` | `tibero` | Recommended | Allows SSH access only for the dedicated DB OS user, blocking access from other accounts. |

**2) Application Method (if needed)**

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
# Reload configuration (re-reads the configuration only, without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — reload succeeded if Active: active (running)
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing sshd settings from a remote SSH session, if sshd fails to reload due to an incorrect configuration, additional SSH connections may be refused. When performing this work, proceed only after separately securing a means of console access.

Immediately after applying the change, always `systemctl status` we recommend verifying that sshd is operating normally using the command. (`Active: failed` or if an error message appears, revert the settings immediately)
{% endhint %}

### 7. Source IP Restriction

`authorized_keys` In front of the item `from=` Grant the option to restrict the key to be valid only within the cluster internal network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only with the DB-dedicated OS user (`tibero`), and **root** It is never shared as the SSH key of the root account. Organize the ownership of the data directory in advance so that recovery operations and SCP do not require root privileges.
{% endhint %}
