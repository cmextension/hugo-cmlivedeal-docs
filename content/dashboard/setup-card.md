---
title: The Setup card
weight: 10
---
The Setup card is at the top of the dashboard (`Components` -> `CM Live Deal` -> `Dashboard`). It lists the settings your site needs before merchants can use it. It shows the settings that are already done as well as the ones that are not.

![/images/setup-card-4-0.png](/images/setup-card-4-0.png)

* A green tick means the setting is done.
* A warning sign means the setting is not done yet. The text tells you what to do, and the buttons on the right open the screen where you do it. The buttons open in a new tab, so the card is still there when you come back.
* The top right corner shows how many settings are done, for example `7 of 8 done`.

When everything is done, the card says `Everything is set up. You can hide this card.`

The card only lists the settings that apply to your site. A new site sees three of them. A site that uses custom fields, the API and PayPal sees eight.

## Plugins the component needs

CM Live Deal needs these two plugins:

* **User - CM Live Deal's Merchant Profile**
* **Button - CM Live Deal Image**

The package enables both when you install it. If one of them is disabled, the card names it. Click `Plugins`, find the plugin and enable it.

The other CM Live Deal plugins are not checked. You only need them for features you choose to use, such as payments, Smart Search or the API.

## Merchant image folder

Merchant images are saved in this folder. If no folder is chosen yet, the package creates `images/cmlivedeal` and chooses it when you install. A folder you already chose is kept.

The card warns you when:

* No folder is chosen. Create a folder inside the Media Manager's image folder, then click `Options` and choose it under `Image`.
* The folder does not exist, or the site cannot write to it. Create the folder, or fix its file permissions on your server.

## Merchant user group

Merchants are Joomla users in one user group. CM Live Deal needs to know which group that is.

Click `Options` and choose the group under `Merchant`. If your site has no group for merchants yet, click `New User Group` to create one first. See [Users component](/configuration/users-component/).

The permission checks below only show after you choose a merchant group.

## Permission to add a deal

The merchant group needs the `Create` permission, or merchants cannot add deals. Click `Permissions`, select your merchant group and set `Create` to `Allowed`. See [Permissions](/configuration/permissions/).

## Permission to fill in custom fields

This check only shows when your site has at least one custom field for CM Live Deal.

The merchant group needs the `Edit Custom Field Value` permission, or merchants cannot fill in your custom fields. Click `Permissions`, select your merchant group and set `Edit Custom Field Value` to `Allowed`.

## Permission to sign in to the API

This check and the next one only show while the **Web Services - CM Live Deal** plugin is enabled.

Joomla only lets a user group use the API when it has the `Web Services Login` permission. By default only Super Users have it. Click `Global Configuration`, open the `Permissions` tab, select your merchant group and set `Web Services Login` to `Allowed`.

## Permission to hold an API token

An API user also needs an API token. Joomla's **User - Joomla API Token** plugin decides which user groups can have one. By default only Super Users can.

Click `Plugins`, open **User - Joomla API Token**, and add your merchant group to `Allowed User Groups`.

Both API checks are needed. With only one of them, a merchant's API requests are refused.

## PayPal REST API

This check only shows while the **PayPal - CM Live Deal** plugin is enabled.

A site that you update from CM Live Deal 3.x keeps taking PayPal payments through PayPal Payments Standard, so the checkout keeps working. PayPal treats Payments Standard as legacy, so switch to the REST API as soon as you can:

1. Create a REST app at [developer.paypal.com](https://developer.paypal.com).
2. Click `PayPal - CM Live Deal` on the card to open the plugin.
3. Set `Integration` to `REST API`, then enter the app's credentials and webhook ID.

See [Payment plugins](/configuration/payment-plugins/).

## Hiding the card

When you are done with the card, click `Hide` in its top right corner. You only see this button if you can change CM Live Deal's `Options`.

You can hide the card even when some settings are not done, for example when you do not want merchants to use the API.

A hidden card comes back by itself when a setting fails that was not failing when you hid it. For example:

* You enable the **Web Services - CM Live Deal** plugin, and the merchant group cannot use the API yet.
* A setting that was done is undone, for example someone disables a plugin the component needs.

The card then says `Something new needs to be set up before merchants can use everything on the site.`

A setting that was not done when you hid the card does not bring it back. But if you fix that setting later and it breaks again, the card comes back.

To show the card again at any time, go to `Options` -> `Administration area` and set `Show the setup card` to `Yes`.
