In **My Page > Manage My Information**, you can view and manage the basic profile and permission status of the currently logged-in account.

- **Root account-specific information:** For the `Root` account only, the **Subscription Information (Subscription ID)** and **Resource Groups** items linked with the Azure console are additionally displayed on the screen. (Excluding regular Member accounts)

## View My Account Information <a href="#view-my-account" id="view-my-account"></a>

View the detailed information of your own account.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>ID</td><td>The ID used when logging in</td></tr><tr><td>Name</td><td>User display name</td></tr><tr><td>Role</td><td><code>Root</code> or <code>Member</code></td></tr><tr><td>Permission</td><td>The permissions the account has (DB Service list)</td></tr><tr><td>Email</td><td><ul><li>Display email information</li><li>Used when finding ID or resetting password</li><li>Root: CSP account information</li></ul></td></tr><tr><td>Status</td><td>Account status</td></tr><tr><td>Creation Date</td><td>Account creation time</td></tr><tr><td>Last access date</td><td>Last login time</td></tr><tr><td>Change date</td><td>Last modification time</td></tr><tr><td>Subscription information</td><td>Retrieves <code>Subscription</code> information within the Azure resource ID item (Root account only)</td></tr><tr><td>Resource group</td><td><code>resourceGroups</code> information within the Azure resource ID item (Root account only)</td></tr></tbody></table>

## Edit my account information <a href="#edit-my-account" id="edit-my-account"></a>

Edit the detailed information of your own account. ID, role, and permissions are displayed for information purposes only and cannot be edited.

| Item | Description | Input rules |
| --- | --- | --- |
| ID | The ID used when logging in | Cannot edit |
| Role | `Root` or `Member` | Cannot edit |
| Permission | The permissions the account has (DB Service list) | Cannot edit |
| Name | User display name | Can edit |
| Current password* | Previously set password | Enter for identity verification |
| New password* | Password to change | 8–20 characters, combination of letters, numbers, and special characters |
| Confirm password* | Confirm the password to change | Enter the same value as the new password |
| Email | Email information | Can edit |

The * mark indicates a required input item.

{% hint style="info" %}
**Note**

- To maintain account security, we recommend changing your password periodically.
- The new password cannot be set to be the same as a previously used password.
{% endhint %}
