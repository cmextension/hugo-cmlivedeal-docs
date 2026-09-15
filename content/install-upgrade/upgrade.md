---
title: Upgrade
weight: 20
---

To upgrade CM Live Deal, you just need to install the package of the new version.

We strongly recommend that you back up your site before you upgrade.

## From 3.x to 4.0

CM Live Deal 4.0 needs Joomla! 5.4 or later, or Joomla! 6, and PHP 8.1 or later. See [Technical requirements](/overview/technical-requirements/).

Most changes need nothing from you. Read this list before you upgrade, because a few of them do.

### Payment plugins: action needed

**PayPal** now uses PayPal's REST API. The old PayPal email and seconds-to-wait settings are gone, and they are not moved to the new settings. After the upgrade, PayPal does not work until you:

1. Create a REST app at [developer.paypal.com](https://developer.paypal.com/dashboard/applications).
2. Enter its client ID and secret key in the PayPal - CM Live Deal plugin.
3. Add the webhook to the app and enter its webhook ID in the plugin.

**Stripe** keeps working with your secret keys. Add the webhook and enter its signing secret too. Without it, an order is only completed if the customer's browser returns to your site.

See [Payment plugins](/configuration/payment-plugins/) for each setting.

### Settings the upgrade changes for you

* **Currency code** is a new option. The upgrade takes the currency of an enabled payment plugin. If no payment plugin is enabled, it uses `USD`. If your prices are not in US dollars and you have no payment plugin enabled, set it by hand. See [Currency](/configuration/currency/).
* If **Currency symbol** was empty, prices showed no symbol before. They now show the symbol of the currency code.
* The price tag can no longer show the sale price. If your site used that, the upgrade switches the price tag to the saved percentage. See [Deal list](/configuration/deal-list/#if-you-upgrade-from-an-older-version).
* Plugins you already have keep their state, enabled or disabled. Only the plugins that are new in 4.0 are enabled on install. See [Install](/install-upgrade/install/).

### Your data

* Every existing deal gets the access level `Public`, so everybody can still see it.
* Every existing deal runs from its start time to its end time, as before. It gets no weekly schedule.
* Two new email templates are added, for deals and coupons that expire soon.
* Some country names on saved orders are updated. For example, `Czech Republic` becomes `Czechia`, `Turkey` becomes `Türkiye`, `Swaziland` becomes `Eswatini` and `Scotland` becomes `United Kingdom`. `Netherlands Antilles`, `West Indies` and `Yugoslavia` are no longer in the list, but orders that have them keep them.
* If you uploaded the GeoLite2 City database, it keeps working. It stays at the same path. See [Get GeoLite2 City database](/cm-live-deal-search-module/get-geolite2-city-database/).

### Template overrides

The front end was redesigned in 4.0. Overrides of CM Live Deal views and layouts that you made for 3.x will probably break. Remove them, check the new look, and then make new overrides from the 4.0 files if you still need them. See [Template overrides](/developers/template-overrides/).

What changed:

* CM Live Deal no longer uses jQuery. Code in your override that calls `jQuery` or `$` fails unless your template loads jQuery itself.
* The scripts no longer create global variables such as `siteUrl`, `zoomLevel` or `token`. Read the values with `Joomla.getOptions()` instead, for example `Joomla.getOptions('com_cmlivedeal.deals')`.
* The deal popup no longer has tabs, and Bootstrap's tab script is no longer loaded.
* These CSS classes are gone: `thumbnail`, `detail-thumbnail`, `share-3`, `deal-description-3`, `deal-fine-print-3` and `share-form-control`. Prices no longer have the `text-primary` class.
* The capture button and the deal title in the popup card are now `<button>` elements.
* The accent colour now follows your template's primary colour.

### Language overrides

If you made language overrides for these keys, rename them to the new keys:

| Old key | New key |
| --- | --- |
| `COM_CMLIVEDEAL_ORDER_FILED_...` | `COM_CMLIVEDEAL_ORDER_FIELD_...` |
| `COM_CMLIVEDEAL_ERROR_EMPLATE_...` | `COM_CMLIVEDEAL_ERROR_TEMPLATE_...` |
| `COM_CMLIVEDEAL_MERCHANT_NO_DEALSS` | `COM_CMLIVEDEAL_MERCHANT_NO_DEALS_AVAILABLE` |

The language keys for countries also changed with the country list above. For example, `COM_CMLIVEDEAL_COUNTRIES_TURKEY` is now `COM_CMLIVEDEAL_COUNTRIES_TURKIYE`.

## From older versions

If you upgrade from a version which is older than 1.2.0, you need to run `Deal alias generator` tool after upgrade. You can view [Tools](/tools) section for more information.

If you upgrade from a version which is older than 1.5.0, you need to run `City alias generator` tool after upgrade. You can view [Tools](/tools) section for more information.
