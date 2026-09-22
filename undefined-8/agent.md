`agent.env` If a value needs to be changed or the Agent fails to start for an unknown reason, you can restart the Agent with the command below.

```bash
sh tbagent_start.sh
```

The following selection prompt is displayed upon restart.

```
============================================
 DB Agent config.json generation script
============================================
▲ Warning: The config.json file already exists.
 1) Overwrite the existing config.json and continue
 ▲ Warning: If you perform this operation on a host that is properly installed/registered, the Agent will start
 with a new config.json, and OwlDB will no longer be able to identify that host.
 2) Use the existing config.json as is and continue
 3) Cancel
```

| Situation | Selection |
| --- | --- |
| Restart on a host properly installed/registered in OwlDB | Be sure to **Option 2** Select |
| Restart on a host not yet registered in OwlDB | Either option 1 or option 2 is possible |

{% hint style="warning" %}
**Caution**

If you select option 1 on a properly registered host, `config.json`a new one is generated and the Agent's unique identifier changes. In this case, OwlDB can no longer identify that host.
{% endhint %}
