`agent.env` If a value change is required or the Agent fails to start for an unknown reason, you can restart the Agent with the command below.

```bash
sh tbagent_start.sh
```

When restarting, the selection prompt shown below is displayed.

```
============================================
 DB Agent config.json generation script
============================================
▲ Warning: The config.json file already exists.
 1) Overwrite the existing config.json and continue
 ▲ Warning: Performing this operation on a properly installed/registered host will start the Agent with a new config.json,
 causing OwlDB to no longer be able to identify the host.
 2) Use the existing config.json as is and continue
 3) Cancel
```

| Situation | Select |
| --- | --- |
| Restarting on a host properly installed/registered in OwlDB | Be sure to **Option 2** Select |
| Restarting on a host not yet registered in OwlDB | Either option 1 or option 2 is acceptable |

{% hint style="warning" %}
**Caution**

If you select option 1 on a properly registered host, `config.json`a new one is generated and the Agent's unique identifier value changes. In this case, OwlDB can no longer identify the host.
{% endhint %}
