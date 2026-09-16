---
title: Template overrides
weight: 30
---

CM Live Deal builds its pages from small files you can replace in your template. You copy a file into your template, change the copy, and CM Live Deal uses your copy from then on. Your work is not lost when you update CM Live Deal, because the extension never writes into your template.

There are two kinds of file you can override:

* **Views**: one whole page, for example the deal list or the coupon page.
* **Layouts**: one small piece that appears on many pages, for example a deal card, a countdown or the share buttons. Change a layout once and every page that uses it changes.

Most of the time a layout is what you want.

## Where the files go

Everything goes under `html` in your template folder. The site template is `templates/<your template>`. The administrator template is `administrator/templates/<your template>`.

| What | CM Live Deal file | Your copy |
| ---- | ----------------- | --------- |
| Site view | `components/com_cmlivedeal/tmpl/<view>/<file>.php` | `templates/<t>/html/com_cmlivedeal/<view>/<file>.php` |
| Site layout | `components/com_cmlivedeal/layouts/<file>.php` | `templates/<t>/html/layouts/com_cmlivedeal/<file>.php` |
| Administrator layout | `administrator/components/com_cmlivedeal/layouts/<file>.php` | `administrator/templates/<t>/html/layouts/com_cmlivedeal/<file>.php` |
| Module | `modules/mod_cmlivedeal_<name>/tmpl/<file>.php` | `templates/<t>/html/mod_cmlivedeal_<name>/<file>.php` |
| Merchants module field | `modules/mod_cmlivedeal_merchants/layouts/merchantfield.php` | `templates/<t>/html/layouts/mod_cmlivedeal_merchants/merchantfield.php` |
| PayPal buttons | `plugins/cmlivedeal/paypal/layouts/<file>.php` | `templates/<t>/html/layouts/plugins/cmlivedeal/paypal/<file>.php` |

Keep the folder names and the file name exactly the same. A file in the wrong folder is simply ignored, with no error message.

**Never delete the whole `html/layouts` folder** in your template. Cassiopeia keeps its own files there, for example `html/layouts/chromes/card.php`, which draws the box around every module. If you delete the folder, your modules lose their frames. Delete only the single file you no longer want to override.

If your template is a child template, Joomla looks in the child first and then in the parent. So you can put the override in either one.

## Which template is used

A page on the site uses the **site** template, and a page in the administrator uses the **administrator** template. This matters for one field: the deal image picker (`cmldmedia`). It is on the deal form in the back end *and* on the deal form in the front end, but it always uses the administrator layout, so the override always goes in `administrator/templates/<t>/html/layouts/com_cmlivedeal/cmldmedia.php`, even for the front-end form.

## The site views

These are in `components/com_cmlivedeal/tmpl/`:

| View | What it is |
| ---- | ---------- |
| `deals/default.php` | The deal list, the map and the toolbar. |
| `deal/default.php` | One deal on its own page. |
| `merchant/default.php` | A merchant and their other deals. |
| `coupon/default.php`, `coupons/default.php` | One coupon, and a customer's list of coupons. |
| `checkout/default.php`, `checkout.php`, `success.php`, `cancel.php` | The payment pages. |
| `customerscanner/default.php`, `merchantscanner/default.php` | The QR code scanners. |
| `dealform/edit.php`, `dealmanagement/default.php`, `images/default.php` | The merchant's own screens. |
| `customer/edit.php`, `customers/default.php` | The customer's own screens. |

## The site layouts

A layout is rendered with a small array of data, called `$displayData`. The table below says what is in that array for each layout, so you know what you can use in your copy. Every layout starts with `extract($displayData);`, so each key becomes a normal PHP variable.

Four layouts sit in `components/com_cmlivedeal/layouts/`:

| Layout | `$displayData` | Used by |
| ------ | -------------- | ------- |
| `coupon` | `coupon` | The scanner, after a code is read |
| `recent` | `coupons` | The merchant scanner's list of recent scans |
| `scannernotice` | *(nothing)* | Both scanners, for the camera message |
| `pagination-link` | `data`, `active` | One page number under the deal list |

The rest are in `components/com_cmlivedeal/layouts/deal/`, and their layout name starts with `deal.`:

| Layout | `$displayData` |
| ------ | -------------- |
| `deal.card` | `item`, `params`, `dealDetail`, `mediaWidth`, `advancePayment`, `showPriceTag`, `discountValues`, `showFeaturedRibbon`, `couponLimit` |
| `deal.modal` | `item`, `params`, `user`, `couponLimit`, `advancePayment`, `showPriceTag`, `discountValues`, `showFeaturedRibbon`, `showPhotos`, `merchantDetail` |
| `deal.list-toolbar` | `location`, `nearMeEnabled`, `nearMeActive`, `radiusSelectOptions`, `currentRadius`, `mapEnabled`, `displayMode`, `showSortOption`, `sortOptions`, `listSort` |
| `deal.map` | `markers`, `mapHeight`, `total`, `shown` |
| `deal.buy-bar` | `item`, `params`, `user`, `couponLimit`, `active` |
| `deal.capture-button` | `item`, `params`, `user`, `couponLimit`, `active`, `buttonClass`, `disabledClass` |
| `deal.countdown` | `endingTime`, and optionally `startingTime`, `class`, `announce` |
| `deal.meta-row` | `icon`, `content`, and optionally `class` |
| `deal.value-tag` | `item`, `params`, `advancePayment`, `showPriceTag`, `discountValues` |
| `deal.prices-card` | `item`, `params` |
| `deal.prices-discount` | `item`, `params` |
| `deal.advance-payment` | `item`, `params`, and optionally `short` |
| `deal.custom-fields` | `item`, `displayType`, and optionally `context` |
| `deal.photos` | `item` |
| `deal.share` | `item`, and optionally `showUrlInput` |
| `deal.merchant-contact` | `merchant`, and optionally `rowClass` |
| `deal.merchant-more-deals` | `name`, `username` |
| `deal.featured-ribbon` | *(nothing)* |

