**Management > Overview** or **Dashboard**After selecting the DB Service in **Operations** you can perform the following functions by clicking the button.

# Stopping and Starting the DB Service <a href="#stop-start-db-service" id="stop-start-db-service"></a>

**Stop** Clicking the button temporarily stops the DB Service, and the stopped DB Service can be **Start** restarted by clicking the button.

1. **Operations** Click the button.
2. **Stop** or **Start** Click the button.

---

# Deleting a DB Service <a href="#delete-db-service" id="delete-db-service"></a>

1. **Operations** Click the button.
2. **Delete** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="warning" %}
**Caution**

A deleted DB Service cannot be recovered, and all data is permanently deleted.
{% endhint %}

---

# Deregistration <a href="#unregister" id="unregister"></a>

This function is enabled only for registered DB Services.

1. **Operations** Click the button.
2. **Deregistration** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="info" %}
**Note**

An unregistered DB Service cannot be found in the list, and can be found again upon re-registration.
{% endhint %}

---

# [Role Switch](#switchover) (Switchover) <a href="#switchover" id="switchover"></a>

This function is enabled only in a DR configuration.

1. **Operations** Click the button.
2. **Role Switch** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
5. Select the Standby database to become the new Primary.
6. Enter the reason for the change.
7. **Confirm** Click the button.

{% hint style="info" %}
**Note**

In the dropdown list, only Standby databases whose status is `Available`are displayed.
{% endhint %}

---

# Failback <a href="#failback" id="failback"></a>

This function is enabled only in a TAC-DR configuration.

1. **Operations** Click the button.
2. **Failback** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
