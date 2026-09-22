Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in an environment using a DR (Disaster Recovery) configuration. Administrators can perform role switching directly in various situations such as failure response or regular maintenance, **Management > Overview** You perform this operation on the page.

Role switching proceeds in two ways depending on the current state of the Primary DB. If the Primary DB is in a normal state, roles are swapped safely without data loss using the Switchover method. If the Primary DB is in an abnormal state, the selected Standby DB is immediately promoted to the new Primary using the Failover method. The system automatically determines which method to use after checking the state of the Primary DB.

{% hint style="info" %}
**Note**

In AWS environments, role switching is supported only by the Tibero engine. In Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switching

The role switching modal **Security Authentication**and **Switching Settings**consists of two steps. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. **Management > Overview** Navigate to the page.
2. **Actions** Click the button, and **Role Switching**click it from the dropdown list.
3. **Security Authentication Before Role Switching** In this step, enter the password of the currently logged-in account and **Confirm**Click it.

- If the password does not match, the error message "Passwords do not match." appears.

1. **Select New Primary DB** Select the Standby DB to promote from the dropdown.
2. If necessary **Remarks**Enter it.
3. **Confirm**Click it.

After the switching request, the system automatically diagnoses the state of the current Primary DB and determines the switching method.

- **Switchover**: Performed when the Primary DB is in a normal state. It safely shuts down the current Primary, completes data synchronization, and then promotes the selected Standby DB to the new Primary.
- **Failover**: Performed when the Primary DB is in an abnormal state. It immediately promotes the selected Standby DB to the new Primary to restore the service.

{% hint style="warning" %}
**Caution**

- If the Health of the current Primary DB is in the `In Progress` state, or if all Standby DBs are in the `Unavailable` or `In Progress` state, role switching cannot be performed.
- `Available` A Standby DB that is not in the state cannot be selected from the new Primary DB selection list.
{% endhint %}

{% hint style="info" %}
**Note**

If you receive a "Configuration Normalization Failed" notification after role switching is complete, manual action is required to maintain high availability and DR.
{% endhint %}
