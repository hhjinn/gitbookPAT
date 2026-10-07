Role switching is a function that manually swaps the roles of the Primary DB and Standby DB in an environment using a DR (Disaster Recovery) configuration. The administrator can perform role switching directly in various situations such as failure response or regular maintenance, and **Management > Overview** work is done on the page.

Role switching proceeds in two ways depending on the current status of the Primary DB. If the Primary DB is in a normal status, the role is swapped safely without data loss using the Switchover method. If the Primary DB is in an abnormal status, the selected Standby DB is immediately promoted to the new Primary using the Failover method. The system automatically determines which method to proceed with after checking the status of the Primary DB.

{% hint style="info" %}
**Note**

In the AWS environment, role switching is supported only by the Tibero engine. In the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

## Role switchover <a href="#role-switchover" id="role-switchover"></a>

The role switching modal is **Security Authentication**and **Switch Settings**consists of two steps. In the first step, you enter the password of the currently logged-in account to verify administrator privileges, and in the second step, you select the new Primary DB.

1. **Management > Overview** Navigate to the page.
2. **Operation** Click the button, and from the dropdown list **Role Switching**Click.
3. **Security Authentication Before Role Switching** In the step, enter the password of the currently logged-in account and **Confirm**Click.

- If the passwords do not match, the "The passwords do not match." error message appears.

1. **Select New Primary DB** Select the Standby DB to promote from the dropdown.
2. If necessary **Remarks**enter the.
3. **Confirm**Click.

After the switch request, the system automatically diagnoses the current status of the Primary DB and determines the switch method.

- **Switchover**: Performed when the Primary DB is in a normal status. It safely shuts down the current Primary, completes data synchronization, and then promotes the selected Standby DB to the new Primary.
- **Failover**: Performed when the Primary DB is in an abnormal status. It immediately promotes the selected Standby DB to the new Primary to restore the service.

{% hint style="warning" %}
**Caution**

- The Health of the current Primary DB is `In Progress` status, or all Standby DBs are in `Unavailable` or `In Progress` status, role switching cannot be performed.
- `Available` A Standby DB that is not in the status cannot be selected in the new Primary DB selection list.
{% endhint %}

{% hint style="info" %}
**Note**

If you receive a "Configuration normalization failed" notification after role switching is complete, manual action is required to maintain high availability and DR.
{% endhint %}
