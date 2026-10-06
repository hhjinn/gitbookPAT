In the Account Management menu, you can view and manage all accounts registered in OwlDB.

{% hint style="info" %}
**Note**

The Account Management menu is accessible only to Root.
{% endhint %}

## Account role guide <a href="#account-roles" id="account-roles"></a>

### Root <a href="#root" id="root"></a>

The Root account can create, view, and modify Member accounts, and can grant or revoke access permissions for specific DB Services. It can also create and delete DB Services.

When a user without an account (an applicant) requests account creation, the request is delivered to Root, and once Root approves it, the account is activated.

### Member <a href="#member" id="member"></a>

A Member can use the operation and management functions provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services to which they have been granted access by Root. A user without an account (an applicant) can request the creation of their own account from Root.

### Permission matrix by role <a href="#undefined" id="undefined"></a>

<table><thead><tr><th>Function category</th><th>Detailed function</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account management</td><td>Account creation and deletion</td><td>✓</td><td>—</td></tr><tr><td>Permission management</td><td>DB Service assignment</td><td>✓</td><td>—</td></tr><tr><td>Service management</td><td>DB Service creation and deletion</td><td>✓</td><td>—</td></tr><tr><td>DB management</td><td><ul><li>Spec Change</li><li>Tablespace management</li><li>Parameter management</li><li>Backup/recovery</li><li>Monitoring</li></ul></td><td>✓</td><td>✓</td></tr></tbody></table>

---

## Account list lookup <a href="#account-list" id="account-list"></a>

**Account management** When you enter the menu, you can view the list of all accounts registered in OwlDB. You can filter by account status or search by ID, name, or email.

**Account status**

| Account status | Description |
| --- | --- |
| Active | Active account |
| Inactive | Deactivated account |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account management tasks <a href="#account-management-tasks" id="account-management-tasks"></a>

## Account creation <a href="#create-account" id="create-account"></a>

You can directly create a new Member account.

1. **Account management** In the menu **Create** Click the button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions.
4. **Create** Click the button to complete the account creation.

When the account is created, a permission grant notification is sent to the corresponding Member.

---

## Account detail lookup and editing <a href="#account-details" id="account-details"></a>

Clicking a user ID in the account list lets you view the detailed information of that account. You can view and edit the detailed information of all accounts, including your own.

The items that can be viewed are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modified date

To edit, on the detail page **Edit** Click the button.

---

## Approval of account creation request <a href="#approve-account-request" id="approve-account-request"></a>

When a user without an account requests account creation, the request record is **Account management** in the menu `Account Requested` displayed in the status.

When Root changes the account `Active` to the status, the account is activated.

---

## Account deletion <a href="#delete-account" id="delete-account"></a>

1. **Account management** Select the account to delete from the menu.
2. **Delete** Click the button.
3. In the confirmation modal **Delete** Clicking the button immediately blocks access for that account.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing task history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
