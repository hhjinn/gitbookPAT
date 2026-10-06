The OwlDB license fee is determined by the combination of instance Status and Health. This page explains whether a license fee is charged for each status combination and what it means.

{% hint style="info" %}
**Note**

Infrastructure resource fees such as instances, storage, and network are incurred separately according to each CSP's billing policy, regardless of this criterion.
{% endhint %}

# Metering Criteria

DB service license fees are calculated based on the time used, with a minimum billing unit of 1 minute.

| Item | Criterion |
| --- | --- |
| Minimum Billing Unit | 1 minute |
| Second-level processing | Rounded up to the nearest minute |
| Fee Display | US dollars ($), rounded to two decimal places |

# Billing Criterion

<table data-full-width="true"><thead><tr><th>Status</th><th>Health</th><th>Billing Status</th><th>Description</th></tr></thead><tbody><tr><td>Provisioning</td><td>-</td><td>Not billed</td><td>Instance being created</td></tr><tr><td>Running</td><td>available</td><td>Billed</td><td>Operating normally</td></tr><tr><td>Updating</td><td><ul><li>available</li><li>in progress</li></ul></td><td>Billed</td><td>Operations in progress such as spec changes, restarts, migrations, recovery, and backups</td></tr><tr><td>Degraded</td><td><ul><li>available</li><li>in progress</li><li>limited</li></ul></td><td>Billed</td><td><ul><li>Some instances being restarted or reconfigured</li><li>Some management functions restricted</li></ul></td></tr><tr><td>Degraded</td><td>unavailable</td><td>Not billed</td><td>Unavailable due to DB/VM outage, etc.</td></tr><tr><td>Degraded</td><td>retired</td><td>Not billed</td><td>Unused instance after Failover</td></tr><tr><td>Failover</td><td>in progress</td><td>Billed</td><td>Automatic Failover in progress</td></tr><tr><td>Down</td><td>unavailable</td><td>Not billed</td><td>Entire DB service outage</td></tr><tr><td>Stopping</td><td>in progress</td><td>Billed</td><td>Transitioning to stopped state</td></tr><tr><td>Stopped</td><td>unavailable</td><td>Not billed</td><td>All resources temporarily deactivated</td></tr><tr><td>Starting</td><td>in progress</td><td>Not billed</td><td>Restarting from stopped state</td></tr><tr><td>Terminating</td><td>unavailable</td><td>Not billed</td><td>Resources and data being permanently deleted</td></tr></tbody></table>

{% hint style="info" %}
**Note**

If you do not plan to use the DB service temporarily, **Stop**is recommended.

In the stopped state, DB service license and instance usage fees are not charged, and data is preserved so you can restart it when needed. However, since volumes are maintained to store the saved data, storage fees continue to be charged.

To stop all fees including storage fees, you must delete the DB service. When you delete a DB service, all resources and data such as instances, volumes, and backups are permanently deleted and cannot be recovered.
{% endhint %}
