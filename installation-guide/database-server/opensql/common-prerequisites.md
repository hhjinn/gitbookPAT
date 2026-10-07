This is the common preparation procedure for database servers monitored by OwlDB. For procedures that differ depending on the configuration method, such as disk requirements, network settings, and deployment file composition, refer to [Installed DB Environment Preparation Guide](install-db-prerequisites.md)and [Registered DB Environment Preparation Guide](register-db-prerequisites.md)respectively.

## OS User / SSH Key Configuration <a href="#os-user-ssh-key" id="os-user-ssh-key"></a>

{% hint style="info" %}
**Note**

This content is **only required when using a DR configuration**.
{% endhint %}

In a DR configuration, OwlDB transfers data files between nodes via SCP. For this, a dedicated OS user is created on each node, and a shared key pair is distributed to enable passwordless SSH access.

The example values used in the procedure below are as follows. Please substitute them to match your actual environment.

- DB OS user: `opensql`
- SSH port: `22`
- Database node Node1 (Leader): `10.10.0.11` Node2 (Replica): `10.10.0.12` Node3 (Replica): `10.10.0.13`
- Cluster internal network CIDR: `10.10.0.0/16`

### 1. Creating a Dedicated DB OS User

On all nodes, **with the same UID/GID**create the user.

```bash
# Run as the root account on each node
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash opensql
```

{% hint style="warning" %}
**Caution**

If the UID/GID differs between nodes, the ownership of files transferred via SCP will be mismatched, and the DB process will not be able to read the files. Create them by explicitly specifying the same numbers.
{% endhint %}

### 2. Creating an SSH Key Pair (Performed Only Once on Node 1)

Node1(`10.10.0.11`) generate the key pair only once. **This key pair becomes the shared key for all nodes.**

```bash
# Run with the opensql account on node1
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

The private key must also be distributed to all nodes. Since every node must perform SCP to every other node (node1 → {node2, node3}, node2 → {node1, node3}, …), **every node also acts as an SSH client.** For the SSH client side to sign the challenge, the private key must exist locally.
{% endhint %}

### 3. Configuring authorized_keys

Register the generated public key in `authorized_keys`.

```bash
# Run with the opensql account on node1
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 4. Distributing the Key Set <a href="#id-4" id="id-4"></a>

On each Replica node, fetch node1's key file via SCP.

```bash
# Run with the opensql account on each Replica node (node2, node3)
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # private key
scp -P 22 opensql@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # public key

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

After distribution is complete, verify the permission status of `~/.ssh/` on all nodes.

```bash
drwx------   opensql:dba  ~/.ssh
-rw-------   opensql:dba  ~/.ssh/id_ed25519
-rw-r--r--   opensql:dba  ~/.ssh/id_ed25519.pub
-rw-------   opensql:dba  ~/.ssh/authorized_keys
```

### 5. Pre-registering known_hosts

To prevent a host key prompt from occurring when running the automation script, collect the host public keys of all nodes in the cluster in advance on each node.

```bash
# Run with the opensql account on each node
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

### 6. Hardening sshd Configuration

Apply the sshd settings below to strengthen security.

For the SSH shared key authentication in this manual to work correctly, the **required items**, and for strengthening security, the **recommended items**are organized together. Please apply them identically on all nodes.

**1) Configuration items**

| Item | Recommended value | Category | Description |
| --- | --- | --- | --- |
| `PubkeyAuthentication` | `yes` | **Required** | Allow public key-based authentication. This is the authentication method used in this manual, so it must be enabled. |
| `AuthorizedKeysFile` | `.ssh/authorized_keys` | **Required** | Path to the per-user public key list file. This is the sshd default; if changed, the file location used in steps 3 and 4 must also be changed accordingly. |
| `StrictModes` | `yes` | Recommended | Verifies permissions on the user home directory and key files. If permissions are too loose, authentication is denied. This is why the permission values specified in step 4 must be followed. |
| `PermitRootLogin` | `no` | Recommended | Reduces the attack surface by blocking SSH access for the root account. |
| `PasswordAuthentication` | `no` | Recommended | Blocks password-based authentication and allows only public key authentication. Blocks brute-force attacks. |
| `AllowUsers` | `opensql` | Recommended | Allows SSH access only for the DB-dedicated OS user and blocks access from other accounts. |

**2) How to apply (if needed)**

`/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/` Edit the corresponding item below and then reload sshd to apply.

```bash
# Example of recommended /etc/ssh/sshd_config settings
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers opensql

# Reload the configuration (re-reads only the settings without restarting the sshd process)
systemctl reload sshd      # RHEL/CentOS/Rocky family  
# systemctl reload ssh       # Ubuntu/Debian family

# Verify application — if Active: active (running), the reload succeeded
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**Caution**

When changing sshd settings from a remote SSH session, if sshd fails to reload due to an incorrect configuration, additional SSH connections may be refused. When performing this work, make sure you have secured a separate console access method before proceeding.

Immediately after applying the change, be sure to `systemctl status` We recommend verifying that sshd is operating normally using the command. (`Active: failed` If an error message is displayed, you must revert the configuration immediately.)
{% endhint %}

### 7. Source IP restriction

`authorized_keys` In front of the item, `from=` grant the option so that the key is valid only within the cluster-internal network.

```bash
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**Caution**

This key must be used only by the DB-dedicated OS user (`opensql`), and **root** it must never be shared as the SSH key of the account. Organize the data directory ownership in advance so that recovery operations and SCP do not require root privileges.
{% endhint %}
