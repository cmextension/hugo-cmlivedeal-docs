---
title: Email templates
weight: 45
---

CM Live Deal sends three emails that you can write yourself. Go to `Components > CM Live Deal > Email Templates`.

![/images/email-templates-4-0-list.png](/images/email-templates-4-0-list.png)

| Template | Sent to | When |
| --- | --- | --- |
| **Coupon code for your order {order_number}** | The buyer, at the email address typed at checkout | An order is paid and its coupon is created |
| **Your deal {deal} ends soon** | The merchant | Before their deal ends, by the [Expiry reminders task](/scheduled-tasks/#expiry-reminders) |
| **Your coupon for {deal} expires soon** | The customer | Before an unredeemed coupon expires, by the [Expiry reminders task](/scheduled-tasks/#expiry-reminders) |

The two reminder templates are sent only when you have created the Expiry reminders task. See [Scheduled tasks](/scheduled-tasks/).

## Edit a template

Click the subject of a template.

![/images/email-templates-4-0-edit.png](/images/email-templates-4-0-edit.png)

* **Email Subject**: the subject line.
* **Email Body**: the email itself. It is sent as HTML, so you can use bold text, links and lists.

The tags you can use are listed next to the editor. A tag is replaced with its value when the email is sent. Tags work in the subject and in the body.

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
