---
title: Submit a new deal
weight: 20
---
To submit a new deal on the site, a merchant needs permission. Please read [Permissions](/configuration/permissions/).

In the deal list, the merchant clicks the `New deal` button to submit a new deal.

The form has the following fields:

*   **Title**: The deal's name.
*   **Category**: The category the deal is in.
*   **Image**: The merchant clicks the `Select` button to open a popup and select an uploaded image. The merchant can also upload a new image.
*   **Discount info**: As in the back-end deal form, choose one of 3 discount types, or `None` if the deal's discount is different. This field is only shown if the `Price and discount fields` option is enabled in the component's Options.
    *   Original price and sale price. For example: was $50, now $30.
    *   Fixed discount amount. For example: $10 off orders over $50.
    *   Percentage discount. For example: 10% off orders over $50.
*   **Description**: The deal's description.
*   **Fine print**: The deal's terms and conditions.
*   **Coupon quantity**: This field is only shown if `Limit coupon quantity` in the component's configuration is enabled. Enter how many coupons customers can get. For an unlimited quantity, enter `0`.
*   **Starting time**: The date and time your deal starts.
*   **Ending time**: The date and time your deal ends. After that, visitors no longer see it.
*   **Schedule**: `Run continuously` runs the deal all the time between the starting and ending times. `Repeat weekly` runs it only on the days and hours you choose.
*   **Days**, **From** and **Until**: Only shown with `Repeat weekly`. The days of the week and the time of day when the deal is live, in the site's time zone. If `Until` is earlier than `From`, the deal runs past midnight into the next day.
*   **Status**: Only published deals are shown in the deal list. Unpublish your deal to stop it.

![/images/deal_frontend_form.png](/images/deal_frontend_form.png)

This is how a deal that runs every Tuesday and Wednesday from 14:00 to 17:00 looks in the form:

![/images/deal-form-4-0-schedule-frontend.png](/images/deal-form-4-0-schedule-frontend.png)

Between two time slots, visitors do not see the deal in the deal list. The deal page says when it is back. Read more in [Starting time, ending time and schedule](/create-new-deal-in-back-end/#starting-time-ending-time-and-schedule).

There is no `Access` field in this form. The deal gets the site's default access level, and an administrator can change it in the back-end.

If you integrate with a membership component, the form has no `Starting time` and `Ending time`, because the membership plan sets them. The `Schedule` fields are still shown.

## After the deal is approved

A new deal waits for an administrator to approve it, unless `Auto approve new deals` is enabled in the component's Options.

What the merchant can change after that depends on the `Merchant can edit published deals` option in the component's Options:

*   `Enabled`: the merchant can still change every field.
*   `Disabled`: the merchant can only change `Starting time`, `Ending time`, the schedule fields and `Status`. The other fields are shown but cannot be changed.

## Saving the deal

The merchant receives the message `Item successfully submitted.` if the deal is saved.

![/images/deal_frontend_merchant_list_saved.png](/images/deal_frontend_merchant_list_saved.png)

If something in the form is wrong, the deal is not saved. The form opens again with everything the merchant entered, and a message says what to fix. For example:

*   `Save failed with the following error: The ending time must be later than the starting time.`
*   `Save failed with the following error: Please choose at least one day of the week for the repeating schedule.`
*   `Save failed with the following error: The "From" and "Until" times of the repeating schedule must be different.`
