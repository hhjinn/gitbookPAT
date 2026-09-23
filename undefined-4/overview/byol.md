Maintain and replace the license configuration of DB services operating in BYOL mode through license reconstruction and renewal.

**Reconstruction**is a feature that restores the original configuration by creating additional instances to make up for shortfalls when the license configuration falls below the initial settings. **License renewal**is a feature for replacing a license by uploading a new license file starting from 90 days before expiration, and applies only to BYOL licenses. Both features can be performed by users with Root and Member permissions **Management > Overview > License** in the tab.

# License Reconstruction

Reconstruction can only be performed when idle licenses exist.

1. **Management > Overview**In **License** Click the tab.
2. **Reconstruction** Click the button.
3. In the confirmation modal, compare the number of currently operating instances with the final configuration after reconstruction.
4. **Confirm** Click the button.

## Reconstruction Behavior by License Type

<table data-full-width="true"><thead><tr><th>License Type</th><th>Recovery Details</th></tr></thead><tbody><tr><td>Single</td><td>Create an additional Standby instance and assign it as Recovery Standby</td></tr><tr><td>TSC</td><td>Create an additional Standby instance and assign it as Read Only Standby</td></tr><tr><td>TAC</td><td><ul><li><strong>Primary node count mismatch</strong>: Create additionally as Primary and assign</li><li><strong>Match</strong>: Create additionally as Standby and assign</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Note**

- While reconstruction is in progress, the service status is displayed `updating`as.
- Reconstruction results are determined on a per-instance basis, and even if some instances fail, you can retry using the Reconstruction button.
{% endhint %}

# License Renewal

When a BYOL license has 90 days or less remaining until its expiration date, an expiration notice banner is displayed on the Overview page, and you can renew it by uploading a new license file. On the renewal screen, basic information such as service name, DB engine, license option, topology, and node count is displayed in read-only mode.

1. The banner's **Renew** link or **License Renewal** Click the button.
2. **Upload** Upload the license file by clicking the button or by dragging and dropping the file.
3. The file to be validated **Check**it.
4. **Validate** Click the button to verify the validity of the license file.
5. After successful validation **Renew** Click the button.

## License File Upload and Validation

For the uploaded file, you can check the following information in the list.

| Item | Description |
| --- | --- |
| License File | File name |
| Edition | Edition information |
| CSP | CSP information |
| Limit CPU | Number of vCPUs allowed by the license |
| Expired Date | License expiration date |
| Signature | Signature information |
