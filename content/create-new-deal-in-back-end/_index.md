---
title: Create new deal in back-end
weight: 80
---
To create a new deal in your Joomla!'s back-end as an administrator, go to `Components` -> `CM Live Deal` -> `Deals` to open the list of deals.

![/images/com_cmlivedeal_dashboard_new_deal_backend.png](/images/com_cmlivedeal_dashboard_new_deal_backend.png)

Click the `New` button on the toolbar to create a new deal.

The form to create or edit a deal looks like the screenshot below.

![/images/deal_backend_form.png](/images/deal_backend_form.png)

Enter your deal's details: name, description, fine print, starting time and ending time. You also need to select a merchant and a category for your deal.

To assign an image to your deal, click the `Select` button to open the image popup. Before you select or upload an image, you must select a merchant first. This merchant owns any image you upload while you create or change the deal, unless you select a different merchant. You can only assign 1 image to a deal.

If you enable the `Price and discount fields` option in the component's Options, `Discount info` is shown in the form. There are 3 types of discount, plus `None`:

*   None: no discount details are shown.
*   Original price and sale price. For example: was $50, now $30.
*   Fixed discount amount. For example: $10 off orders over $50.
*   Percentage discount. For example: 10% off orders over $50.

If a deal's discount does not fit these 3 types, select `None`. The original and sale prices can be shown in the deal list and the deal popup. A fixed or percentage discount can be shown in the deal list.

## Starting time, ending time and schedule

`Starting time` and `Ending time` are the first and last moment of the deal. Outside these times, visitors do not see the deal.

`Schedule` decides when the deal runs between those two times:

*   **Run continuously**: the deal is live all the time from the starting time to the ending time.
*   **Repeat weekly**: the deal is live only on the days and hours you choose. For example, a restaurant can run a lunch deal every Tuesday and Wednesday from 14:00 to 17:00, when it is quiet.

![/images/deal-form-4-0-schedule-backend.png](/images/deal-form-4-0-schedule-backend.png)

When you choose `Repeat weekly`, three more fields are shown:

*   **Days**: the days of the week when the deal is live. Choose at least one day.
*   **From**: the time of day the deal goes live.
*   **Until**: the time of day the deal stops.

The times use the site's time zone (`System` -> `Global Configuration` -> `Server` -> `Website Time Zone`).

If `Until` is earlier than `From`, the deal runs past midnight into the next day. For example, `Friday` from `22:00` until `02:00` runs from Friday 22:00 to Saturday 02:00.

The weekly schedule only works inside the starting and ending times. A deal with the schedule above and an ending time of 31 December stops on 31 December.

If the form is not filled in correctly, the deal is not saved and you see one of these messages:

*   `Please choose at least one day of the week for the repeating schedule.`
*   `The "From" and "Until" times of the repeating schedule must be different.`
*   `The ending time must be later than the starting time.`

### What visitors see

The deal list, the deal popup and the deal page show the schedule, for example `Every Tue, Wed, 14:00 to 17:00`. The countdown runs to the end of today's time slot, not to the deal's ending time.

Between two time slots, for example on a Monday:

*   The deal is not shown in the deal list or in the CM Live Deal modules, and the deal search does not find it.
*   Its deal page still opens. It says `Not available right now` and when the deal is back, for example `Back on Saturday, 19 September 2026 11:00`. Customers cannot capture a coupon until then.

![/images/deal-page-4-0-back-on.png](/images/deal-page-4-0-back-on.png)

Smart Search only uses the starting and ending times. It still lists the deal between time slots, and the link opens the deal page above.

The dashboard's `Live deals` tile uses the schedule too. See [Dashboard](/dashboard/).

## Status, approval and access

![/images/deal-form-4-0-access.png](/images/deal-form-4-0-access.png)

To publish the deal, set `Status` to `Published` and `Approval` to `Yes`. Visitors then see the deal between its starting and ending times, and inside its schedule.

`Approval` shows that an administrator has checked a deal that a merchant submitted in the front-end. Once a deal is approved, what the merchant can still change depends on the `Merchant can edit published deals` option. See [Submit a new deal](/create-new-deal-in-front-end/submit-new-deal/).

`Access` decides who can see the deal. It uses Joomla's access levels, the same as articles. For example, set it to `Registered` and only users who are logged in see the deal. Other visitors do not see it in the deal list, the CM Live Deal modules or Smart Search, and they cannot open its deal page.

Merchants do not see the `Access` field in the front-end form. A deal that a merchant creates gets the site's default access level (`System` -> `Global Configuration` -> `Site` -> `Default Access Level`). An administrator can change it later.

The deal list in the back-end has an `Access` column. To show only the deals with some access levels, open `Filter Options` and use `- Select Access -`.

## Other fields

If you enable `Limit coupon quantity` in the component's configuration, the `Coupon quantity` field is shown in the form. Enter how many coupons customers can capture. For an unlimited quantity, enter 0.

If you enable advance payment in [configuration](/configuration/advance-payment/), the form has an `Advance payment` field. Turn it on to ask customers to pay in advance for this deal. The `Original price` and `Sale price` fields are always shown when advance payment is enabled.

![/images/advance_payment_deal_backend_form.png](/images/advance_payment_deal_backend_form.png)

*   **Meta description**: Meta description for SEO. Only administrators can see this field, in the back-end.
*   **Meta keywords**: Meta keywords for SEO. Only administrators can see this field, in the back-end.
*   **Impressions**: How many times this deal has been shown in the deal list.
*   **Clicks**: How many times visitors have clicked this deal to see its details.
*   **Created Date**: The date the deal was created.
*   **Created by**: The person who created the deal.
*   **Modified Date**: The date the deal was last changed.
*   **Modified by**: The person who last changed the deal.
*   **ID**: The ID of the deal.

If you added [custom fields](/custom-fields/) to deals, they are shown near the bottom of the form.

## Saving the deal

If something in the form is wrong, the deal is not saved. The form opens again with everything you entered, and a message tells you what to fix. For example:

`Save failed with the following error: The ending time must be later than the starting time.`

After you save the deal, it is shown in your deal list.

![/images/deal_backend_list_saved.png](/images/deal_backend_list_saved.png)
