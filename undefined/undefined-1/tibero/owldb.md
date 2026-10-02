This page verifies the inter-node readiness (UID/GID match, SSH key authentication, key file consistency) required for OwlDB Agent connection and DR configuration.

## 1. Verify Agent connection

After accessing the OwlDB UI **Explore** Click the button to verify the Agent connection status.

```
http://[OwlDB server IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. Verify UID/GID match (for DR configuration)

Run the following command on each node to verify that the UID/GID are **identical down to the number** and compare.

```bash
# Run on each node
id tibero
```

Expected result (identical on all nodes):

```bash
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

If the UID or GID numbers are output differently, recreate the user on that node following the OS user creation procedure in the database server common prerequisites.

## 3. Verify SSH key authentication operation (for DR configuration)

From each node, attempt SSH connections to all other nodes using the tibero account, and **without a password** verify that the connection succeeds.

```bash
# Run as the tibero account on Node1 (10.10.0.11)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run as the tibero account on Node2 (10.10.0.12)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run as the tibero account on Node3 (10.10.0.13)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
```

`BatchMode=yes`forces an immediate failure if a password prompt occurs, so key authentication failures can be clearly detected.

## 4. Verify key file consistency (for DR configuration)

Verify via hash that the distributed key files are identical on all nodes.

```bash
# Run as the tibero account on each node, then compare results (must be identical on all nodes)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

If the hashes are output differently between nodes, in the database server common prerequisites [key set distribution step](#id-4)perform again.
