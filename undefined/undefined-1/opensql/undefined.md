This is the common preparation procedure for database servers monitored by OwlDB. For procedures that differ by configuration method, such as disk requirements, network settings, and deployment file configuration, refer to [Installed DB Environment Preparation Guide](db.md)and [Registered DB Environment Preparation Guide](db-1.md)respectively.

## OS user / SSH key configuration

{% hint style="info" %}
**Note**

This content is **when using a DR configuration**only required.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this purpose, a dedicated OS user is created on each node, and a shared key pair is distributed so that SSH access is possible without a password.

The example values used in the procedure below are as follows. Please substitute them to match your actual environment.

- DB OS user: `opensql`
- SSH port: `22`
- Database node Node1 (Leader): `10.10.0.11` Node2 (Replica): `10.10.0.12` Node3 (Replica): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Create a dedicated DB OS user

On all nodes **with the same UID/GID**, create the user.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash opensql
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched and the DB process will not be able to read the files. Create the user by explicitly specifying the same numbers.
{% endhint %}

### 2. Create an SSH key pair (performed only once on Node 1)

Node1(`10.10.0.11`), generate the key pair only once. **This key pair becomes the shared key for all nodes.**

```bash
# Run as the opensql account on node1
su - opensql
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519 -C "owldb-shared-key"
```

| Option | Description |
| --- | --- |
| `-t ed25519` | Recommended algorithm. When using RSA `-t rsa -b 4096` |
| `-N ""` | No passphrase for automated SCP |
| `-C` | Comment for key identification |

{% hint style="warning" %}
**Caution**

The private key must also be distributed to all nodes. Since all nodes must perform SCP to each other (node1 → {node2, node3}, node2 → {node1, node3}, …), **all nodes also act as SSH clients.** For the SSH client side to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configure authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run as the opensql account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distribute the key set

On each Replica node, fetch node1's key file via SCP.

```bash
# Run as the opensql account on each Replica node (node2, node3)
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # private key
scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # public key

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

After distribution is complete, verify the `~/.ssh/` permission status on all nodes.

```bash
drwx------   opensql:dba  ~/.ssh
-rw-------   opensql:dba  ~/.ssh/id_ed25519
-rw-r--r--   opensql:dba  ~/.ssh/id_ed25519.pub
-rw-------   opensql:dba  ~/.ssh/authorized_keys
```

### 5. Pre-register known_hosts

To prevent a host key prompt from occurring when running the automation script, collect the host public keys of all nodes in the cluster in advance on each node.

```bash
# Run as the opensql account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Harden sshd configuration

Apply the sshd settings below to strengthen security.

This covers both the **mandatory items**required for the SSH shared-key authentication in this manual to work properly, and the **recommended items**for strengthening security. Please apply them identically on all nodes.

**1) Configuration items**

| Item | Recommended value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allows public key-based authentication. This is the authentication method of this manual, so it must be enabled. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | Path to the per-user public key list file. This is the sshd default; if changed, the file location used in steps 3 and 4 must also be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Verifies the permissions of the user home directory and key files. If permissions are too loose, authentication is denied. This is why compliance with the permission values specified in step 4 is required. |
| `PermitRootLogin` | `no` | Recommended | Reduces the attack surface by blocking SSH access for the root account. |
| `PasswordAuthentication` | `no` | Recommended | Allows only public key authentication by blocking password-based authentication. Blocks brute-force attacks. |
| `AllowUsers` | `opensql` | Recommended | Allows SSH access only for the dedicated DB OS user, blocking access by other accounts. |

**2) How to apply (if needed)**

`/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/` Edit the corresponding item below, then reload sshd to apply the changes.

```bash
# Recommended /etc/ssh/sshd_config configuration example
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers opensql

# Reload configuration (re-reads the configuration only, without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — if Active: active (running), the reload succeeded
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing sshd settings from a remote SSH session, if sshd fails to reload due to an incorrect configuration, additional SSH connections may be refused. When performing this work, make sure you have secured a separate means of console access beforehand.

Immediately after applying the changes, be sure to `systemctl status` run the command to verify that sshd is operating normally. (`Active: failed` If an error message is displayed, revert the configuration immediately)
{% endhint %}

### 7. Source IP Restriction

`authorized_keys` In front of the item, `from=` add the option so that the key is only valid within the internal cluster network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only by the dedicated DB OS user (`opensql`), and **root** must never be shared as the SSH key of the account. Organize ownership of the data directory in advance so that recovery operations and SCP do not require root privileges.
{% endhint %}
