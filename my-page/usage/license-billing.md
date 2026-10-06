OwlDB license fees are determined by the combination of instance Status and Health. This page explains whether license fees are charged for each status combination and what they mean.

{% hint style="info" %}
**Note**

Infrastructure resource fees such as instances, storage, and network are incurred separately according to each CSP's billing policy, independent of these criteria.
{% endhint %}

# Metering Criteria <a href="#metering-criteria" id="metering-criteria"></a>

DB service license fees are calculated based on the time used, with a minimum billing unit of 1 minute.

| Item | Criteria |
| --- | --- |
| Minimum Billing Unit | 1 minute |
| Seconds Handling | Rounded up to the minute |
| Fee Display | Dollars ($), rounded to the second decimal place |

# Billing Criteria <a href="#billing-criteria" id="billing-criteria"></a>

<table><thead><tr><th>Status</th><th>Health</th><th>Billing status</th><th>Description</th></tr></thead><tbody><tr><td>Provisioning</td><td>-</td><td>Not Billed</td><td>Instance Creating</td></tr><tr><td>Running</td><td>available</td><td>Billed</td><td>Operating normally</td></tr><tr><td>Updating</td><td><ul><li>available</li><li>in progress</li></ul></td><td>Billed</td><td>Operation in progress, such as spec change, restart, migration, recovery, or backup</td></tr><tr><td>Degraded</td><td><ul><li>available</li><li>in progress</li><li>limited</li></ul></td><td>Billed</td><td><ul><li>Some instances are restarting or reconfiguring</li><li>Some management features are restricted</li></ul></td></tr><tr><td>Degraded</td><td>unavailable</td><td>Not Billed</td><td>Unavailable due to DB/VM outage, etc.</td></tr><tr><td>Degraded</td><td>retired</td><td>Not Billed</td><td>Unused instance after Failover</td></tr><tr><td>Failover</td><td>in progress</td><td>Billed</td><td>Automatic Failover in progress</td></tr><tr><td>Down</td><td>unavailable</td><td>Not Billed</td><td>Entire DB service is down</td></tr><tr><td>Stopping</td><td>in progress</td><td>Billed</td><td>Transitioning to stopped state</td></tr><tr><td>Stopped</td><td>unavailable</td><td>Not Billed</td><td>All resources temporarily deactivated</td></tr><tr><td>Starting</td><td>in progress</td><td>Not Billed</td><td>Restarting from stopped state</td></tr><tr><td>Terminating</td><td>unavailable</td><td>Not Billed</td><td>Permanently deleting resources and data</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you do not plan to use the DB service temporarily **Stop**is recommended.

In the stopped state, DB service license and instance usage fees are not billed, and data is preserved so that you can restart whenever needed. However, since the volume is retained to store the saved data, storage fees continue to be charged.

To stop all charges including storage fees, you must delete the DB service. When the DB service is deleted, all resources and data, including instances, volumes, and backups, are permanently deleted and cannot be recovered.
{% endhint %}
