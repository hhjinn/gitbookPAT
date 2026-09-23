In the Account Management menu, you can view and manage all accounts registered in OwlDB.

{% hint style="info" %}
**Note**

Only Root can access the Account Management menu.
{% endhint %}

## Account Role Guide

### Root

The Root account can create, view, and modify Member accounts, and can grant or revoke access permissions for specific DB Services. It can also create and delete DB Services.

When a user without an account (a sign-up applicant) requests account creation, the request is forwarded to Root, and the account is activated once Root approves it.

### Member

A Member can use the operation and management features provided by OwlDB (monitoring, backup/recovery, parameter management, etc.) only for the DB Services for which access permission has been granted by Root. A user without an account (a sign-up applicant) can request the creation of their own account from Root.

### Permission Matrix by Role

<table data-full-width="true"><thead><tr><th>Feature Category</th><th>Detailed Feature</th><th>Root</th><th>Member</th></tr></thead><tbody><tr><td>Account Management</td><td>Account Creation and Deletion</td><td>✓</td><td>—</td></tr><tr><td>Permission Management</td><td>DB Service Assignment</td><td>✓</td><td>—</td></tr><tr><td>Service Management</td><td>DB Service Creation and Deletion</td><td>✓</td><td>—</td></tr><tr><td>DB Management</td><td><ul><li>Spec Change</li><li>Tablespace Management</li><li>Parameter Management</li><li>Backup/Recovery</li><li>Monitoring</li></ul></td><td>✓</td><td>✓</td></tr></tbody></table>

---

## Account List View

**Account Management** When you enter the menu, you can view the list of all accounts registered in OwlDB. You can filter by account status or search by ID, name, or email.

**Account Status**

| Account Status | Description |
| --- | --- |
| Active | Normal account |
| Inactive | Account that has been deactivated |
| Account Requested | Account awaiting administrator approval |
| Deleted | Deleted account |

---

# Account Management Tasks

## Account Creation

You can directly create a new Member account.

1. **Account Management** In the menu, **Create** Click the button.
2. Enter the account information (ID, name, password, etc.).
3. If necessary, you can also grant DB Service access permissions.
4. **Create** Clicking the button completes the account creation.

Once the account is created, a permission grant notification is sent to the Member.

---

## Account Detail View and Modification

Clicking a user ID in the account list allows you to view the detailed information of that account. You can view and modify the detailed information of all accounts, including your own.

The items that can be viewed are as follows.

- ID, name, role, email
- Granted DB Service permissions
- Account status, creation date, last access date, modification date

To modify, on the detail page **Edit** Click the button.

---

## Account Creation Request Approval

When a user without an account requests account creation, the request record appears in the **Account Management** menu with the `Account Requested` status.

When Root changes the account to `Active` status, the account is activated.

---

## Account Deletion

1. **Account Management** Select the account to delete from the menu.
2. **Delete** Click the button.
3. In the confirmation modal, **Delete** Clicking the button immediately blocks access for the corresponding account.

{% hint style="info" %}
**Note**

- The ID of a deleted account cannot be reused. However, existing task history and logs are retained.
- The Root account cannot be deleted.
{% endhint %}
