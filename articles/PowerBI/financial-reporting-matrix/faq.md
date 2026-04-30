# FAQ — Financial Reporting Matrix

Common questions answered. For bug reports, feature requests, or anything not covered below, see [Get help](getting-started/get-help.md). For deeper feature documentation, see the rest of this section.

## Licensing

**Q: How many licenses do we need?**

A: Our licensing policy requires one license for each user. All users that view or interact with the reports need a license.

If you have 1 developer and 9 viewers, you need 10 licenses.

**Q: Do I need a license for those who will only view the report?**

A: Yes — the licensing terms do not differentiate on the type of user.

**Q: I'm a single developer, and my colleagues only want to see the visual and export to Excel. Do all of us need licenses?**

A: Yes. You need a license for everyone who wants to view and interact with the visual.

**Q: Can we get a free trial?**

A: Yes. By completing the [checkout](https://appsource.microsoft.com/en-us/marketplace/checkout/WA200000642?tab=Overview) for any plan up to 20 users, you automatically get 1 month free trial. If you cancel the subscription before the trial expires, you will not be charged.

**Q: How can we purchase licenses?**

A: The main channel is [AppSource](https://appsource.microsoft.com/en-us/product/power-bi-visuals/WA200000642?tab=Overview), where you can get the exact number of licenses you want, starting at 1 license. Licenses purchased through AppSource are paid by credit card.

**Q: How can we assign licenses?**

A: Once purchased, licenses are assigned through the Microsoft Admin Center the same way normal Office licenses are assigned. [This Microsoft article](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users?view=o365-worldwide#assign-licenses-by-using-the-licenses-page) explains how to assign licenses to a product in Microsoft 365 Admin Center.

**Q: Can we use the visual in Power BI Embedded and Power BI Report Server?**

A: Yes, but in that case you must contact us at [pbi.visuals@profitbase.com](mailto:pbi.visuals@profitbase.com) to get a specific license for that usage. Licenses purchased through AppSource do not cover Embedded or Report Server.

**Q: Can we get licenses through a reseller?**

A: Yes, but only if the reseller contacts us at [pbi.visuals@profitbase.com](mailto:pbi.visuals@profitbase.com). The minimum purchase through resellers is €2100 (corresponds to 50 users).

**Q: Why do I get an error in AppSource?**

A: For AppSource-related errors, you'll need to reach out to Microsoft support — Profitbase doesn't have access to the AppSource platform itself. See the [AppSource issues](#appsource-issues) section below for the contact procedure, or use the [guide on how to contact Microsoft regarding AppSource issues](https://github.com/Profitbase/PowerBI-visuals-FinancialReportingMatrix/discussions/609).

### Limitations on AppSource

- Power BI Embedded capacities and Power BI Report Server (on-prem) do not support licenses bought through AppSource. For those use cases, contact us at [pbi.visuals@profitbase.com](mailto:pbi.visuals@profitbase.com).
- It's not possible to change ownership of the subscription once purchased — we recommend using a shared user for the initial purchase.
- For issues related to license assignment or payment, contact Microsoft directly. Profitbase doesn't have access to their platform. Register a ticket from the [Admin Center](https://admin.microsoft.com/Adminportal/Home#/homepage) → "Help and support" in the bottom right corner.
- It's not possible to upgrade plans. If you purchase the 1–5 plan and later want a 6th license, you need to purchase another plan in addition to the one you have.
- Licenses cannot be assigned to users in different tenants. They can only be assigned within the same tenant where they were purchased.
- There is currently no way for resellers to purchase licenses through AppSource.
- There is currently no way to change or upgrade to a larger subscription tier through AppSource.

---

## AppSource issues

We offer our visuals through the AppSource platform, but we don't have access to it ourselves when it comes to troubleshooting issues or managing subscriptions. AppSource issues include:

- Error messages when trying to make a purchase
- Issues assigning licenses
- Issues cancelling licenses

For all AppSource-related issues, you need to get in touch with Microsoft.

### How to contact Microsoft about AppSource issues

**1. Go to the [Admin Center](https://admin.microsoft.com/Adminportal/Home#/licenses), and click "Help and Support" in the bottom right corner:**

![Admin Center help and support button](https://github.com/Profitbase/PowerBI-visuals-FinancialReportingMatrix/assets/82056309/76585b09-dfe7-4222-916e-35f12372cad4)

**2. Write some keywords for your issue, and click "Contact Support":**

![Contact support keyword search](https://github.com/Profitbase/PowerBI-visuals-FinancialReportingMatrix/assets/82056309/a0fa9d5a-e2ac-4b60-82a4-cba2fb8603ca)

**3. Then fill in the details, and upload a screenshot of the error message if you have one:**

![Support ticket details form](https://github.com/Profitbase/PowerBI-visuals-FinancialReportingMatrix/assets/82056309/6f23fb80-9692-48d1-9694-422243cb3ab1)

---

## Power BI tenant settings

**Q: I have the Financial Reporting Matrix license, but when I try to export to Excel, I get the error "Download from custom visual needs to be enabled by a Power BI administrator." How do I fix this?**

A: A Power BI administrator needs to enable "Export data from custom visuals" in the Admin Portal. Without this setting enabled, export will not be possible:

![Power BI admin portal export setting](https://user-images.githubusercontent.com/82056309/208896813-ed2e2622-d8bd-4c58-b715-259b01756dca.png)

More details in Microsoft's Power BI documentation: [Export data from custom visuals](https://learn.microsoft.com/en-us/power-bi/admin/organizational-visuals#export-data-to-file).

---

## Still need help?

If your question isn't answered here, see [Get help](getting-started/get-help.md) for the canonical channels: GitHub Discussions for general questions, and the support email for license-specific or sensitive issues.
