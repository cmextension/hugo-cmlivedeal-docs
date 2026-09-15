---
title: Deal detail
weight: 50
---
There are 2 ways to show a deal's details. You choose one with **Deal detail** in [Deal](/configuration/deal/) options.

## Popup

![/images/deal-popup-4-0.png](/images/deal-popup-4-0.png)

The deal opens in a popup on top of the deal list. This is the default. The visitor can close the popup and open another deal without leaving the list.

The popup is one page that the visitor scrolls. From top to bottom it has:

*   The price tag and the `Featured` ribbon.
*   The deal image and title, the merchant's name and address, and the days and times of a deal that repeats weekly. The merchant's name links to the merchant's page when **Merchant page** is enabled.
*   For a deal with advance payment, how much to pay now and how much later, for example `Pay $3.51 now, $35.49 when you redeem`.
*   The time left, with a bar that gets shorter as the deal's time runs out. It changes colour like the [time left on the cards](/create-new-deal-in-front-end/deal-list/#time-left).
*   **Description** and **Fine print**.
*   **About** the merchant: the merchant's description, contact details and a map. The map is only shown when the merchant has coordinates.
*   **Photos**, when **Show merchant photos** is enabled and the merchant has a photo other than the deal's own image.
*   **Share this deal**: the deal's link with a `Copy link` button, and buttons to share on Facebook, X and Pinterest. After a click, the button says `Link copied` for 2 seconds.
*   At the bottom, always in view: the prices, what the visitor saves, how many coupons are left, and the `Get coupon` button.

Custom fields are shown in the popup too.

When the popup opens, the address in the browser changes to the deal's own link, so a visitor can copy it from the address bar as well. Opening that link shows the deal list with the popup already open. The browser's Back button closes the popup.

### On phones

![/images/deal-popup-4-0-phone.png](/images/deal-popup-4-0-phone.png)

On screens narrower than 576px, the popup fills the whole screen. The `Get coupon` button stays at the bottom while the visitor scrolls.

### Weekly deals between time slots

A deal that repeats weekly is not in the deal list between its time slots. If a visitor opens the deal from a link at that time, the time left is replaced by the next time slot, for example `Back on Saturday, 19 September 2026 11:00`. See [Schedule](/create-new-deal-in-back-end/#what-visitors-see).

## Separate page

![/images/deal-page-4-0.png](/images/deal-page-4-0.png)

The deal opens on its own page, with the deal's details, the merchant's contact details, a map and share buttons.

### Sticky buy bar

![/images/deal-page-4-0-buy-bar.png](/images/deal-page-4-0-buy-bar.png)

When **Sticky buy bar on mobile** is enabled in [Deal](/configuration/deal/) options, screens narrower than 768px show the `Get coupon` button in a bar at the bottom of the deal page. The bar stays in view while the visitor scrolls. It also shows the prices, when the deal shows them, and how many coupons are left. The button near the top of the page is hidden, so there is only one.

## Deal detail menu item

There is a menu item for the deal detail page, but it is optional. If you create this menu item (as a hidden menu item), its alias is used in the deal's link. If this menu item doesn't exist, your deal list's alias is used instead.

For example, if your deal list's URL is www.yoursite.com/deal-list and you don't create a menu item for the deal detail page, your deal URL could look like:

www.yoursite.com/deal-list/1-50-off-cocktails-and-2-tacos-at-rocks-lakeview-s-tuesday-specials

But if you create a menu item for the deal detail page with the alias `deal`, your deal URL would be:

www.yoursite.com/deal/1-50-off-cocktails-and-2-tacos-at-rocks-lakeview-s-tuesday-specials

The benefit of a separate menu item for the deal detail page is that you can show different modules on the deal page, or hide some modules there. If you use the deal list's menu item, every module on the deal list page is also shown on the deal page.
