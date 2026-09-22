**Management > Overview** or **Dashboard**After selecting a DB Service in **Actions** Click the button to perform the following functions.

# Stopping and Starting a DB Service

**Stop** Click the button to temporarily stop the DB Service, and a stopped DB Service can be **Start** restarted by clicking the button.

1. **Actions** Click the button.
2. **Stop** or **Start** Click the button.

---

# Deleting a DB Service

1. **Actions** Click the button.
2. **Delete** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="warning" %}
**Caution**

A deleted DB Service cannot be recovered, and all data is permanently deleted.
{% endhint %}

---

# Deregister

This function is enabled only for registered DB Services.

1. **Actions** Click the button.
2. **Deregister** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="info" %}
**Note**

A deregistered DB Service cannot be viewed in the list, and it can be viewed again upon re-registration.
{% endhint %}

---

# [Switchover](#switchover) (Switchover)

This function is enabled only when DR is configured.

1. **Actions** Click the button.
2. **Switchover** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
5. Select the Standby database that will become the new Primary.
6. Enter the reason for the change.
7. **Confirm** Click the button.

{% hint style="info" %}
**Note**

In the dropdown list, only databases whose status is `Available`are displayed as Standby databases.
{% endhint %}

---

# Failback

This function is enabled only when TAC-DR is configured.

1. **Actions** Click the button.
2. **Failback** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
