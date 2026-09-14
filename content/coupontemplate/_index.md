---
title: Coupon template
weight: 40
---
After capturing a deal, a coupon for the deal is available for user. The user can print the coupon or show it to the merchant on their phone.

To design how coupon on your site looks like, go to Components -> CM Live Deal -> Coupon Template.

![/images/com_cmlivedeal_menu_coupon_template.png](/images/com_cmlivedeal_menu_coupon_template.png)

In the form, you can use HTML and CSS to design your coupon.

![/images/coupon_template.png](/images/coupon_template.png)

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