---
title: Payment plugins
weight: 180
---

Payment plugins take the advance payment for a coupon. Turn on advance payment in [the component's options](/configuration/advance-payment/) and in [the deal form](/create-new-deal-in-back-end/).

CM Live Deal comes with two payment plugins: **PayPal - CM Live Deal** and **Stripe - CM Live Deal**. Both are installed **disabled**, because each one needs your own PayPal or Stripe account before it can take money. Set a plugin up first, then enable it.

To find the plugins, go to `System` → `Plugins` and filter by the type `cmlivedeal`.

![/images/payment-plugins-4-0-list.png](/images/payment-plugins-4-0-list.png)

Enable only the plugins you have set up. At checkout, customers choose from the enabled plugins. If only one plugin is enabled, it is selected for them.

## PayPal

The **Integration** option decides how customers pay with PayPal:

* **REST API** shows the PayPal buttons on your checkout page. The customer pays in a PayPal window and stays on your site. New sites use it, and we recommend it.
* **PayPal Payments Standard (legacy)** sends the customer to PayPal's website with a form. This is how CM Live Deal 3 took payments. PayPal treats it as legacy, so use it only until you can switch.

These options are the same for both integrations:

* **Display Name**: The name of the payment method shown at checkout. Leave it blank to use the default name, `PayPal`.
* **Integration**: `REST API` or `PayPal Payments Standard (legacy)`, see above.
* **Currency**: The currency customers pay in.
* **Environment**: Choose `Live` to accept real payments, or `Sandbox` to test with the PayPal sandbox.

### REST API

![/images/payment-plugin-4-0-paypal-rest.png](/images/payment-plugin-4-0-paypal-rest.png)

* **Live Client ID** and **Live Secret Key**, **Sandbox Client ID** and **Sandbox Secret Key**: Create a REST app at [developer.paypal.com](https://developer.paypal.com/dashboard/applications) and copy its client ID and secret key here. The app must belong to the environment you selected. Each environment has its own credentials and its own webhook.
* **Live Webhook ID** and **Sandbox Webhook ID**: The ID PayPal gives to the webhook you add to the REST app. It starts with `WH-` or is a long hexadecimal string. Without it, notifications cannot be verified as coming from PayPal and are rejected, so an order is only completed if the customer's browser returns to your site.
* **Webhook Endpoint**: In your REST app, add a webhook at `<your site>/index.php?option=com_cmlivedeal&task=checkout.webhook&gateway=paypal` and subscribe it to the events `Checkout order approved` and `Payment capture completed`. This completes the order even if the customer closes the browser before returning to your site.

### PayPal Payments Standard (legacy)

![/images/payment-plugin-4-0-paypal-standard.png](/images/payment-plugin-4-0-paypal-standard.png)

* **PayPal Email**: The email address of the PayPal account that receives the payments. A payment to any other account does not complete an order.
* **Seconds to Wait**: How many seconds the checkout page waits before it takes the customer to PayPal, from 0 to 30. The customer can also click **Go to PayPal Now**.

PayPal confirms each payment by sending a notification (IPN) to your site. CM Live Deal sends the notification address with every payment, so you do not have to set it in your PayPal account. Your site must be reachable from the internet, so this does not work on a local test site.

If your site took PayPal payments with CM Live Deal 3, the upgrade keeps it on this integration. While it is selected, the [Setup card](/dashboard/setup-card/) on the dashboard reminds you to switch. See [Upgrade](/install-upgrade/upgrade/#payment-plugins-action-needed) for the steps. An order a customer started before you switched is still completed when PayPal confirms it.

### When PayPal has not finished the payment

CM Live Deal completes an order only when PayPal reports the payment as **completed**. Some payments are not completed at once. For example, a payment by eCheck stays pending for a few days.

Until then, the order stays unpaid and the customer gets no coupon. With the REST API, the customer goes back to the cancel page with the message "Your payment was not successful". When PayPal completes the payment later, it notifies your site and the order is completed then. With the REST API this needs the webhook.

## Stripe

![/images/payment-plugin-4-0-stripe.png](/images/payment-plugin-4-0-stripe.png)

At checkout, the customer is taken to Stripe's payment page and comes back to your site after paying.

* **Display Name**: The name of the payment method shown at checkout. Leave it blank to use the default name, `Stripe`.
* **Currency ISO Code (e.g. EUR)**: The ISO code of the currency customers pay in, for example `USD` or `EUR`.
* **Mode**: `Live` to accept real payments, `Test` to test with Stripe's test mode.
* **Live Mode Secret Key**: Your Stripe secret key for `Live` mode.
* **Test Mode Secret Key**: Your Stripe secret key for `Test` mode.
* **Live Mode Webhook Signing Secret** and **Test Mode Webhook Signing Secret**: The signing secret Stripe shows when you add the webhook endpoint below. It starts with `whsec_`. Without it, notifications cannot be verified as coming from Stripe and are rejected, so an order is only completed if the customer's browser returns to your site.
* **Webhook Endpoint**: In the Stripe Dashboard, add a webhook endpoint at `<your site>/index.php?option=com_cmlivedeal&task=checkout.webhook&gateway=stripe` and subscribe it to the events `checkout.session.completed` and `checkout.session.async_payment_succeeded`. This completes the order even if the customer closes the browser before returning to your site.

Some payment methods, such as bank debits, are not paid at once. Stripe sends `checkout.session.async_payment_succeeded` when the money arrives, and the order is completed then. This also needs the webhook.

## Logs

Each plugin writes every payment notification it checks to a log file in Joomla's log folder. You find the folder in `System` → `Global Configuration` → `System` → **Path to Log Folder**.

* PayPal: `plg_cmlivedeal_paypal.php`
* Stripe: `plg_cmlivedeal_stripe.php`

Each entry shows the order and, when the order was not completed, the reason. For example, `Paid 4.00, expected 4.50` or `Payment is Pending`. Look here first when a customer paid but did not get a coupon.

When a log file grows over 1 MB, it is renamed to `plg_cmlivedeal_paypal_1.php` (or `plg_cmlivedeal_stripe_1.php`) and a new file is started. The older copy is deleted.
