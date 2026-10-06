Role switchover is a feature that manually swaps the roles of the Primary DB and Standby DB in an environment using a DR (Disaster Recovery) configuration. Administrators can perform role switchover directly in various situations such as failure response or regular maintenance, **Management > Overview** performed on the page.

Role switchover proceeds in two ways depending on the current state of the Primary DB. The system automatically determines which method to use after checking the state of the Primary DB.

{% hint style="info" %}
**Note**

In AWS environments, role switchover is supported only for the Tibero engine. In Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switchover

The role switchover modal **Security Authentication**and **Switchover Settings**consists of two steps. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. **Management > Overview** Navigate to the page.
2. **Actions** Click the button.
3. From the dropdown list, **Role Switchover**Click.
4. **Security Authentication Before Role Switchover** In this step, enter the password of the currently logged-in account and **Confirm**Click. If the password does not match, the error message "The password does not match." appears.
5. **Select New Primary DB** From the dropdown, select the Standby DB to be promoted.
6. If necessary, **Remarks**Enter.
7. **Confirm**Click.

After the switchover request, the system automatically diagnoses the current state of the Primary DB and determines the switchover method.

- **Primary DB in normal state**: Performed using the Switchover method. The current Primary is safely shut down and data synchronization is completed, after which the selected Standby DB is promoted to the new Primary.
- **Primary DB in abnormal state**: Performed using the Failover method. The selected Standby DB is immediately promoted to the new Primary to restore the service.

{% hint style="warning" %}
**Caution**

- If the Health of the current Primary DB is `In Progress` state, or if all Standby DBs are in the `Unavailable` or `In Progress` state, role switchover cannot be performed.
- `Available` Standby DBs not in the state cannot be selected from the new Primary DB selection list.
- If you receive a "Configuration Normalization Failed" notification after the role switchover is completed, manual action is required to maintain high availability and DR.
{% endhint %}
