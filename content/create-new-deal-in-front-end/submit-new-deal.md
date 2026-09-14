---
title: Submit a new deal
weight: 20
---
To submit a new deal on the site, you need permission. Please read [HERE](/configuration/permissions/)

In deal list, merchant can click `New deal` button to submit a new deal.

The form has the following fields:

*   **Title**: The deal's name.
*   **Category**: The category which the deal is in.
*   **Image**: Merchant can click `Select` button to open a popup and select an uploaded image, merchant can also upload a new image.
*   **Discount info**: As in the back-end deal form, choose one of 3 discount types, or `None` if the deal's discount is different. This field is only shown if the `Price and discount fields` option is enabled in the component's Options.
    *   Original price and sale price. For example: was $50, now $30.
    *   Fixed discount amount. For example: $10 off orders over $50.
    *   Percentage discount. For example: 10% off orders over $50.
*   **Description**: The deal's description.
*   **Fine print**: The deal's terms and conditions.
*   **Coupon quantity**: This field is only visible if `Limit coupon quantity` in the component's configuration is enabled. You can enter the quantity of how many coupon you want customers to get, if you want unlimited quantity you can enter `0`.
*   **Starting time**: The date and time your deal starts.
*   **Ending time**: The date and time your deal ends. After that, visitors no longer see it.
*   **Status**: Only published deals are shown in the deal list. Unpublish your deal to stop it.

![/images/deal_frontend_form.png](/images/deal_frontend_form.png)

You will receive message `Item successfully submitted.` if deal is saved successfully.

![/images/deal_frontend_merchant_list_saved.png](/images/deal_frontend_merchant_list_saved.png)
