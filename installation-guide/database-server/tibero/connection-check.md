On this page, you verify the inter-node readiness (UID/GID match, SSH key authentication, key file consistency) required for OwlDB Agent connection and DR configuration.

## 1. Verify Agent connection <a href="#check-agent-connection" id="check-agent-connection"></a>

After connecting to the OwlDB UI **Explore** Press the button to check the Agent connection status.

```
http://[OwlDB server IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. Verify UID/GID match (when configuring DR) <a href="#check-uid-gid" id="check-uid-gid"></a>

Run the following command on each node to check whether the UID/GID **are identical down to the number** and compare.

```bash
# Run on each node
id tibero
```

Expected result (identical on all nodes):

```bash
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

If the UID or GID number is output differently, recreate the user on that node following the OS user creation procedure in the database server common preparations.

## 3. Verify SSH key authentication behavior (when configuring DR) <a href="#check-ssh-key-auth" id="check-ssh-key-auth"></a>

From each node, attempt SSH connection to all other nodes with the tibero account, **without a password** and verify whether the connection is established.

```bash
# Run with the tibero account on Node1(10.10.0.11)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run with the tibero account on Node2(10.10.0.12)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run with the tibero account on Node3(10.10.0.13)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
```

`BatchMode=yes`forces immediate failure if a password prompt occurs, so key authentication failure can be clearly detected.

## 4. Verify key file consistency (when configuring DR) <a href="#check-key-file-consistency" id="check-key-file-consistency"></a>

Verify with a hash whether the distributed key files are identical on all nodes.

```bash
# Run with the tibero account on each node and compare results (must be identical on all nodes)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

If the hash is output differently between nodes, perform the [key set distribution step](#id-4)in the database server common preparations again.
