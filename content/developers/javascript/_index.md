---
title: JavaScript
weight: 50
---

CM Live Deal 4.0 has no jQuery and no inline `<script>` blocks. Everything is a plain file loaded through Joomla's web asset manager, and every value a script needs is handed to it as JSON. This page is for you if you write a [template override](/developers/template-overrides/) and need to keep the scripts working, or if you want to add your own behaviour.

## What changed in 4.0

* **jQuery is gone.** CM Live Deal no longer loads it and no longer needs it. If your own code used `jQuery` on a CM Live Deal page, load jQuery yourself, or rewrite the code — modern browsers do everything the extension used it for.
* **jQuery History is gone.** `media/com_cmlivedeal/js/jquery.history.js` and the `History` global no longer exist. The deal popup uses the browser's own `history.pushState()`.
* **The calendar helper is gone.** The old `cmlivedealadministrator.calendar` HTML helper printed an inline jQuery snippet. Date fields are ordinary Joomla `calendar` fields now.
* **All the JavaScript is in `src/js/`** and built into `media/com_cmlivedeal/js/`.

Never edit a file in `media/com_cmlivedeal/js/`. An update overwrites it. Add your own file instead, as shown below.

## The assets

The assets are declared in `media/com_cmlivedeal/joomla.asset.json` under the names below. Use a name, not a path, and Joomla loads the file once with its dependencies, whoever asked for it:

```php
use Joomla\CMS\Factory;

$wa = Factory::getApplication()->getDocument()->getWebAssetManager();
$wa->useScript('com_cmlivedeal.site-deals');
```

If your code runs somewhere CM Live Deal has not loaded yet — your own module, a plugin — register the file first:

```php
use CMExtension\Component\CMLiveDeal\Administrator\Helper\CMLiveDealHelper;

CMLiveDealHelper::registerAssets($wa);
```

| Asset | What it does |
| ----- | ------------ |
| `com_cmlivedeal.site-styles`, `com_cmlivedeal.site-styles-rtl` | The front-end stylesheet, and its right-to-left companion. |
| `com_cmlivedeal.site-countdown` | The `<cmld-countdown>` element. Loaded on every front-end page. |
| `com_cmlivedeal.site-deals` | The deal list: the quick-view popup, click tracking, the copy-link button. |
| `com_cmlivedeal.site-deals-map` | The map on the deal list, "Near me" and the list/map switch. |
| `com_cmlivedeal.site-deal` | One deal page: its map, and `getCoupon()`. |
| `com_cmlivedeal.site-deal-form` | The merchant's deal form. |
| `com_cmlivedeal.site-search-module` | The search module's buttons. |
| `com_cmlivedeal.site-scanner-camera` | Camera picking, shared by both scanners. |
| `com_cmlivedeal.site-customer-scanner`, `com_cmlivedeal.site-merchant-scanner` | The two QR code screens. |
| `com_cmlivedeal.site-html5-qrcode` | The QR code reader library. |
| `com_cmlivedeal.site-paypal`, `com_cmlivedeal.site-paypal-standard` | The two PayPal checkouts. |
| `com_cmlivedeal.leaflet` | Leaflet 1.9.4, for OpenStreetMap. Style and script. |
| `com_cmlivedeal.chartjs` | Chart.js 4.5.1, for the dashboard. |
| `com_cmlivedeal.admin-styles`, `com_cmlivedeal.admin-dashboard`, `com_cmlivedeal.admin-deal-edit`, `com_cmlivedeal.admin-images`, `com_cmlivedeal.admin-cmldmedia`, `com_cmlivedeal.admin-location-map`, `com_cmlivedeal.admin-merchants-map`, `com_cmlivedeal.admin-merchants-module`, `com_cmlivedeal.admin-optimization`, `com_cmlivedeal.admin-noty` | The back end. |

Font Awesome is registered on the fly as `com_cmlivedeal.fontawesome` when the `Font Awesome` option is on.

## How a script gets its data

No script holds a URL, a token or a translated string. Each one reads a named block of JSON that PHP wrote into the page:

```js
var options = Joomla.getOptions('com_cmlivedeal.deals', {});
```

| Key | Written by | Holds |
| --- | ---------- | ----- |
| `com_cmlivedeal.deals` | The deal list view | `siteUrl`, `token`, `zoom`, `mapProvider`, and `openDealId` when a deal must open on load. |
| `com_cmlivedeal.deals-map` | The deal list view, when the map is on | `mapProvider`, `zoom`, `markers`, `centre`, `dealDetail`, `geolocationService` and four messages. |
| `com_cmlivedeal.deal` | The deal view | `siteUrl`, `token`, `zoom`, `mapProvider`, `latitude`, `longitude`. |
| `com_cmlivedeal.customerscanner` | The customer scanner | `token`, `countdown`, `camera`. |
| `com_cmlivedeal.merchantscanner` | The merchant scanner | `token`, `camera` and two messages. |
| `mod_cmlivedeal_search` | The search module | `html5` and two geolocation messages. |
| `plg_cmlivedeal_paypal` | The PayPal plugin | `containerId`, `orderId`, `returnUrl`, `cancelUrl`. |
| `plg_cmlivedeal_paypal_standard` | The PayPal plugin, legacy mode | `formId`, `seconds`. |
| `com_cmlivedeal.location-map` | The city, options and merchant profile map fields | The form field ids to write to, and the map's settings. |
| `com_cmlivedeal.dashboard`, `com_cmlivedeal.merchants-map`, `com_cmlivedeal.merchants-module`, `com_cmlivedeal.optimization` | The back end | |

