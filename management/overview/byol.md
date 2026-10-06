Through license rebuild and renewal, the license configuration of a DB service operating in BYOL mode is maintained and replaced.

**Rebuild**is a feature that, when the license configuration falls short of the initial setting, creates additional instances to make up the shortfall and restores the original configuration. **License Renewal**is a feature for replacing a license by uploading a new license file starting from 90 days before expiration, and it applies only to BYOL licenses. Both features can be performed by users with Root and Member permissions. **Management > Overview > License** tab.

# License Rebuild <a href="#rebuild-license" id="rebuild-license"></a>

Rebuild can only be performed when an idle license exists.

1. **Management > Overview**In **License** Click the tab.
2. **Rebuild** Click the button.
3. In the confirmation modal, compare the number of currently operating instances with the final configuration after rebuild.
4. **Confirm** Click the button.

## Rebuild Behavior by License Type <a href="#rebuild-by-license-type" id="rebuild-by-license-type"></a>

<table><thead><tr><th>License Type</th><th>Recovery Details</th></tr></thead><tbody><tr><td>Single</td><td>Create an additional Standby instance and assign it as a Recovery Standby</td></tr><tr><td>TSC</td><td>Create an additional Standby instance and assign it as a Read Only Standby</td></tr><tr><td>TAC</td><td><ul><li><strong>Primary node count mismatch</strong>: Create additional Primary nodes and assign</li><li><strong>Match</strong>: Create additional Standby nodes and assign</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- While the rebuild is in progress, the service status is displayed as `updating`.
- Rebuild results are determined on a per-instance basis, and even if some instances fail, you can retry using the Rebuild button.
{% endhint %}

# License Renewal <a href="#renew-license" id="renew-license"></a>

When the BYOL license has 90 days or fewer remaining until its expiration date, an expiration notice banner is displayed on the Overview page, and you can renew by uploading a new license file. On the renewal screen, basic information such as service name, DB engine, license options, topology, and node count is displayed as read-only.

1. On the banner, the **Renew** link or **License Renewal** Click the button.
2. **Upload** button, or drag and drop the file to upload the license file.
3. The file to validate **Check**it.
4. **Validate** Click the button to verify the validity of the license file.
5. After successful validation, **Renew** Click the button.

## License File Upload and Validation <a href="#upload-license-file" id="upload-license-file"></a>

For the uploaded file, you can check the following information in the list.

| Item | Description |
| --- | --- |
| License file | File name |
| Edition | Edition information |
| CSP | CSP information |
| Limit CPU | Number of vCPUs allowed by the license |
| Expired Date | License expiration date |
| Signature | Signature information |
