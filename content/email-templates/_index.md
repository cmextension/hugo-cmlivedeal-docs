---
title: Email templates
weight: 45
---

CM Live Deal sends three emails that you can write yourself. Go to `Components > CM Live Deal > Email Templates`.

![/images/email-templates-4-0-list.webp](/images/email-templates-4-0-list.webp)

| Template | Sent to | When |
| --- | --- | --- |
| **Coupon code for your order {order_number}** | The buyer, at the email address typed at checkout | An order is paid and its coupon is created |
| **Your deal {deal} ends soon** | The merchant | Before their deal ends, by the [Expiry reminders task](/scheduled-tasks/#expiry-reminders) |
| **Your coupon for {deal} expires soon** | The customer | Before an unredeemed coupon expires, by the [Expiry reminders task](/scheduled-tasks/#expiry-reminders) |

The two reminder templates are sent only when you have created the Expiry reminders task. See [Scheduled tasks](/scheduled-tasks/).

All three templates come with a ready-made design: a card with your site name at the top and, in the order and coupon emails, the coupon code in a large box. You can use them as they are, change the words, or write your own.

## Edit a template

Click the subject of a template.

![/images/email-templates-4-0-edit.webp](/images/email-templates-4-0-edit.webp)

* **Email Subject**: the subject line.
* **Email Body**: the email itself. It is sent as HTML.

The tags you can use are listed next to the editor. A tag is replaced with its value when the email is sent. Tags work in the subject and in the body.

The editor shows the design the way it will look, so you can click into the text and change the words. To change the layout, click **Toggle Editor** and edit the HTML.

When you write your own design, remember that email programs are not web browsers:

* Use inline `style` attributes. Many email programs remove a `<style>` block, and Joomla removes it when you save, unless you are a Super User.
* Lay the email out with tables. Many email programs do not support flexbox or CSS grid.

### Start from the sample

Click **Sample** under the editor. A window shows the ready-made design of this template as HTML.

![/images/email-templates-4-0-sample.webp](/images/email-templates-4-0-sample.webp)

1. Click **Toggle Editor** first, so the editor shows HTML. If you paste into the editor while it shows the design, it can rewrite the HTML and break the design.
2. Click **Copy** in the window, then close it.
3. Paste into the editor, replacing what was there, and click `Save`.

This is how you get the new design back after you changed a template, or after an upgrade from CM Live Deal 3 kept your old order email.

### Preview

Click **Preview** under the editor. A new browser tab shows the email as it is in the editor now, with the subject above it. You do not have to save first.

![/images/email-templates-4-0-preview.webp](/images/email-templates-4-0-preview.webp)

The tags are filled in with example details, such as the customer `Anna Nguyen`, the merchant `Corner Bistro` and the coupon code `KXQRT`. The site name is your real site name. If the editor is empty, Preview shows the sample.

Preview needs the same permission as editing the template: **Edit** on CM Live Deal.

You cannot add or delete templates. Each template has one text for the whole site, so write it in the language of your customers.

All emails are sent from the address and name in `System > Global Configuration > Server > Mail`.

## Tags

Dates use the long date format of the site language, for example `Thursday, 17 September 2026 12:48`.

### Coupon code for your order {order_number}

| Tag | Value |
| --- | --- |
| `{first_name}` | Buyer's first name |
| `{last_name}` | Buyer's last name |
| `{created}` | Date the order was created |
| `{completed}` | Date the order was paid |
| `{amount}` | Order amount, with the currency |
| `{order_number}` | Order number |
| `{deal}` | Deal's name |
| `{merchant}` | Merchant's name |
| `{coupon}` | Coupon code |
| `{payment_method}` | Payment method, for example `PayPal` |
| `{transaction_id}` | Transaction ID of the payment service |
| `{expired}` | Deal's ending time |
| `{sitename}` | Site name from Global Configuration |

This email is sent only for deals that customers pay for with [Advance payment](/configuration/advance-payment/). A free coupon has no order, so no email is sent.

To attach the coupon as a PDF file, turn on **Attach PDF coupon to order confirmation email** in the Advance Payment options. See [PDF coupon](/pdf-coupon/).

### Your deal {deal} ends soon

| Tag | Value |
| --- | --- |
| `{merchant}` | Merchant's business name, or the user's name if the merchant has no business name |
| `{deal}` | Deal's name |
| `{expired}` | Deal's ending time |
| `{sitename}` | Site name from Global Configuration |

### Your coupon for {deal} expires soon

| Tag | Value |
| --- | --- |
| `{name}` | Customer's name |
| `{deal}` | Deal's name |
| `{merchant}` | Merchant's name |
| `{coupon}` | Coupon code |
| `{expired}` | Deal's ending time |
| `{sitename}` | Site name from Global Configuration |

## Other emails

CM Live Deal sends three more emails. They are not templates. Turn each one on or off in `Components > CM Live Deal > Options`:

| Email | Option | Sent to |
| --- | --- | --- |
| A new deal was submitted | **New deal notification**, Deal tab | Every user with **Receive System Emails** set to `Yes` |
| A new merchant registered | **New merchant notification**, Merchant tab | Every user with **Receive System Emails** set to `Yes` |
| A customer captured a coupon | **New coupon notification**, Merchant tab | The merchant of the deal |

Their text comes from language strings. To change it, go to `System > Manage > Language Overrides`, choose your language with **Site** and click `New`:

| Email | Subject key | Body key |
| --- | --- | --- |
| New deal | `COM_CMLIVEDEAL_EMAIL_NEW_DEAL_SUBJECT` | `COM_CMLIVEDEAL_EMAIL_NEW_DEAL_BODY` |
| New merchant | `COM_CMLIVEDEAL_EMAIL_NEW_MERCHANT_SUBJECT` | `COM_CMLIVEDEAL_EMAIL_NEW_MERCHANT_BODY` |
| New coupon | `COM_CMLIVEDEAL_NEW_MERCHANT_COUPON_SUBJECT` | `COM_CMLIVEDEAL_NEW_MERCHANT_COUPON_EMAIL_BODY` |

Each `%s` is replaced with a value, in order. Keep the same number of `%s`. For example, the new coupon email gets the merchant's name, the deal, the site name and the coupon code:

```
COM_CMLIVEDEAL_NEW_MERCHANT_COUPON_EMAIL_BODY="Hi %s,\n\nGood news: someone just captured a coupon for \"%s\" on %s. The code is %s."
```
