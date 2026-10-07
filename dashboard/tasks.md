You can perform the following functions by selecting a DB Service in **Management > Overview** or **Dashboard** and then clicking the **Actions** button.

# Stopping and starting the DB Service <a href="#stop-start-db-service" id="stop-start-db-service"></a>

Clicking the **Stop** button temporarily stops the DB Service, and a stopped DB Service can be started again by clicking the **Start** button.

1. Click the **Actions** button.
2. Click the **Stop** or **Start** button.

---

# Delete DB Service <a href="#delete-db-service" id="delete-db-service"></a>

1. Click the **Actions** button.
2. Click the **Delete** button.
3. Enter the DB Service Name in the input field.
4. Click the **Confirm** button.

{% hint style="warning" %}
**Caution**

A deleted DB Service cannot be recovered, and all data is permanently deleted.
{% endhint %}

---

# Unregister <a href="#unregister" id="unregister"></a>

This function is enabled only for registered DB Services.

1. Click the **Actions** button.
2. Click the **Unregister** button.
3. Enter the DB Service Name in the input field.
4. Click the **Confirm** button.

{% hint style="info" %}
**Note**

An unregistered DB Service cannot be found in the list, but it can be found again upon re-registration.
{% endhint %}

---

# [Role Switch](#switchover) (Switchover) <a href="#switchover" id="switchover"></a>

This function is enabled only when DR is configured.

1. Click the **Actions** button.
2. Click the **Role Switch** button.
3. Enter the password.
4. Click the **Confirm** button.
5. Select the Standby database that will become the new Primary.
6. Enter the reason for the change.
7. Click the **Confirm** button.

{% hint style="info" %}
**Note**

The dropdown list displays only Standby databases whose status is `Available`.
{% endhint %}

---

# Failback <a href="#failback" id="failback"></a>

This function is enabled only when TAC-DR is configured.

1. Click the **Actions** button.
2. Click the **Failback** button.
3. Enter the password.
4. Click the **Confirm** button.
