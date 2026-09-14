---
title: Payment plugins
weight: 180
---

Payment plugins are used in advance payment feature, you can enable advance payment in [the component's configuration](/configuration/advance-payment/) and in [back-end's deal form](/create-new-deal-in-back-end/).

There are 2 payment plugins available: PayPal and Stripe. These plugins are installed automatically by default. To find these plugins in Joomla!'s plugin list, you filter for `cmlivedeal` type.

![/images/payment_plugins.png](/images/payment_plugins.png)

## PayPal

![/images/payment_plugin_paypal.png](/images/payment_plugin_paypal.png)

* **Display Name**: The name of the payment method shown at checkout. Leave it blank to use the default name, `PayPal`.
* **Currency**: The currency customers pay in.
* **Environment**: Choose `Live` to accept real payments, or `Sandbox` to test with the PayPal sandbox. Each environment has its own credentials and its own webhook.
* **Live Client ID** and **Live Secret Key**, **Sandbox Client ID** and **Sandbox Secret Key**: Create a REST app at [developer.paypal.com](https://developer.paypal.com/dashboard/applications) and copy its client ID and secret key here. The app must belong to the environment you selected.
* **Live Webhook ID** and **Sandbox Webhook ID**: The ID PayPal gives to the webhook you add to the REST app. It starts with `WH-` or is a long hexadecimal string. Without it, notifications cannot be verified as coming from PayPal and are rejected, so an order is only completed if the customer's browser returns to your site.
* **Webhook Endpoint**: In your REST app, add a webhook at `<your site>/index.php?option=com_cmlivedeal&task=checkout.webhook&gateway=paypal` and subscribe it to the events `Checkout order approved` and `Payment capture completed`. This completes the order even if the customer closes the browser before returning to your site.

## Stripe

![/images/payment_plugin_stripe.png](/images/payment_plugin_stripe.png)

* **Display Name**: The name of the payment method shown at checkout. Leave it blank to use the default name, `Stripe`.
* **Currency ISO Code (e.g. EUR)**: The ISO code of the currency customers pay in, for example `USD` or `EUR`.
* **Mode**: `Live` to accept real payments, `Test` to test with Stripe's test mode.
* **Live Mode Secret Key**: Your Stripe secret key for `Live` mode.
* **Test Mode Secret Key**: Your Stripe secret key for `Test` mode.
* **Live Mode Webhook Signing Secret** and **Test Mode Webhook Signing Secret**: The signing secret Stripe shows when you add the webhook endpoint below. It starts with `whsec_`. Without it, notifications cannot be verified as coming from Stripe and are rejected, so an order is only completed if the customer's browser returns to your site.
* **Webhook Endpoint**: In the Stripe Dashboard, add a webhook endpoint at `<your site>/index.php?option=com_cmlivedeal&task=checkout.webhook&gateway=stripe` and subscribe it to the events `checkout.session.completed` and `checkout.session.async_payment_succeeded`. This completes the order even if the customer closes the browser before returning to your site.