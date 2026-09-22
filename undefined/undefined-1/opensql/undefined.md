This is the common preparation procedure for the database servers managed by OwlDB. For procedures that differ by configuration method, such as disk requirements, network settings, and deployment file composition, refer respectively to [Install DB Environment Preparation Guide](db.md)and [Registration DB Environment Preparation Guide](db-1.md).

## OS User / SSH Key Configuration

{% hint style="info" %}
**Note**

This content is **when using a DR configuration**only required.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this, a dedicated OS user is created on each node, and a shared key pair is distributed to enable passwordless SSH access.

The example values used in the procedures below are as follows. Please substitute them to match your actual environment.

- DB OS user: `opensql`
- SSH port: `22`
- Database node Node1 (Leader): `10.10.0.11` Node2 (Replica): `10.10.0.12` Node3 (Replica): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Create a Dedicated DB OS User

On all nodes, **the same UID/GID**create the user with.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash opensql
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched, and the DB process will not be able to read the files. Create the user by explicitly specifying the same numeric values.
{% endhint %}

### 2. Create an SSH Key Pair (performed only once on Node 1)

On Node1 (`10.10.0.11`), generate the key pair only once. **This key pair becomes the shared key for all nodes.**

```bash
# Run as the opensql account on node1
su - opensql
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

The private key must also be distributed to all nodes. Since every node must perform SCP to every other node (node1 → {node2, node3}, node2 → {node1, node3}, …), **every node also acts as an SSH client.** For the SSH client to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configure authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run as the opensql account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distribute the Key Set

On each Replica node, fetch node1's key files via SCP.

```bash
# Run as the opensql account on each Replica node (node2, node3)
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # private key
scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # public key

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

After distribution is complete, on all nodes `~/.ssh/` check the permission status.

```bash
drwx------   opensql:dba  ~/.ssh
-rw-------   opensql:dba  ~/.ssh/id_ed25519
-rw-r--r--   opensql:dba  ~/.ssh/id_ed25519.pub
-rw-------   opensql:dba  ~/.ssh/authorized_keys
```

### 5. Pre-register known_hosts

To prevent a host key prompt from occurring when running the automation script, collect the host public keys of all cluster nodes in advance on each node.

```bash
# Run as the opensql account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Harden the sshd Configuration

Apply the sshd settings below to strengthen security.

For the SSH shared key authentication in this manual to work correctly, **required items**and, for security hardening, **recommended items**are organized together. Please apply them identically on all nodes.

**1) Configuration Items**

| Item | Recommended Value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allows public-key-based authentication. This is the authentication method used in this manual, so it must be enabled. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | Path to the per-user public key list file. This is the sshd default; if changed, the file location used in steps 3 and 4 must also be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Validates the permissions of the user home directory and key files. If permissions are too loose, authentication is denied. This is why the permission values specified in step 4 must be observed. |
| `PermitRootLogin` | `no` | Recommended | Reduces the attack surface by blocking SSH access for the root account. |
| `PasswordAuthentication` | `no` | Recommended | Blocks password-based authentication to allow only public-key authentication. Blocks brute-force attacks. |
| `AllowUsers` | `opensql` | Recommended | Allows SSH access only for the dedicated DB OS user, blocking access from other accounts. |

**2) How to Apply (if needed)**

`/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/` After editing the relevant items under it, reload sshd to apply the changes.

```bash
# /etc/ssh/sshd_config recommended configuration example
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers opensql

# reload configuration (re-reads the configuration only, without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — reload succeeded if Active: active (running)
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing the sshd configuration from a remote SSH session, if sshd fails to reload due to an invalid configuration, additional SSH connections may be refused. When performing this work, proceed only after separately securing a means of console access.

Immediately after applying changes, always `systemctl status` We recommend verifying that sshd is operating normally using the command. (`Active: failed` or if an error message is displayed, you must immediately revert the configuration)
{% endhint %}

### 7. Source IP restriction

`authorized_keys` In front of the entry `from=` By granting the option, you restrict the key to be valid only within the cluster's internal network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only by a DB-dedicated OS user (`opensql`) only, and **root** It is never shared as the SSH key of the root account. Organize the ownership of the data directory in advance so that recovery work and SCP do not require root privileges.
{% endhint %}
