This page verifies the inter-node readiness required for OwlDB Agent connection and DR configuration (UID/GID match, SSH key authentication, key file consistency).

## 1. Verify Agent Connection

After accessing the OwlDB UI, **Explore** Click the button to verify the Agent connection status.

```
http://[OwlDB server IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. Verify UID/GID Match (for DR Configuration)

Run the following command on each node to compare whether the UID/GID are **identical down to the number** compare.

```bash
# Run on each node
id tibero
```

Expected result (identical across all nodes):

```bash
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

If the UID or GID numbers are output differently, recreate the user on that node according to the OS user creation procedure in the database server common preparation requirements.

## 3. Verify SSH Key Authentication Behavior (for DR Configuration)

From each node, attempt SSH connection to all other nodes using the tibero account, **without a password** verify that the connection succeeds.

```bash
# Run with the tibero account on Node1 (10.10.0.11)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run with the tibero account on Node2 (10.10.0.12)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Run with the tibero account on Node3 (10.10.0.13)
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
```

`BatchMode=yes`forces immediate failure when a password prompt occurs, allowing key authentication failures to be clearly detected.

## 4. Verify Key File Consistency (for DR Configuration)

Verify by hash whether the deployed key file is identical across all nodes.

```bash
# Run with the tibero account on each node, then compare results (must be identical across all nodes)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

If the hashes are output differently between nodes, perform the [key set deployment step](#id-4)in the database server common preparation requirements again.
