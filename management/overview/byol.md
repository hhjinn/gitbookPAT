Through license rebuilding and renewal, you maintain and replace the license configuration of a DB service operating in the BYOL method.

**Rebuilding** is a feature that restores the original configuration by creating additional missing instances when the license configuration falls short of the initial settings. **License renewal** is a feature that replaces the license by uploading a new license file starting from 90 days before expiration, and it applies only to BYOL licenses. Both features can be performed by users with Root and Member permissions on the **Management > Overview > License** tab.

# License Rebuilding <a href="#rebuild-license" id="rebuild-license"></a>

Rebuilding can only be performed when an idle license exists.

1. In **Management > Overview**, click the **License** tab.
2. Click the **Rebuild** button.
3. In the confirmation modal, compare the number of currently operating instances with the final configuration after rebuilding.
4. Click the **Confirm** button.

## Rebuilding Behavior by License Type <a href="#rebuild-by-license-type" id="rebuild-by-license-type"></a>

<table><thead><tr><th>License Type</th><th>Recovery Details</th></tr></thead><tbody><tr><td>Single</td><td>After creating an additional Standby instance, assign it as Recovery Standby</td></tr><tr><td>TSC</td><td>After creating an additional Standby instance, assign it as Read Only Standby</td></tr><tr><td>TAC</td><td><ul><li><strong>Primary node count mismatch</strong>: Create additionally as Primary and then assign</li><li><strong>Match</strong>: Create additionally as Standby and then assign</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- While rebuilding is in progress, the service status is displayed as `updating`.
- The rebuilding result is determined on a per-instance basis, and even if some instances fail, you can retry through the Rebuild button.
{% endhint %}

# License Renewal <a href="#renew-license" id="renew-license"></a>

When the expiration date of a BYOL license is within 90 days, an expiration notice banner is displayed on the Overview page, and you can renew by uploading a new license file. On the renewal screen, basic information such as the service name, DB engine, license option, topology, and number of nodes is displayed as read-only.

1. Click the **Renew** link in the banner or the **License Renewal** button.
2. Upload the license file by clicking the **Upload** button or by dragging and dropping the file.
3. **Check** the file to be validated.
4. Click the **Validate** button to verify the validity of the license file.
5. After successful validation, click the **Renew** button.

## License File Upload and Validation <a href="#upload-license-file" id="upload-license-file"></a>

You can check the following information for the uploaded file in the list.

| Item | Description |
| --- | --- |
| License File | File name |
| Edition | Edition information |
| CSP | CSP information |
| Limit CPU | Number of vCPUs allowed by the license |
| Expired Date | License expiration date |
| Signature | Signature information |
