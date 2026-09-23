**Management > Overview** or **Dashboard**After selecting a DB Service from **Actions** click the button to perform the functions below.

# Stopping and Starting a DB Service

{% hint style="info" %}
**Note**

A DB Service can be stopped for up to 7 days (168 hours). If you do not start it manually within 7 days, it will start automatically at the next top of the hour after 168 hours have elapsed.
{% endhint %}

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

Even if you stop a DB Service, charges for the provisioned storage still apply. Charges for backup storage, including manual snapshots and automatic backups within the specified retention period, also apply.
{% endhint %}

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

The dropdown list displays only Standby databases whose status is `Available`Only Standby databases are displayed.
{% endhint %}
