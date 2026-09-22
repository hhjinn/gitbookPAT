**Management > Overview** or **Dashboard**After selecting a DB Service in **Actions** you can perform the following functions by clicking the button.

# Stopping and Starting a DB Service

**Stop** Clicking the button allows you to temporarily stop the DB Service, and a stopped DB Service can be **Start** restarted by clicking the button.

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

# Unregister

This function is enabled only for registered DB Services.

1. **Actions** Click the button.
2. **Unregister** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="info" %}
**Note**

An unregistered DB Service cannot be viewed in the list, and can be viewed again upon re-registration.
{% endhint %}

---

# [Role Switch](#switchover) (Switchover)

This function is enabled only when DR is configured.

1. **Actions** Click the button.
2. **Role Switch** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
5. Select the Standby database to become the new Primary.
6. Enter the reason for the change.
7. **Confirm** Click the button.

{% hint style="info" %}
**Note**

In the dropdown list, the status is `Available`Only Standby databases that are will be displayed.
{% endhint %}

---

# Failback

This function is enabled only when TAC-DR is configured.

1. **Actions** Click the button.
2. **Failback** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
