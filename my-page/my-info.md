**My Page > Manage My Information**Here, you can view and manage the basic profile and permission status of the currently logged-in account.

- **Root account-specific information:** `Root` For the account only, linked with the Azure console, **Subscription ID** and **Resource Groups** items are additionally shown on the screen. (Excluding general Member accounts)

## View my account information <a href="#view-my-account" id="view-my-account"></a>

View the detailed information of your own account.

<table><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>ID</td><td>ID used when logging in</td></tr><tr><td>Name</td><td>User display name</td></tr><tr><td>Role</td><td><code>Root</code> or <code>Member</code></td></tr><tr><td>Permissions</td><td>Permissions held by the account (DB Service list)</td></tr><tr><td>Email</td><td><ul><li>Email information display</li><li>Used when finding ID and resetting password</li><li>Root: CSP account information</li></ul></td></tr><tr><td>Status</td><td>Account status</td></tr><tr><td>Creation date</td><td>Account creation time</td></tr><tr><td>Last access date</td><td>Last login time</td></tr><tr><td>Modified date</td><td>Last modified time</td></tr><tr><td>Subscription information</td><td>Within the Azure resource ID item <code>Subscription</code> Information lookup (Root account only)</td></tr><tr><td>Resource group</td><td>Within the Azure resource ID item <code>resourceGroups</code> Information (Root account only)</td></tr></tbody></table>

## Edit my account information <a href="#edit-my-account" id="edit-my-account"></a>

Edit the detailed information of your own account. The ID, role, and permissions are displayed for informational purposes only and cannot be modified.

| Item | Description | Input rules |
| --- | --- | --- |
| ID | ID used when logging in | Not editable |
| Role | `Root` or `Member` | Not editable |
| Permissions | Permissions held by the account (DB Service list) | Not editable |
| Name | User display name | Editable |
| Current password* | Previously set password | Input for identity verification |
| New password* | Password to change | 8–20 characters, combination of letters, numbers, and special characters |
| Confirm password* | Confirm the password to change | Enter the same value as the new password |
| Email | Email information | Editable |

The * mark indicates a required input field.

{% hint style="info" %}
**Note**

- To maintain account security, we recommend changing your password periodically.
- The new password cannot be set to a value that duplicates a previously used password.
{% endhint %}
