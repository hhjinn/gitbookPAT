Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in an environment using a DR (Disaster Recovery) configuration. Administrators can perform role switching directly in various situations such as failure response or regular inspections, and the work is done on the **Management > Overview** page.

Role switching proceeds in one of two ways depending on the current state of the Primary DB. The system automatically determines which method to use after checking the state of the Primary DB.

{% hint style="info" %}
**Note**

In the AWS environment, role switching is supported only by the Tibero engine. In the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

## Role Switch <a href="#role-switchover" id="role-switchover"></a>

The role switching modal consists of two steps: **Security Authentication** and **Switch Settings**. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. Go to the **Management > Overview** page.
2. Click the **Actions** button.
3. Click **Role Switching** in the dropdown list.
4. In the **Security Authentication Before Role Switching** step, enter the password of the currently logged-in account and click **Confirm**. If the password does not match, the error message "Password does not match." appears.
5. Select the Standby DB to promote from the **Select New Primary DB** dropdown.
6. Enter **Remarks** if necessary.
7. Click **Confirm**.

After the switch request, the system automatically diagnoses the current state of the Primary DB and determines the switching method.

- **Primary DB is in a normal state**: Performed using the Switchover method. The current Primary is safely shut down and data synchronization is completed, after which the selected Standby DB is promoted to the new Primary.
- **Primary DB is in an abnormal state**: Performed using the Failover method. The selected Standby DB is immediately promoted to the new Primary to restore service.

{% hint style="warning" %}
**Caution**

- Role switching cannot be performed if the current Primary DB's Health is in the `In Progress` state, or if all Standby DBs are in the `Unavailable` or `In Progress` state.
- A Standby DB that is not in the `Available` state cannot be selected from the new Primary DB selection list.
- If you receive a "Configuration Normalization Failed" notification after role switching is complete, manual action is required to maintain high availability and DR.
{% endhint %}
