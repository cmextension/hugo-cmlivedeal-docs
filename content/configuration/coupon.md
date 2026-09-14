---
title: Coupon
weight: 100
---

![/images/com_cmlivedeal_coupon.png](/images/com_cmlivedeal_coupon.png)

*   **Coupon code length**: The number of characters in a coupon code. The default is 5.
*   **Coupon code characters**: The type of characters used in coupon codes. The default is letters and digits. Letters are always uppercase.
    *   _Letters only (A to Z)_: for example, `GWLCA`.
    *   _Digits only (0 to 9)_: for example, `83061`.
    *   _Letters and digits (A to Z, 0 to 9)_: for example, `F52H2`.
*   **Coupon format**:
    *   _HTML_: The coupon is a web page.
    *   _PDF_: The coupon is a PDF file. PDF coupons need the mPDF PHP library. See the [PDF coupon](pdfcoupon.html#ref-pdfcoupon) section to learn how to install it.
*   **QR code size (HTML coupon)**: The size of the QR code, in pixels. The default is 150. Only used for HTML coupons.
*   **QR code size (PDF coupon)**: The size of the QR code in PDF coupons.
*   **Guest can get coupon**: Allow visitors to get coupons without registering.
*   **Limit coupon quantity**: Let administrators and merchants set how many coupons each deal has. When this option is disabled, every deal has unlimited coupons.
*   **One coupon per registered user**: Let each registered user get only one coupon per deal. Disable this option to let registered users get as many coupons as they want.
*   **Merchant QR code scanner**: Enable the QR code scanner for merchants. Merchants scan the QR code on a customer's coupon to find and redeem it quickly.
*   **Number of recently scanned coupons**: How many recently scanned coupons the QR code scanner page shows. The list is kept for the current session.
*   **Customer QR code scanner**: Enable the QR code scanner for customers. Customers use it to redeem their own coupons: as soon as a coupon is scanned and found, it is marked as redeemed.
*   **Text above the customer QR code scanner**: Custom HTML shown above the customer QR code scanner, for example a title and instructions.
*   **Text below the customer QR code scanner**: Custom HTML shown below the customer QR code scanner, for example instructions or privacy information.
*   **Component-only view for the customer QR code scanner**: Hide the rest of the site, such as menus and modules, and show only the CM Live Deal content.
*   **Close the customer scanner message after (seconds)**: After a QR code is scanned, a message tells the customer whether the coupon code is valid. This option sets how many seconds the message stays open.
