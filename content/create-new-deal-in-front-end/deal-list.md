---
title: Deal list
weight: 40
---
The deal list is where visitors browse deals and find the ones they want.

In your back end, create a menu item of the `Deal List` type. Open that menu item on your site to see the list.

![/images/deal-list-4-0-cards.png](/images/deal-list-4-0-cards.png)

## Deal cards

Each deal is shown as a card. A card has:

*   The deal image, with the price tag and the `Featured` ribbon on top of it.
*   The deal title, and the sale and original prices.
*   The merchant's name and address.
*   How far away the deal is, when the list is searched around a location. See [Deals near me](#deals-near-me).
*   How much the visitor pays in advance, when the deal uses advance payment.
*   The days and times of a deal that repeats weekly.
*   The time left.
*   How many coupons are left, when **Limit coupon quantity** is enabled.
*   A `View deal` button. It opens the deal in a popup or on its own page. See [Deal detail](/create-new-deal-in-front-end/deal-detail/).

How many cards are in a row, and how wide the image is, are set in [Deal list](/configuration/deal-list/) options. On narrow screens the cards step down to fewer columns by themselves.

## Time left

The time left counts down while the page is open. It changes colour as the end comes closer:

*   **Green**: more than 12 hours left, for example `Ends in 3 days 4 hours 10 minutes`.
*   **Orange**: less than 12 hours left.
*   **Red**: less than 2 hours left. The text changes to `Hurry! Only 1 hour 34 minutes left!`
*   **Grey**: `Already expired`.

For a deal that repeats weekly, the time left counts down to the end of the current time slot, not to the deal's **Ending time**. Between time slots the deal is not in the list. See [Schedule](/create-new-deal-in-back-end/#what-visitors-see).

## Sorting

When **Show sorting options** is enabled in [Deal list](/configuration/deal-list/) options, visitors can sort the list by `Newest first` or `Ending soonest`. `Nearest first` is added when the list is searched around a location.

When **Show featured deals first** is enabled, featured deals stay at the top whatever the visitor picks.

## Deals near me

![/images/deal-list-4-0-near-me.png](/images/deal-list-4-0-near-me.png)

When **Geolocation service** is set in [General](/configuration/general/) options, the list shows a `Deals near me` button.

*   With _HTML5 Geolocation_, the browser asks the visitor to share their location. If the visitor says no, or the location cannot be found, they see `Your location could not be detected. Please allow location access and try again.` An old browser without geolocation shows `Your browser cannot detect your location.`
*   With _MaxMind GeoLite2_, the location comes from the visitor's IP address. The browser asks nothing. See [Get GeoLite2 City database](/cm-live-deal-search-module/get-geolite2-city-database/).

After the location is found:

*   A `Near you` chip replaces the button. `Show all deals` goes back to the full list.
*   Only deals whose merchant is inside the search radius are listed. The radius starts at **Search radius around the visitor**.
*   A drop-down list lets the visitor pick another radius, for example `Within 25 km`. The choices come from **Search radius options**. Leave that option empty to hide the drop-down list.
*   Each card shows how far away the merchant is, for example `1.1 km away`, or `800 m away` when it is closer than 1 km.

Merchants without coordinates are never inside the radius, so their deals are not listed near the visitor. Make sure each merchant has a location. See [Manage merchants](/merchants/manage-merchants/).

Visitors can also search near their location, or in a city, with the [Search module](/cm-live-deal-search-module/search-for-nearby-deals/). A city search uses the city's own coordinates and radius. It shows the distances and the radius drop-down list too, but not the `Near you` chip.

## Map view

![/images/deal-list-4-0-map.png](/images/deal-list-4-0-map.png)

When **Map view** is enabled in [General](/configuration/general/) options, a `List` / `Map` switch is added to the list.

The map shows every deal that matches the visitor's search, not only the deals on the current page. Each deal is a marker at its merchant's location. Click a marker to see the deal's image, title, merchant, distance, prices and time left, with a `View deal` button.

*   When the visitor searched near their location, a `You are here` marker shows where they are.
*   Deals whose merchant has no coordinates are not on the map. When none of the deals has a location, the map shows `None of these deals has a merchant location to show on the map.`
*   The map shows at most **Maximum deals on the map** markers. When there are more deals, it says `Showing the first 200 deals on the map.`

## Search engines

The deal list, each deal and each merchant page include structured data (JSON-LD) that search engines read: the deal as an offer with its price and end date, and the merchant as a local business with its address. There is nothing to set up. You can check a page with Google's [Rich Results Test](https://search.google.com/test/rich-results).
