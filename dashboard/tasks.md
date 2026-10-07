You can perform the following functions by selecting a DB Service in **Management > Overview** or **Dashboard** and then clicking the **Actions** button.

# Stopping and Starting a DB Service <a href="#stop-start-db-service" id="stop-start-db-service"></a>

{% hint style="info" %}
**Note**

A DB Service can be stopped for up to 7 days (168 hours). If you do not start it manually within 7 days, it automatically starts at the next top of the hour after 168 hours have elapsed.
{% endhint %}

1. Click the **Actions** button.
2. Click the **Stop** or **Start** button.

---

# Deleting a DB Service <a href="#delete-db-service" id="delete-db-service"></a>

1. Click the **Actions** button.
2. Click the **Delete** button.
3. Enter the DB Service Name in the input field.
4. Click the **Confirm** button.

{% hint style="warning" %}
**Caution**

Even if you stop a DB Service, you are still charged for the provisioned storage. You are also charged for backup storage, including manual snapshots and automatic backups within the specified retention period.
{% endhint %}

---

# Deleting a DB Service <a href="#delete-db-service-2" id="delete-db-service-2"></a>

1. Click the **Actions** button.
2. Click the **Delete** button.
3. Enter the DB Service Name in the input field.
4. Click the **Confirm** button.

{% hint style="warning" %}
**Caution**

A deleted DB Service cannot be recovered, and all data is permanently deleted.
{% endhint %}

---

# [Role Switch](#switchover) (Switchover) <a href="#switchover" id="switchover"></a>

This feature is enabled only when DR is configured.

1. Click the **Actions** button.
2. Click the **Role Switch** button.
3. Enter the password.
4. Click the **Confirm** button.
5. Select the Standby database that will become the new Primary.
6. Enter the reason for the change.
7. Click the **Confirm** button.

{% hint style="info" %}
**Note**

Only Standby databases with a status of `Available` are shown in the dropdown list.
{% endhint %}
