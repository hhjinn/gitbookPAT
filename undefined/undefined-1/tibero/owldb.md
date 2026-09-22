This page verifies the inter-node readiness (UID/GID match, SSH key authentication, key file consistency) required for OwlDB Agent connection and DR configuration.

## 1. Verify Agent Connection

After accessing the OwlDB UI, **Explore** Click the button to check the Agent connection status.

```
http://[OwlDB Server IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. Verify UID/GID Match (When Configuring DR)

Run the following command on each node to compare whether the UID/GID are **identical down to the number** .

```bash
# Run on each node
id tibero
```

Expected result (identical on all nodes):

```bash
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

If the UID or GID number is displayed differently, recreate the user on that node following the OS user creation procedure in the Database Server Common Preparations.

## 3. Verify SSH Key Authentication Operation (When Configuring DR)

From each node, attempt SSH connection to all remaining nodes using the tibero account, and verify **without a password** whether the connection succeeds.

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

`BatchMode=yes`forces immediate failure when a password prompt occurs, allowing key authentication failures to be clearly detected.

## 4. Verify Key File Consistency (When Configuring DR)

Verify with a hash whether the deployed key files are identical on all nodes.

```bash
# Run with the tibero account on each node and compare the results (must be identical on all nodes)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

If the hash is displayed differently between nodes, redo the [key set deployment step](#id-4)in the Database Server Common Preparations.
