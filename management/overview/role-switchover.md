Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in environments using a DR (Disaster Recovery) configuration. Administrators can directly perform role switching in various situations such as failure response or regular maintenance, and **Management > Overview** you perform the operation on the page.

Role switching proceeds in two ways depending on the current status of the Primary DB. If the Primary DB is in a normal state, roles are switched safely without data loss using the Switchover method. If the Primary DB is in an abnormal state, the selected Standby DB is immediately promoted to the new Primary using the Failover method. The system automatically determines which method to proceed with after checking the status of the Primary DB.

{% hint style="info" %}
**Note**

In the AWS environment, role switching is supported only by the Tibero engine. In the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switch <a href="#role-switchover" id="role-switchover"></a>

The role switching modal consists of **Security Authentication**and **Switch Settings**, in two steps. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. **Management > Overview** Move to the page.
2. **Operations** Click the button, and in the dropdown list **Role Switching**.
3. **Security Authentication Before Role Switching** In the step, enter the password of the currently logged-in account and **Confirm**.

- If the password does not match, the error message "Passwords do not match." appears.

1. **Select New Primary DB** In the dropdown, select the Standby DB to be promoted.
2. If necessary, **Remarks**enter.
3. **Confirm**.

After the switch request, the system automatically diagnoses the status of the current Primary DB to determine the switch method.

- **Switchover**: Performed when the Primary DB is in a normal state. The current Primary is safely shut down, data synchronization is completed, and then the selected Standby DB is promoted to the new Primary.
- **Failover**: Performed when the Primary DB is in an abnormal state. The selected Standby DB is immediately promoted to the new Primary to restore service.

{% hint style="warning" %}
**Caution**

- If the Health of the current Primary DB is `In Progress` status, or if all Standby DBs are in `Unavailable` or `In Progress` status, role switching cannot be performed.
- `Available` A Standby DB that is not in the status cannot be selected from the new Primary DB selection list.
{% endhint %}

{% hint style="info" %}
**Note**

If you receive a "Configuration Normalization Failed" notification after completing role switching, manual action is required to maintain high availability and DR.
{% endhint %}
