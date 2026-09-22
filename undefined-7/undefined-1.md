You can view and manage all accounts registered in OwlDB from the Account Management menu.

{% hint style="info" %}
**Note**

Only Root can access the Account Management menu.
{% endhint %}

## Account Role Guide

### Root

The Root account can create, view, and modify Member accounts, and can grant or revoke access permissions for specific DB Services. It can also use the database installation, DB Service registration, and infrastructure discovery features.

When a user without an account (a sign-up applicant) requests account creation, the request is forwarded to Root, and once Root approves it, the account is activated.

### Member

A Member can use the operation and management features provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services to which Root has granted access permissions. A user without an account (a sign-up applicant) can request the creation of their own account from Root.

### Permission Matrix by Role

<table data-full-width="true"><thead><tr><th>Feature Category</th><th>Detailed Feature</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account Management</td><td>Create and Delete Users</td><td>O</td><td>X</td></tr><tr><td>Permission Management</td><td>DB Service Assignment</td><td>O</td><td>X</td></tr><tr><td>Service Management</td><td>Install/Delete DB Service</td><td>O</td><td>X</td></tr><tr><td>Service Management</td><td>Register/Deregister DB Service</td><td>O</td><td>X</td></tr><tr><td>DB Management</td><td><ul><li>Spec Change</li><li>Tablespace Management</li><li>Parameter Management</li><li>Backup/Recovery</li><li>Monitoring</li></ul></td><td>O</td><td>O</td></tr></tbody></table>

---

## View Account List

**Account Management** When you enter the menu, you can view the list of all accounts registered in OwlDB. You can filter by account status, or search by ID, name, or email.

**Account Status**

| Account Status | Description |
| --- | --- |
| Active | Normal account |
| Inactive | Account that has been deactivated |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account Management Tasks

## Create Account

You can directly create a new Member account.

1. **Account Management** From the menu, **Create** Click the button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions at the same time.
4. **Create** Clicking the button completes the account creation.

Once the account is created, a permission grant notification is sent to that Member.

---

## View and Modify Account Details

Clicking a user ID in the account list lets you view the detailed information of that account. You can view and modify the detailed information of all accounts, including your own.

The items you can view are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modification date

To modify, on the detail page **Modify** Click the button.

---

## Approve Account Creation Request

When a user without an account requests account creation, the request record is **Account Management** in the menu `Account Requested` displayed with the status.

When Root changes the account to the `Active` status, the account is activated.

---

## Delete Account

1. **Account Management** Select the account to delete from the menu.
2. **Delete** Click the button.
3. In the confirmation modal **Delete** When you click the button, access for that account is immediately blocked.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing task history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
