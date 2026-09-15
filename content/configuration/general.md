---
title: General
weight: 40
---

![/images/options-4-0-general.png](/images/options-4-0-general.png)

*   **Font Awesome**: CM Live Deal uses Font Awesome for icons. If your template or another extension already loads Font Awesome, set this option to `Disabled` so CM Live Deal does not load it again.
*   **Dark mode**: Show CM Live Deal's pages in dark colours. Only CM Live Deal's own content changes, not your template, so pick the value that matches your template.
    *   _Disabled_: Always light. Pick this if your template is always light. This is the default.
    *   _Follow visitor's system setting_: Dark when the visitor's phone or computer is set to dark mode. Pick this if your template does the same.
    *   _Always dark_: Pick this if your template is always dark.
    *   _Follow template (data-bs-theme)_: Dark when the page has `data-bs-theme="dark"`. Pick this if your template has a light/dark switch that sets this attribute.

    If you are not sure, open your site on a device in dark mode. If your template turns dark, pick _Follow visitor's system setting_.
*   **Thumbnail width in image list**: The width, in pixels, of the thumbnails in the administrator's image list. The default is 50.
*   **Date format**: The date format used on the site.
*   **Time format**: The time format used on the site.
*   **Geolocation service**: The service used to detect the visitor's location.
    *   _Disabled_: Visitors cannot search for deals near their location.
    *   _HTML5 Geolocation_: The browser asks the visitor to share their location.
    *   _Maxmind_: Use GeoLite2 data created by [MaxMind](https://www.maxmind.com).
*   **Search radius around the visitor**: How far, in kilometres, to look for deals near the visitor's location. The default is 5. Used when a visitor searches for deals near them.
*   **Search radius options**: The distances, in kilometres, a visitor can choose from on the deal list, separated by commas. The default is `1,5,10,25,50`. Leave it empty to always use **Search radius around the visitor**.
*   **Map view**: Add a List/Map switch to the deal list. The map shows every matching deal at its merchant's location. Enabled by default.
*   **Maximum deals on the map**: The most markers the map shows at once. The map has no pages, so this keeps a busy site from drawing thousands of markers. The default is 200. Only shown when **Map view** is enabled.
*   **Map provider**: The map service to use.
    *   _OpenStreetMap_: Free and needs no key. This is the default.
    *   _Google Maps_: Needs a **Google Maps API key**.

    With OpenStreetMap the map pictures load from `openstreetmap.fr`. With Google Maps they load from Google.
*   **Map height**: The height of the maps, in pixels, on both the site and the administrator. The default is 400.
*   **Map zoom level**: The zoom level maps open at. The default is 15.
*   **Default map location**: The location the map shows first, on both the site and the administrator. Click the map or drag the marker to the location you want, or enter its latitude and longitude. This works with both map providers.
*   **Google Maps API key**: Your Google Maps API key, used for the maps in CM Live Deal. You can get a key at https://console.developers.google.com/. Only needed with Google Maps.
*   **Falang integration**: Enable this option if you use Falang.
