---
title: Merchant's deal list
weight: 10
---
To allow merchants access the list of their deals, you need to create a new menu item for `Deal Management` page.

Create a new menu item, select CM Live Deal -> Deal Management as the menu item type.

![/images/deal_frontend_menu.png](/images/deal_frontend_menu.png)

In your front-end, login as a merchant and access the new menu item, you can see the list of merchant's deals.

![/images/deal_frontend_merchant_list.jpg](/images/deal_frontend_merchant_list.png)

The list has 8 columns:

*   **Title**: Displays deal name and the name of the category which the deal is in.
*   **Impressions**: How many times the deal is showed in deal list to users.
*   **Clicks**: How many times users click the deal to view its details.
*   **Captured**: How many coupons of the deal that users have captured.
*   **Redeemed**: The number of redeemed coupons of the deal.
*   **Approved**: The deal is approved by administrators or is still in review.
*   **Published**: The deal is published.
*   **Ending time**: When the deal expires.

Click the deal's name to edit the deal. If the deal is not approved yet, the merchant can change every field. After the deal is approved, it depends on the `Merchant can edit published deals` option. When the option is disabled, the merchant can only change the starting time, the ending time, the schedule and the status. See [Submit a new deal](/create-new-deal-in-front-end/submit-new-deal/#after-the-deal-is-approved).

The `Ending time` column shows when the deal ends. A deal with a weekly schedule is also hidden from visitors between its time slots, before that time.