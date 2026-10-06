---
title: Mobile app
weight: 235
---

The **CM Live Deal App API** add-on lets a customer mobile app work with your site. Customers browse deals, sign in, take free coupons, pay for deals and show their coupons at the counter, all from the app.

The add-on is a separate package, `pkg_cmlivedealapp`. A site without an app does not need it. Without it, your site has no public API and no app sign-in.

The add-on is the server side only. CMExtension builds the app itself for you, with your name, colours and icon. [Contact us](https://www.cmextension.com/) if you want your own app.

## Requirements

* CM Live Deal 4.0.0 or later. The add-on refuses to install on an older version.
* Joomla 5.4 or later and PHP 8.1 or later, the same as CM Live Deal.
* For payments inside the app: Stripe, or PayPal with the REST API. See [Payments](#payments).

## Install

1. Install CM Live Deal 4.0.0 or later first.
2. Go to `System > Install > Extensions` and upload `pkg_cmlivedealapp_1.0.0.zip`.
3. The package installs one component and five plugins, and enables the plugins.

Then go to `Components > CM Live Deal App API > Dashboard` and look at the **Setup** card.

## Setup

The **Setup** card lists what the app still needs. When everything is done, it says *Everything is set up for the app*.

![/images/app-4-0-setup.png](/images/app-4-0-setup.png)

* **Customers may sign in to the app.** Joomla only lets a group sign in to its API when the group has the **Web Services Login** permission. Go to `System > Global Configuration > Permissions`, select the group new accounts join (usually **Registered**), and set **Web Services Login** to **Allowed**.
* **The app's routes are enabled.** The plugin **Web Services - CM Live Deal App API** must be enabled.
* **The app's sign-in plugin is enabled.** The plugin **API Authentication - CM Live Deal App API** must be enabled.

You do not need Joomla's own API tokens for the app. The app signs in with the customer's username and password and gets its own token.

Accounts that can sign in to the administrator cannot sign in to the app. This is on purpose: a lost phone must never carry administrator rights. Use a customer account to test the app.

## Options

Go to `Components > CM Live Deal App API > Options`.

### App

![/images/app-4-0-options-app.png](/images/app-4-0-options-app.png)

* **Browse Without Signing In**: Let the app show deals, cities and categories before the customer signs in. Switch it off to show deals to signed-in customers only. Default: on.
* **App Link Scheme**: The scheme of the links that bring the customer back to the app, for example `cmlivedeal` in `cmlivedeal://payment/success`. Letters only. It must match your app. Default: `cmlivedeal`.
* **Checkout Link Lifetime (seconds)**: How long the link that opens the website checkout works, from 10 to 600 seconds. The app opens it at once, and it works only once. Default: 60.

### Sign-in

![/images/app-4-0-options-signin.png](/images/app-4-0-options-signin.png)

* **Failed Sign-ins per Account**: How many wrong passwords one username may have before it has to wait. Default: 5.
* **Failed Sign-ins per IP Address**: The same for one IP address. Keep it well above the per-account limit, because many customers can share one address on a mobile network. Default: 20.
* **Sign-in Window (minutes)**: The time over which failed sign-ins are counted, and how long a limited account or address waits. Default: 15.

A successful sign-in starts the account's count again.

## Who can sign in

The app refuses these accounts, and tells the customer why:

* blocked accounts, accounts that are not activated yet, and accounts that must reset their password;
* accounts with two-factor authentication. The app does not support it yet, so these customers use the website;
* accounts that can sign in to the administrator;
* accounts in a group without **Web Services Login**.

If you block an account, or it gets two-factor authentication, its app session ends at the next request.

Registration, password reset and two-factor setup happen on the website. The app links there.

## Payments

When advance payment is on (see [Advance payment](/coupons/advance-payment/)), the customer pays for a deal before they get the coupon.

* **Stripe** is paid inside the app with Stripe's payment sheet (cards, and Google Pay when **Google Pay and Apple Pay** is on in the plugin's **Mobile App** tab). Add the **Publishable Key** for your mode in the Stripe plugin, and subscribe your Stripe webhook to `payment_intent.succeeded` as well. See [Payment plugins](/configuration/payment-plugins/#stripe).
* **PayPal with the REST API** opens PayPal's own page inside the app. The customer approves the payment there, and PayPal sends them back to the app.
* **PayPal Payments Standard and other payment plugins** cannot take a payment inside the app. For these, the app opens your website's checkout. The customer is signed in there automatically, with a link that works once and only for the **Checkout Link Lifetime**.

Your site, not the app, checks every payment with Stripe or PayPal before it completes the order. The webhooks complete the order too, so a customer who closes the app right after paying still gets the coupon.

Free deals need no payment. The app takes the coupon through CM Live Deal's own Web Services API.

## Dashboard

`Components > CM Live Deal App API > Dashboard` shows how your customers use the app. Choose the last 7, 30 or 90 days at the top.

![/images/app-4-0-dashboard.png](/images/app-4-0-dashboard.png)

* **Active today**, **Active in 7 days**, **Active in 30 days**: how many different customers used the app.
* **Active devices**: devices still signed in that were used in the last 30 days.
* **Sign-ins** and **Failed sign-ins**: in the days you chose.
* Charts: customers and sign-ins per day, coupons taken per day in the app and on the website, and active devices by platform and by app version.
* **App and website**: coupons taken, coupons redeemed and, with advance payment on, paid orders and advance payments, split into app and website, with the app's share.

A coupon counts as the app's when it was taken in the app, or when its order was placed from the app.

## Devices

`Components > CM Live Deal App API > Devices` lists every device that signed in: the customer, the device name and platform, the app version, when it signed in and when it was last used.

![/images/app-4-0-devices.png](/images/app-4-0-devices.png)

To sign a device out, select it and click **Sign Out**. The app on that device is signed out at its next request. Do this when a customer loses their phone.

Search finds customer names, usernames and device names. Type `id:` and a number to find one device.

## Privacy

The plugin **Privacy - CM Live Deal App API** adds the app's data to Joomla's privacy requests. Go to `Users > Privacy > Capabilities` to read what it stores.

* **Export** includes the customer's devices, the days they used the app, which coupons and orders came from the app, their checkout links, and their failed sign-ins (the username and date only).
* **Removal** deletes the devices, which signs the app out everywhere, and the activity, checkout links and failed sign-ins. Which coupons and orders came from the app is kept without the customer, so the dashboard's numbers stay right.

Failed sign-ins are kept for 90 days, with the username and IP address, for the dashboard and the sign-in limits. Mention this in your privacy policy.

## Uninstall

Go to `System > Manage > Extensions`, find **CM Live Deal App API package** and uninstall it. This removes the component, the five plugins and the add-on's five tables. CM Live Deal and its data are not touched, and the app stops working.

For developers: the app's endpoints are described in [App API](/app/api/).
