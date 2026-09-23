**My Page > My Account Management**View and manage the basic profile and permission status of the currently logged-in account.

- **Root Account-Specific Information:** `Root` For this account only, linked to the Azure console, **Subscription ID** and **Resource Groups** items are additionally displayed on the screen. (Excluding general Member accounts)

## View My Account Information

View the detailed information of your account.

<table data-full-width="true"><thead><tr><th>Item</th><th>Description</th></tr></thead><tbody><tr><td>ID</td><td>ID used when logging in</td></tr><tr><td>Name</td><td>User display name</td></tr><tr><td>Role</td><td><code>Root</code> or <code>Member</code></td></tr><tr><td>Permission</td><td>Permissions held by the account (DB Service list)</td></tr><tr><td>Email</td><td><ul><li>Email information display</li><li>Used when finding ID and resetting password</li><li>Root : CSP account information</li></ul></td></tr><tr><td>Status</td><td>Account status</td></tr><tr><td>Created Date</td><td>Account creation time</td></tr><tr><td>Last Access Date</td><td>Last login time</td></tr><tr><td>Modified Date</td><td>Last modification time</td></tr><tr><td>Subscription Information</td><td>Within the Azure Resource ID item, <code>Subscription</code> information lookup (Root account only)</td></tr><tr><td>Resource Groups</td><td>Within the Azure Resource ID item, <code>resourceGroups</code> information (Root account only)</td></tr></tbody></table>

## Edit My Account Information

Edit the detailed information of your account. ID, Role, and Permission are displayed for information confirmation only and cannot be edited.

| Item | Description | Input Rules |
| --- | --- | --- |
| ID | ID used when logging in | Cannot be edited |
| Role | `Root` or `Member` | Cannot be edited |
| Permission | Permissions held by the account (DB Service list) | Cannot be edited |
| Name | User display name | Editable |
| Current Password* | Previously set password | Input for identity verification |
| New Password* | Password to change | 8-20 characters, combination of letters, numbers, and special characters |
| Confirm Password* | Confirm the password to change | Enter the same as the new password |
| Email | Email information | Editable |

*표기는 필수 입력 항목을 의미합니다.

The * mark indicates required input items.

{% hint style="info" %}
**Note**

- Periodic password changes are recommended to maintain account security.
- The new password cannot be set to be the same as a previously used password.
{% endhint %}
