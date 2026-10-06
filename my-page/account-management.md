In the Account Management menu, you can view and manage all accounts registered in OwlDB.

{% hint style="info" %}
**Note**

Only Root can access the Account Management menu.
{% endhint %}

## Account role guide

### Root

The Root account can create, view, and edit Member accounts, and can grant or revoke access permissions for specific DB Services. It can also use the database installation, DB Service registration, and infrastructure exploration features.

When a user without an account (an applicant) requests account creation, the request is forwarded to Root, and once Root approves it, the account is activated.

### Member

A Member can use the operational and management features provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services to which they have been granted access permission by Root. A user without an account (an applicant) can request the creation of their own account from Root.

### Permission matrix by role

<table><thead><tr><th>Feature category</th><th>Detailed feature</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account Management</td><td>Create and delete users</td><td>O</td><td>X</td></tr><tr><td>Permission management</td><td>DB Service assignment</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>DB Service installation/deletion</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>DB Service registration/deregistration</td><td>O</td><td>X</td></tr><tr><td>DB management</td><td><ul><li>Spec Change</li><li>Tablespace management</li><li>Parameter management</li><li>Backup/recovery</li><li>Monitoring</li></ul></td><td>O</td><td>O</td></tr></tbody></table>

---

## View account list

**Account Management** When you enter the menu, you can view the list of all accounts registered in OwlDB. You can filter by account status or search by ID, name, or email.

**Account status**

| Account status | Description |
| --- | --- |
| Active | Normal account |
| Inactive | Deactivated account |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account management tasks

## Create account

You can create a new Member account directly.

1. **Account Management** From the menu, **Create** Click the button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions.
4. **Create** Clicking the button completes the account creation.

Once the account is created, a permission grant notification is sent to the corresponding Member.

---

## Account Details View and Edit

Clicking a user ID in the account list allows you to view the detailed information of that account. You can view and edit the detailed information of all accounts, including your own.

The items that can be viewed are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modification date

To edit, on the details page, **Edit** Click the button.

---

## Account Creation Request Approval

When a user without an account requests account creation, the request record is **Account Management** in the menu `Account Requested` displayed with the status.

When Root changes the corresponding account `Active` to the status, the account is activated.

---

## Account Deletion

1. **Account Management** Select the account to delete from the menu.
2. **Delete** Click the button.
3. In the confirmation modal, **Delete** Clicking the button immediately blocks access for the corresponding account.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing work history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
