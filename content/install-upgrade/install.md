---
title: Install
weight: 10
---
This section shows you how to install CM Live Deal on your Joomla! website.

We strongly recommend that you back up your site before you install any new extension.

## Install the package

1. Log in to your Joomla! administrator area.
2. In the left menu, choose `System`. In the `Install` panel, click `Extensions`.
3. Open the `Upload Package File` tab.
4. Drag the CM Live Deal package file (`pkg_cmlivedeal_4.0.0.zip`) into the box, or click `Or browse for file` and choose it.

![/images/install-4-0-upload.png](/images/install-4-0-upload.png)

Joomla! installs the package as soon as you choose the file. When it is done, you see the message `Installation of the package was successful.`

![/images/install-4-0-success.png](/images/install-4-0-success.png)

## What the package does for you

The package installs the CM Live Deal component, its modules and its plugins.

Joomla! normally installs every plugin disabled. CM Live Deal needs some of its plugins to work, so the package enables these for you:

* User - CM Live Deal's Merchant Profile
* Button - CM Live Deal Image
* Smart Search - CM Live Deal
* Privacy - CM Live Deal
* Task - CM Live Deal

The package only enables a plugin the first time it installs it. If you disable one of these plugins later, it stays disabled when you update CM Live Deal.

These plugins stay disabled. Enable them only when you need them:

* **PayPal - CM Live Deal** and **Stripe - CM Live Deal**: each needs an account with the payment service first. See [Payment plugins](/configuration/payment-plugins/).
* **Web Services - CM Live Deal**: enable it only if an app or another system uses the CM Live Deal API.
* **Membership Pro - CM Live Deal Integration**: enable it only if your site sells memberships with Membership Pro. See [Membership](/membership/).

The component also needs a folder for merchant images. If the `Image folder` option is empty, the package creates the folder `images/cmlivedeal` and chooses it for you.

## After you install

Go to `Components` -> `CM Live Deal` -> `Dashboard`. The Setup card at the top of the dashboard lists the settings your site still needs, for example the merchant user group and its permissions. Work down the list until every check is done. See [The Setup card](/dashboard/setup-card/).

If you want PDF coupons, you also need to install the mPDF library package. See [PDF coupon](/pdf-coupon).
