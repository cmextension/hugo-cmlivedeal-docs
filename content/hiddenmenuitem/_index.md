---
title: Hidden menu item
weight: 190
---

Hidden menu item is not visible in your front-end. It is used to provide friendly URL to specific pages in CM Live Deal. At the present time there are 2 types of pages requiring hidden menu item:

*   The coupon download page requires a `Coupon Download (Hidden)` menu item.
*   The merchant page requires a `Merchant Details (Hidden)` menu item.

If your `Coupon download` page is

**http://yoursite.com/download-coupon**

then the URL to download a coupon looks like

**http://yoursite.com/download-coupon/COUPON-CODE-HERE**

In the same way, the merchant page can have a URL like this:

**http://yoursite.com/merchants/merchant-username**

with `merchants` is the alias of `Merchant` menu item, `merchant-username` is the username of the merchant.

To create hidden menu items for these pages, you create a new menu, name it whatever you like, for example `Hidden menu`. Create new menu items for `Coupon download` page and `Merchant` page in this new menu, as long as you don't assign this new menu to a menu module and display the module in your front-end, `Coupon download` menu item and `Merchant` menu item are not displayed in your front-end.