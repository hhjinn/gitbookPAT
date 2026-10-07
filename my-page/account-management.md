In the Account Management menu, you can view and manage all accounts registered in OwlDB.

{% hint style="info" %}
**Note**

Only Root can access the Account Management menu.
{% endhint %}

## Account role guide <a href="#account-roles" id="account-roles"></a>

### Root <a href="#root" id="root"></a>

A Root account can create, view, and edit Member accounts, and can grant or revoke access permissions for specific DB Services. It can also use the database installation, DB Service registration, and infrastructure discovery features.

When a user without an account (a sign-up applicant) requests account creation, the request is forwarded to Root, and once Root approves it, the account is activated.

### Member <a href="#member" id="member"></a>

A Member can use the operation and management features provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services to which Root has granted them access. A user without an account (a sign-up applicant) can request the creation of their own account from Root.

### Permission matrix by role <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Feature category</th><th>Detailed feature</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account management</td><td>Create and delete users</td><td>O</td><td>X</td></tr><tr><td>Permission management</td><td>DB Service assignment</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>Install/delete DB Service</td><td>O</td><td>X</td></tr><tr><td>Service management</td><td>Register/unregister DB Service</td><td>O</td><td>X</td></tr><tr><td>DB management</td><td><ul><li>Spec Change</li><li>Tablespace management</li><li>Parameter management</li><li>Backup/recovery</li><li>Monitoring</li></ul></td><td>O</td><td>O</td></tr></tbody></table>

---

## View account list <a href="#account-list" id="account-list"></a>

When you enter the **Account Management** menu, you can view the list of all accounts registered in OwlDB. You can filter by account status, or search by ID, name, or email.

**Account status**

| Account status | Description |
| --- | --- |
| Active | Normal account |
| Inactive | Deactivated account |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account management tasks <a href="#account-management-tasks" id="account-management-tasks"></a>

## Create account <a href="#create-account" id="create-account"></a>

You can directly create a new Member account.

1. In the **Account Management** menu, click the **Create** button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions at the same time.
4. Clicking the **Create** button completes the account creation.

When the account is created, a permission grant notification is sent to that Member.

---

## Viewing and Editing Account Details <a href="#account-details" id="account-details"></a>

Clicking a user ID in the account list lets you view the detailed information of that account. You can view and edit the detailed information of all accounts, including your own.

The items you can view are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modification date

To edit, click the **Edit** button on the detail page.

---

## Approving Account Creation Requests <a href="#approve-account-request" id="approve-account-request"></a>

When a user without an account requests account creation, the request appears in the **Account Management** menu with the `Account Requested` status.

When Root changes the account to the `Active` status, the account is activated.

---

## Deleting an Account <a href="#delete-account" id="delete-account"></a>

1. In the **Account Management** menu, select the account to delete.
2. Click the **Delete** button.
3. In the confirmation modal, clicking the **Delete** button immediately blocks access for that account.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing work history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
