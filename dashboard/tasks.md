**Management > Overview** or **Dashboard**After selecting the DB Service in **Operations** You can perform the following functions by clicking the button.

# Stopping and starting the DB Service <a href="#stop-start-db-service" id="stop-start-db-service"></a>

{% hint style="info" %}
**Note**

The DB Service can be stopped for up to 7 days (168 hours). If you do not start it manually within 7 days, it will automatically start at the next top of the hour after 168 hours have elapsed.
{% endhint %}

1. **Operations** Click the button.
2. **Stop** or **Start** Click the button.

---

# Deleting the DB Service <a href="#delete-db-service" id="delete-db-service"></a>

1. **Operations** Click the button.
2. **Delete** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="warning" %}
**Caution**

Even if you stop the DB Service, charges for the provisioned storage still apply. Charges for backup storage also apply, including manual snapshots and automatic backups within the specified retention period.
{% endhint %}

---

# Deleting the DB Service <a href="#delete-db-service-2" id="delete-db-service-2"></a>

1. **Operations** Click the button.
2. **Delete** Click the button.
3. Enter the DB Service Name in the input field.
4. **Confirm** Click the button.

{% hint style="warning" %}
**Caution**

A deleted DB Service cannot be recovered, and all data is permanently deleted.
{% endhint %}

---

# [Role switchover](#switchover) (Switchover) <a href="#switchover" id="switchover"></a>

This is a function that is enabled only when DR is configured.

1. **Operations** Click the button.
2. **Role switchover** Click the button.
3. Enter the password.
4. **Confirm** Click the button.
5. Select the Standby database that will become the new Primary.
6. Enter the reason for the change.
7. **Confirm** Click the button.

{% hint style="info" %}
**Note**

The dropdown list displays only Standby databases whose status is `Available`.
{% endhint %}
