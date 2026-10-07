In the Account Management menu, you can view and manage all accounts registered in OwlDB.

{% hint style="info" %}
**Note**

The Account Management menu can only be accessed by Root.
{% endhint %}

## Account role guide <a href="#account-roles" id="account-roles"></a>

### Root <a href="#root" id="root"></a>

A Root account can create, view, and edit Member accounts, and can grant or revoke access permissions for specific DB Services. It can also use the database installation, DB Service registration, and infrastructure exploration features.

When a user without an account (a sign-up applicant) requests account creation, the request is forwarded to Root, and once Root approves it, the account is activated.

### Member <a href="#member" id="member"></a>

A Member can use the operation and management features provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services to which Root has granted access permission. A user without an account (a sign-up applicant) can request the creation of their own account from Root.

### Permission matrix by role <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Feature category</th><th>Detailed features</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account Management</td><td>Create and delete users</td><td>O</td><td>X</td></tr><tr><td>Permission management</td><td>DB Service assignment</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>DB Service installation/deletion</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>DB Service registration/deregistration</td><td>O</td><td>X</td></tr><tr><td>DB management</td><td><ul><li>Spec change</li><li>Tablespace management</li><li>Parameter management</li><li>Backup/Recovery</li><li>Monitoring</li></ul></td><td>O</td><td>O</td></tr></tbody></table>

---

## View account list <a href="#account-list" id="account-list"></a>

**Account Management** When you enter the menu, you can view the list of all accounts registered in OwlDB. You can filter by account status, or search by ID, name, or email.

**Account status**

| Account status | Description |
| --- | --- |
| Active | Active account |
| Inactive | Deactivated account |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account management tasks <a href="#account-management-tasks" id="account-management-tasks"></a>

## Create account <a href="#create-account" id="create-account"></a>

You can directly create a new Member account.

1. **Account Management** In the menu, **Create** Click the button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions.
4. **Create** Clicking the button completes the account creation.

Once the account is created, a permission grant notification is sent to the corresponding Member.

---

## View and edit account details <a href="#account-details" id="account-details"></a>

Clicking a user ID in the account list lets you view the detailed information for that account. You can view and edit the detailed information of all accounts, including your own.

The items you can view are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modification date

To edit, on the detail page, **Edit** Click the button.

---

## Approve account creation request <a href="#approve-account-request" id="approve-account-request"></a>

When a user without an account requests account creation, the request record is **Account Management** in the menu `Account Requested` It is displayed with the status.

When Root changes the account `Active` to the status, the account is activated.

---

## Delete account <a href="#delete-account" id="delete-account"></a>

1. **Account Management** Select the account to delete from the menu.
2. **Delete** Click the button.
3. In the confirmation modal, **Delete** Clicking the button immediately blocks that account's access.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing task history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