A dot in the layout name is a folder on disk. So `deal.card` is the file `deal/card.php`, and your copy goes in `templates/<t>/html/layouts/com_cmlivedeal/deal/card.php`.

`item` is one deal. `params` is the component's settings, as a `Joomla\Registry\Registry`. `user` is the visitor, as a `Joomla\CMS\User\User`. `merchant` is a `Registry` with the merchant's profile: `name`, `address`, `phone`, `website`, `latitude`, `longitude`, `username` and so on.

Layouts call each other. `deal.card` draws `deal.value-tag`, `deal.prices-card`, `deal.countdown` and several `deal.meta-row`s. If you only want to change the line that shows a merchant's address, override `deal.meta-row`, not the whole card.

## The administrator layouts

These are in `administrator/components/com_cmlivedeal/layouts/`, and your copies go in `administrator/templates/<t>/html/layouts/com_cmlivedeal/`:

`badge`, `chart-canvas`, `chart-range`, `checked-out`, `cmldmedia`, `dashboard-card`, `dashboard-placeholder`, `deal-image`, `expired-label`, `external-link`, `field-addon`, `field-time`, `grid-state`, `map-button`, `map-canvas`, `setup-checks`, `setup-hide`, `sort-link`, `stats-table`, `stats-tabs`, `stats-tiles`, `tos-field`, `user-stats`.

## Example: add a WhatsApp share button

The share buttons under a deal come from the `deal.share` layout. Here is how to add WhatsApp to them.

**1. Copy the file.** Copy `components/com_cmlivedeal/layouts/deal/share.php` to `templates/cassiopeia/html/layouts/com_cmlivedeal/deal/share.php`. Create the folders if they do not exist.

**2. Open your copy.** Near the top it builds the links:

```php
$dealUrl      = urlencode($item->url);
$imageUrl     = urlencode($item->thumbnail);
$description  = urlencode($item->title);
$facebookUrl  = 'https://www.facebook.com/sharer/sharer.php?u=' . $dealUrl;
```

**3. Add one line** for the WhatsApp address, just above `$links`:

```php
$whatsAppUrl = 'https://wa.me/?text=' . $description . '%20' . $dealUrl;
```

**4. Add one entry** to the `$links` array:

```php
$links = [
    ['url' => $whatsAppUrl, 'icon' => 'fa-whatsapp', 'title' => 'COM_CMLIVEDEAL_SHARE_WHATSAPP'],
    ['url' => $facebookUrl, 'icon' => 'fa-facebook', 'title' => 'COM_CMLIVEDEAL_SHARE_FACEBOOK'],
    ['url' => $xUrl, 'icon' => 'fa-x-twitter', 'title' => 'COM_CMLIVEDEAL_SHARE_TWITTER'],
    ['url' => $pinterestUrl, 'icon' => 'fa-pinterest', 'title' => 'COM_CMLIVEDEAL_SHARE_PINTEREST'],
];
```

The rest of the file draws the buttons for you, so there is nothing else to change.

**5. Save and reload a deal.** The new button is there:

![/images/template-overrides-4-0-share.png](/images/template-overrides-4-0-share.png)

`title` is a language key, used for screen readers. Add `COM_CMLIVEDEAL_SHARE_WHATSAPP` to your site's language override in `System > Language Overrides`, or use a key that already exists.

## Tip: reuse the original instead of copying it

If you only want to *add* something, you do not have to copy the whole file. Your override can call the original file and print its output first:

```php
<?php

defined('_JEXEC') or die;

$layout = new Joomla\CMS\Layout\FileLayout(
    'deal.share',
    JPATH_SITE . '/components/com_cmlivedeal/layouts'
);

echo $layout->render($displayData);
?>
<p class="mt-2">Tell your friends and you both get a free coffee.</p>
```

The good part is that you keep every improvement we make to the original file. The risk is the same as with any override: if we change the original, your extra part may no longer fit.

## After a CM Live Deal update

An update never touches your template, so your overrides keep working. But they are copies of files that have changed, so:

* Compare your copy with the new original and bring over anything useful.
* If a layout you copied no longer exists, delete your copy.
* Overrides made for CM Live Deal 3.x will not work in 4.0. The front end was rebuilt. Remove them, look at the new pages, and make new overrides from the 4.0 files if you still need them.

## See also

* [Styling and JavaScript](/developers/styling/): change the look with CSS variables instead of an override.
* [Custom fields](/custom-fields/): add your own data to a deal without touching a template.
* [Events](/developers/events/): change what CM Live Deal *does*, not what it looks like.
