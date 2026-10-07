If the `agent.env` value needs to be changed or the Agent fails to start for an unknown reason, you can restart the Agent with the command below.

```bash
sh tbagent_start.sh
```

Upon restart, a selection prompt like the following is displayed.

```
============================================
 DB Agent config.json generation script
============================================
▲ Warning: The config.json file already exists.
 1) Overwrite the existing config.json and continue
 ▲ Warning: If you perform this operation on a normally installed/registered host, the Agent starts
 with the new config.json, and OwlDB can no longer identify that host.
 2) Use the existing config.json as is and continue
 3) Cancel
```

| Situation | Selection |
| --- | --- |
| Restart on a host normally installed/registered in OwlDB | Be sure to select **No. 2** |
| Restart on a host not yet registered in OwlDB | Either No. 1 or No. 2 is possible |

{% hint style="warning" %}
**Caution**

If you select No. 1 on a normally registered host, `config.json` is newly created and the Agent's unique identifier value changes. In this case, OwlDB can no longer identify that host.
{% endhint %}
