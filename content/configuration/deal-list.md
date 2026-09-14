---
title: Deal list
weight: 50
---
![/images/com_cmlivedeal_deal_list.png](/images/com_cmlivedeal_deal_list.png)

*   **Deal list columns**: The number of columns in the deal list.
*   **Thumbnail width**: The width of deal image in deal list calculated by Bootstrap framework's 12 grid column. You can select one of the 3 options and refresh your front-end deal list until it looks good for you. This option is only used if you only have 1 column in your deal list.
*   **Default pagination limit**: How many deals the deal list shows per page by default.
*   **Show sorting options**: Let visitors choose how the deals in the deal list are sorted.
*   **Show price tag**: Show price tag at the top left corner of every deal in deal list. The price tag shows how much the customer saves. It shows the fixed discount, the discount percent, or the saving on a deal which has an original price and a sale price.
*   **Show original and sale prices**: Show original price and sale price of deal if it has these prices.
*   **Price tag for deals with a sale price**: Choose how the price tag shows the saving on a deal which has an original price and a sale price. **Saved amount** shows the money saved, for example -$60.00. **Saved percentage** shows the percent saved, for example -60%. If the original price is $100 and the sale price is $40, the tag shows -$60.00 or -60%. The price tag does not show the sale price, because the price row under the photo already shows it. A deal with a fixed discount or a discount percent always shows that discount instead.
*   **Show featured deals first**: Show featured deals at the top of the deal list.
*   **Show ribbon for featured deals**: Show a ribbon in the top right corner of featured deals.

A deal which is paid in advance always shows the saving in its price tag, even if **Show price tag** is off. Before version 4.0 this deal showed its sale price there.

The saved percentage is always rounded down, so the tag never promises more than the real saving.

### If you upgrade from an older version

Older versions could show the sale price in the price tag, right above the same sale price. Version 4.0 shows the saving there instead, so that option is gone.

If your site used it, the upgrade does two things for you:

*   **Price tag for deals with a sale price** is set to **Saved percentage**.
*   If the price tag was the only place your deal list showed a price, **Show original and sale prices** is turned on, so the price stays on the card.

You can change both options after the upgrade.