Two rules follow from this. **Keep the wrapper elements and their ids** when you override a view — a script that cannot find `mapCanvas` or `adminForm` does nothing. And **do not print the token yourself**: read it from the options, as the scripts do.

Translations reach the scripts the same way, through `Text::script()` and `Joomla.Text._()`. The countdown's nine strings and the share button's "Copied" message are already there; add your own key from your override if you need one.

## The hooks in the HTML

Scripts find their buttons by `data-` attribute, never by a class name, so you can restyle freely. Keep these attributes when you copy a layout:

| Attribute | Where | What it does |
| --------- | ----- | ------------ |
| `data-cmld-search="submit"` | Search module | Submits the form, asking for the visitor's position first when "Near me" is chosen. |
| `data-cmld-search="clear"` | Search module | Empties keyword, category and city, then submits. |
| `data-cmld-deals="nearme"`, `="clear-nearme"` | Deal list toolbar | Turn "Near me" on and off. |
| `data-cmld-deals="display"` | Deal list toolbar | The list/map switch. The button's `value` is `list` or `map`. |
| `data-cmld-copy="<input id>"` | Share links | Copies that input's value and says so on the button. |
| `data-dealid`, `data-dealtitle`, `data-dealurl` | Deal popup | Click tracking and the address in the address bar. |
| `data-cmld-theme` | The `.cmlivedeal` wrapper | The [colour scheme](/developers/styling/#dark-mode). Written by PHP, read by CSS. |

The back end has `data-cmld-target`, `data-cmld-section` and `data-cmld-range` on the dashboard.

Two functions are global on purpose, because layouts call them from an `onclick`:

* `getCoupon(dealId)` — sends the visitor to the capture task. Defined by `site-deal` on a deal page and by `site-deals` on the list.
* `openModal(dealId, dealTitle, dealUrl)` — used by the quick-view popup.

If you override `deal/capture-button.php` and keep its button, keep the `onclick`, or wire your own listener to the same URL.

## The countdown element

The live countdown is a custom element, `<cmld-countdown>`, with the text inside it:

```html
<cmld-countdown ends="2026-09-30T18:00:00+00:00">
    <span class="cmld-meta">
        <i class="far fa-clock"></i>
        <time datetime="2026-09-30T18:00:00+00:00" class="cmld-countdown-text">Ends in 3 days</time>
    </span>
</cmld-countdown>
```

| | |
| --- | --- |
| `ends` | Required, ISO 8601. Change it and the text updates at once. |
| `starts` | Optional, ISO 8601. With both, the element also sets `--cmld-countdown-left` from `1` to `0` and the progress bar fills. |
| `data-state` | Set by the element: `live`, `soon`, `urgent` or `expired`. See [Styling](/developers/styling/#the-countdown-states). |
| `.cmld-countdown-text` | **Required.** The element writes the remaining time into this child and does nothing if it is missing. |

The server renders the first text, so a visitor without JavaScript still sees how long is left. After that all the countdowns on a page share one timer, which ticks on the minute, stops when there is nothing left to count, and catches up when the tab comes back into view.

### Replacing it

The element is only defined if nothing has taken the name yet:

```js
if (!window.customElements.get('cmld-countdown')) {
    window.customElements.define('cmld-countdown', CMLiveDealCountdown);
}
```

So to use your own, define `cmld-countdown` from a script that runs **before** `com_cmlivedeal.site-countdown`, and CM Live Deal will leave it alone.

## Adding your own script

Do it from a template override, so an update cannot remove it. Put the file in your template, for example `media/templates/site/cassiopeia/js/deals-extra.js`, and register it at the top of your copy of the layout or view:

```php
use Joomla\CMS\Factory;

$wa = Factory::getApplication()->getDocument()->getWebAssetManager();

$wa->registerAndUseScript(
    'my-template.deals-extra',
    'media/templates/site/cassiopeia/js/deals-extra.js',
    [],
    ['defer' => true],
    ['core', 'com_cmlivedeal.site-deals']
);
```

The last argument is the list of assets yours depends on, so Joomla loads them in the right order. In the file, read whatever you need from the options:

```js
document.addEventListener('DOMContentLoaded', function () {
    var options = Joomla.getOptions('com_cmlivedeal.deals', {});

    document.querySelectorAll('.cmld-card').forEach(function (card) {
        // your code
    });
});
```

CM Live Deal's own scripts are not deferred and do their work on `DOMContentLoaded`, so a listener added the same way runs after them.

## See also

* [Styling](/developers/styling/): the CSS variables, dark mode and the class names.
* [Template overrides](/developers/template-overrides/): where the HTML above comes from.
* [Web Services API](/developers/api/): for a script that has to read or write data.
