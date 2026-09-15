---
title: QR code scanner
weight: 220
---

Every coupon has a QR code. The QR code holds only the coupon code. CM Live Deal has 2 QR code scanners that read it:

* The **merchant scanner**: the merchant scans a customer's coupon, sees the coupon, and redeems it.
* The **customer scanner**: the customer shows their own coupon to a device at the shop, and the coupon is redeemed right away.

Both scanners are disabled by default. Enable and configure them in [the Coupon options](/configuration/coupon/).

## Menu items

Create a menu item for each scanner you use. The menu item types are `Customer QR Code Scanner` and `Merchant QR Code Scanner`.

![/images/qr_code_scanner_menu_item.png](/images/qr_code_scanner_menu_item.png)

* Only merchants can use the merchant scanner. Set the menu item's **Access** to a level for your merchant group, so other visitors do not see it in the menu.
* Anyone can use the customer scanner, guests too. It usually runs on a tablet at the shop. Turn on **Component-only view for the customer QR code scanner** to hide the site's menus and modules on that device.

If a scanner is disabled in the options, its page shows "Page not found".

## The camera

The scanner uses the device's camera through the web browser. Two things must be right:

* **The site must use HTTPS.** Browsers only give the camera to secure pages. Over plain HTTP, the scanner does not start.
* **The browser must be allowed to use the camera.** The browser asks the first time. If the visitor refuses, they must allow the camera for your site in the browser settings.

![/images/qr_code_scanner_permission.png](/images/qr_code_scanner_permission.png)

On a phone or tablet, the scanner uses the back camera.

When the camera cannot start, the scanner shows a message in its place instead of an empty box:

| Problem | Message |
| --- | --- |
| Camera access refused | Access to the camera was refused. Allow camera access for this site in your browser settings, then try again. |
| No camera | No camera was found on this device. |
| Camera used by another app | The camera is already in use by another application. Close the other application, then try again. |
| Page is not HTTPS | The camera can only be used over a secure (HTTPS) connection. |
| Any other error | The camera could not be started. |

A **Try again** button is shown below the message, except for the HTTPS message: a page that is not secure never gets the camera, so trying again does not help.

The end of the message tells the visitor what to do instead:

* On the merchant scanner: "Enter the coupon code below instead."
* On the customer scanner: "Ask the merchant to scan your coupon instead."

## Merchant scanner

The merchant opens the scanner page and holds the customer's coupon in front of the camera. The merchant can also type the code in **Enter coupon code** and click **Search**, or press Enter. Typing works without a camera, so merchants can also use the page on a computer.

![/images/qr-code-scanner-4-0-merchant.png](/images/qr-code-scanner-4-0-merchant.png)

The scanner finds only coupons for the merchant's own deals. For any other code, it shows "Coupon not found".

When the coupon is found, a window shows it: the coupon code, the date it was captured, the deal, the expiry date, and whether it is redeemed. If **Show customer stats** and **Show customer visits** are on in [the Merchant options](/configuration/merchant/), the customer's stats and visits are shown too.

![/images/qr-code-scanner-4-0-coupon.png](/images/qr-code-scanner-4-0-coupon.png)

* If the coupon is not redeemed, click **Redeem**.
* If the coupon is already redeemed, click **Cancel Redemption** to undo it, for example after a mistake.

The camera pauses while the window is open, and starts again when it closes.

**Recently Scanned Coupons** lists the coupons the merchant scanned or searched, newest first, with the time. Click a code to open that coupon again. The list is kept for the current login session, and **Number of recently scanned coupons** in the options sets its length.

## Customer scanner

The customer holds their coupon in front of the camera. When the code is found, the coupon is redeemed at once, and a message says so:

* "Thank you! Coupon %s has been redeemed."
* "Coupon %s has already been redeemed."
* "Coupon not found"

The message closes by itself after the seconds set in **Close the customer scanner message after (seconds)**, and the scanner is ready for the next coupon.

![/images/qr-code-scanner-4-0-customer.png](/images/qr-code-scanner-4-0-customer.png)

Put instructions for customers in **Text above the customer QR code scanner** and **Text below the customer QR code scanner**. If the device has no working camera, the page tells the customer to ask the merchant to scan the coupon instead.

## For developers

Both scanners, and the Web Services API, trigger the `onCMLDBeforeRedeemCoupon` event before a coupon is redeemed or a redemption is cancelled. A plugin can stop the change, and the scanner shows the plugin's reason. See [Events](/developers/events/#oncmldbeforeredeemcoupon).
