---
title: Privacy
weight: 228
---

CM Live Deal works with Joomla's **Privacy** component. When a person asks what data your site holds about them, or asks you to remove it, Joomla also includes the data CM Live Deal holds. This helps you answer requests under laws like the GDPR.

The package enables the plugin **Privacy - CM Live Deal** when it installs it. You do not need to set anything.

## What CM Live Deal stores

Go to `Users > Privacy > Capabilities` and open **Privacy - CM Live Deal**. Use this list when you write your site's privacy policy.

![/images/privacy-4-0-capabilities.png](/images/privacy-4-0-capabilities.png)

* **Orders**: the name, email address and postal address entered at checkout, and the amount, payment method and transaction ID of the payment service.
* **Coupons**: the coupons a person captured, and when they captured and redeemed them.
* **Merchant profile**: the business name, description, address, coordinates, phone number, website and social media links.
* **Deals**: the deals a merchant published, with their custom fields.
* **Images**: the images a merchant uploaded for their deals.

## Handle a request

A person sends a request with Joomla's request form on the site, or you add one in `Users > Privacy > Requests`. The request must be **Confirmed** before you can act on it. Joomla emails the person a link to confirm it.

Open the request. The buttons depend on the request type.

![/images/privacy-4-0-export-request.png](/images/privacy-4-0-export-request.png)

* **Export**: click `Export Data` to download an XML file, or `Email Data Export` to send it to the person.
* **Remove**: click `Delete Data`.

When you are done, click `Complete`.

See Joomla's own help for the request form, the emails and the other privacy plugins.

## What an export holds

CM Live Deal adds these sections to the XML file:

* `cmlivedeal_orders`: the person's orders. An order is found by the account that placed it **or** by the email address typed at checkout. So an order placed before the person registered, or with an email address they later changed, is still found.
* `cmlivedeal_coupons`: the person's coupons.
* `cmlivedeal_merchant`: the person's merchant profile.
* `cmlivedeal_deals`: the deals the person published.
* `cmlivedeal_deal_custom_fields`: the custom field values of those deals.
* `cmlivedeal_images`: the images the person uploaded.

Orders and coupons include the deal title, so the person can see which deal each one is for. The columns that name your staff (`modified_by`, `checked_out`, `checked_out_time`) are left out.

A request for an email address with no account only finds orders. Everything else needs an account.

Part of an export looks like this:

```xml
<domain name="cmlivedeal_coupons" description="cmlivedeal_coupons_data">
  <item id="2">
    <id>2</id>
    <code>JBBEX</code>
    <user_id>426</user_id>
    <deal_id>9</deal_id>
    <redeemed>0</redeemed>
    <redeemed_time>0000-00-00 00:00:00</redeemed_time>
    <created>2022-06-11 02:28:41</created>
    <created_by>426</created_by>
    <deal_title>Sample Deal 09</deal_title>
  </item>
</domain>
```

## What removal does

CM Live Deal does not delete everything. Some records must stay so your site and your accounts still work.

| Data | What happens |
| --- | --- |
| Orders | The order stays. The name, email address and postal address are cleared. The amount, status, payment method and transaction ID stay, because they are your record of a real payment. |
| Coupons | The coupon stays, but it no longer belongs to the account. The code still works, so a customer who already has it can still redeem it, and the merchant keeps the redemption record. |
| Merchant profile | Deleted. |
| Images | Deleted, both the database rows and the files, including the resized copies. |
| Deals | Not deleted. See below. |

Joomla's own **Privacy - User Accounts** plugin handles the account itself: it replaces the name, username and email address and blocks the account.

### A merchant who still has deals

Deals are public content, and orders and coupons belong to them. Only you can decide what happens to those. So CM Live Deal refuses to remove a merchant who still has deals, and nothing is removed:

![/images/privacy-4-0-remove-refused.png](/images/privacy-4-0-remove-refused.png)

To finish the request:

1. Go to `Components > CM Live Deal > Deals` and choose the merchant in `Select a merchant`.
2. Delete the deals, or open each one and choose another merchant.
3. Open the request again and click `Delete Data`.

Joomla also refuses to remove a Super User.
