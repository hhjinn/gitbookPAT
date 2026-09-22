Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in an environment that uses a DR (Disaster Recovery) configuration. Administrators can perform role switching directly in various situations such as failure response or regular maintenance, **Management > Overview** you perform this task on the page.

Role switching proceeds in two ways depending on the current state of the Primary DB. If the Primary DB is in a normal state, roles are swapped safely without data loss using the Switchover method. If the Primary DB is in an abnormal state, the selected Standby DB is immediately promoted to the new Primary using the Failover method. The system automatically determines which method to use after checking the state of the Primary DB.

{% hint style="info" %}
**Note**

In the AWS environment, role switching is supported only on the Tibero engine. In the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switching

The role switching modal consists of two steps: **Security Authentication**and **Switching Settings**In the first step, you enter the password of the currently logged-in account to verify administrator privileges, and in the second step, you select the new Primary DB.

1. **Management > Overview** Navigate to the page.
2. **Action** Click the button, and from the dropdown list **Role Switching**click it.
3. **Security Authentication Before Role Switching** In this step, enter the password of the currently logged-in account and **Confirm**click it.

- If the password does not match, the error message "Passwords do not match." appears.

1. **Select New Primary DB** Select the Standby DB to promote from the dropdown.
2. If necessary, **Remarks**enter it.
3. **Confirm**click it.

After the switch request, the system automatically diagnoses the state of the current Primary DB and determines the switching method.

- **Switchover**: Performed when the Primary DB is in a normal state. It safely shuts down the current Primary, completes data synchronization, and then promotes the selected Standby DB to the new Primary.
- **Failover**: Performed when the Primary DB is in an abnormal state. It immediately promotes the selected Standby DB to the new Primary to restore service.

{% hint style="warning" %}
**Caution**

- If the Health of the current Primary DB is in `In Progress` this state, or if all Standby DBs are in `Unavailable` or `In Progress` this state, role switching cannot be performed.
- `Available` A Standby DB that is not in this state cannot be selected from the new Primary DB selection list.
{% endhint %}

{% hint style="info" %}
**Note**

If you receive a "Configuration Normalization Failed" notification after role switching is complete, manual action is required to maintain high availability and DR.
{% endhint %}
