---
title: Coupon template
weight: 40
---
After capturing a deal, a coupon for the deal is available for user. The user can print the coupon or show it to the merchant on their phone.

To design how coupon on your site looks like, go to Components -> CM Live Deal -> Coupon Template.

![/images/com_cmlivedeal_menu_coupon_template.webp](/images/com_cmlivedeal_menu_coupon_template.webp)

In the form, you can use HTML and CSS to design your coupon.

![/images/coupon_template.webp](/images/coupon_template.webp)

The coupon template comes with a ready-made design: the deal's name, the coupon code and the QR code in a large box, then the merchant, the dates, the description and the fine print. You can use it as it is, change the words, or write your own.

The same template is used in three places, so a design must work in all of them:

* The coupon page on your site, when **Coupon format** is `HTML`.
* The PDF coupon, when **Coupon format** is `PDF`. See [PDF coupon](/pdf-coupon/).
* The PDF attached to the order email, when **Attach PDF coupon to order confirmation email** is on.

When you write your own design:

* Use inline `style` attributes.
* Lay the coupon out with tables. The PDF library does not support flexbox or CSS grid.
* Leave room for the QR code. The PDF library draws the QR code without taking up any space, so give the table cell around `{qrcode}` a height. The sample does this for you.

## Start from the sample

Click **Sample** under the editor. A window shows the ready-made design as HTML.

![/images/coupon-template-4-0-sample.webp](/images/coupon-template-4-0-sample.webp)

1. Click **Toggle Editor** first, so the editor shows HTML. If you paste into the editor while it shows the design, it can rewrite the HTML and break the design.
2. Click **Copy** in the window, then close it.
3. Paste into the editor, replacing what was there, and click `Save`.

The sample leaves room for a QR code of the size in **QR code size (PDF coupon)**. If you change that size, copy the sample again. At size `5` the QR code fills the whole coupon box, so use a smaller size.

This is how you get the new design back after you changed the template, or after an upgrade from CM Live Deal 3 kept your old template.

## Preview

Click **Preview** under the editor. A new browser tab shows the coupon as it is in the editor now. You do not have to save first.

If the [mPDF library](/pdf-coupon/) is installed, Preview shows a PDF file, the way the PDF coupon looks. If not, it shows a web page, the way the HTML coupon looks.

![/images/coupon-template-4-0-preview.webp](/images/coupon-template-4-0-preview.webp)

The tags are filled in with example details, such as the merchant `Corner Bistro` and the coupon code `KXQRT`. If the editor is empty, Preview shows the sample.

Preview needs the same permission as editing the template: **Edit** on CM Live Deal.

To see a PDF coupon with the details of a real deal, use **Sample PDF Coupon Generator** in `Components > CM Live Deal > Tools`.

## Tags

To display the information of deal on your coupon, you can use the following tags (shortcodes):

*   **{code}**: Coupon's code.
*   **{qrcode}**: Coupon's QR code.
*   **{deal}**: Deal's name.
*   **{description}**: Deal's description.
*   **{terms}**: Deal's fine print.
*   **{merchant}**: Merchant's name.
*   **{address}**: Merchant's address.
*   **{phone}**: Merchant's phone.
*   **{captured}**: The date the coupon was captured.
*   **{expired}**: The date the coupon expires.

When a coupon is shown to the user, the tags are replaced with the deal's information.