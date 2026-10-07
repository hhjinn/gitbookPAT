Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in environments using a DR (Disaster Recovery) configuration. Administrators can perform role switching directly in various situations, such as failure response or regular maintenance, and the operation is performed on the **Management > Overview** page.

Role switching proceeds in one of two ways depending on the current status of the Primary DB. If the Primary DB is in a normal state, the role is switched safely without data loss using the Switchover method. If the Primary DB is in an abnormal state, the selected Standby DB is immediately promoted to the new Primary using the Failover method. The system automatically decides which method to use after checking the status of the Primary DB.

{% hint style="info" %}
**Note**

In AWS environments, role switching is supported only by the Tibero engine. In Azure environments, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switch <a href="#role-switchover" id="role-switchover"></a>

The role switching modal consists of two steps: **Security Authentication** and **Switch Settings**. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. Go to the **Management > Overview** page.
2. Click the **Actions** button, and then click **Role Switching** from the dropdown list.
3. In the **Security Authentication Before Role Switching** step, enter the password of the currently logged-in account and click **Confirm**.

- If the password does not match, the error message "Passwords do not match." appears.

1. Select the Standby DB to promote from the **Select New Primary DB** dropdown.
2. Enter **Remarks** if necessary.
3. Click **Confirm**.

After the switch request, the system automatically diagnoses the current status of the Primary DB to determine the switching method.

- **Switchover**: Performed when the Primary DB is in a normal state. It safely shuts down the current Primary and completes data synchronization, then promotes the selected Standby DB to the new Primary.
- **Failover**: Performed when the Primary DB is in an abnormal state. It immediately promotes the selected Standby DB to the new Primary to restore service.

{% hint style="warning" %}
**Caution**

- Role switching cannot be performed if the Health of the current Primary DB is in the `In Progress` state, or if all Standby DBs are in the `Unavailable` or `In Progress` state.
- A Standby DB that is not in the `Available` state cannot be selected from the new Primary DB selection list.
{% endhint %}

{% hint style="info" %}
**Note**

If you receive a "Configuration Normalization Failed" notification after role switching is complete, manual action is required to maintain high availability and DR.
{% endhint %}
