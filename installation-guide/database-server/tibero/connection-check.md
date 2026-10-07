This page verifies the inter-node readiness (UID/GID match, SSH key authentication, key file consistency) required for OwlDB Agent connection and DR configuration.

## 1. Verify Agent Connection <a href="#check-agent-connection" id="check-agent-connection"></a>

After accessing the OwlDB UI, click the **Explore** button to check the Agent connection status.

```
http://[OwlDB server IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. Verify UID/GID Match (when configuring DR) <a href="#check-uid-gid" id="check-uid-gid"></a>

Run the following command on each node to compare whether the UID/GID are **identical down to the number**.

```bash
# Run on each node
id tibero
```

Expected result (identical on all nodes):

```bash
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

If the UID or GID number is output differently, recreate the user on that node following the OS user creation procedure in the database server common preparation requirements.

## 3. Verify SSH Key Authentication Operation (when configuring DR) <a href="#check-ssh-key-auth" id="check-ssh-key-auth"></a>

From each node, attempt SSH connection to all other nodes with the tibero account, and verify that the connection succeeds **without a password**.

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

`BatchMode=yes` forces an immediate failure when a password prompt occurs, so key authentication failures can be clearly detected.

## 4. Verify Key File Consistency (when configuring DR) <a href="#check-key-file-consistency" id="check-key-file-consistency"></a>

Verify with a hash whether the distributed key file is identical on all nodes.

```bash
# Run with the tibero account on each node and compare the results (must be identical on all nodes)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

If the hash is output differently between nodes, perform the [key set distribution step](#id-4) of the database server common preparation requirements again.
