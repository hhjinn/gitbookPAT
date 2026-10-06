Role switching is a feature that manually swaps the roles of the Primary DB and Standby DB in environments using a DR (Disaster Recovery) configuration. Administrators can directly perform role switching in various situations such as failure response or regular maintenance, and **Management > Overview** it is performed on the page.

Role switching proceeds in one of two ways depending on the current state of the Primary DB. The system automatically determines which way to proceed after checking the state of the Primary DB.

{% hint style="info" %}
**Note**

In the AWS environment, role switching is supported only by the Tibero engine. In the Azure environment, both Tibero and OpenSQL are supported.
{% endhint %}

## Role switchover <a href="#role-switchover" id="role-switchover"></a>

The role switching modal consists of **Security Authentication**and **Switch Settings**two steps. In the first step, you verify administrator privileges by entering the password of the currently logged-in account, and in the second step, you select the new Primary DB.

1. **Management > Overview** You are redirected to the page.
2. **Operations** Click the button.
3. From the dropdown list, **Role Switch**Click.
4. **Security Authentication Before Role Switching** In this step, enter the password of the currently logged-in account and **Confirm**Click. If the password does not match, the error message "The password does not match." appears.
5. **Select New Primary DB** Select the Standby DB to promote from the dropdown.
6. If necessary, **Remarks**enter the
7. **Confirm**Click.

After the switch request, the system automatically diagnoses the state of the current Primary DB and determines the switching method.

- **Primary DB in Normal State**: Performed using the Switchover method. After safely shutting down the current Primary and completing data synchronization, the selected Standby DB is promoted to the new Primary.
- **Primary DB in Abnormal State**: Performed using the Failover method. The selected Standby DB is immediately promoted to the new Primary to recover the service.

{% hint style="warning" %}
**Caution**

- If the Health of the current Primary DB is `In Progress` state, or all Standby DBs are in the `Unavailable` or `In Progress` state, role switching cannot be performed.
- `Available` A Standby DB that is not in the

state cannot be selected from the new Primary DB selection list.
- If you receive a "Configuration Normalization Failed" notification after role switching is completed, manual action is required to maintain high availability and DR.
{% endhint %}
