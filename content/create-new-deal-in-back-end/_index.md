---
title: Create new deal in back-end
weight: 80
---
To create a new deal in your Joomla!'s back-end as an administrator, you go to Components -> CM Live Deal -> Deals item to access the list of deals.

![/images/com_cmlivedeal_dashboard_new_deal_backend.png](/images/com_cmlivedeal_dashboard_new_deal_backend.png)

Click `New` button on the toolbar to create a new deal.

The form to create/edit deal looks like the screenshot below.

![/images/deal_backend_form.png](/images/deal_backend_form.png)

You need to enter your deal's details: name, description, fine print, starting time and ending time. You also need to select a merchant and a category for your deal.

To assign an image to your deal, you click `Select` button to open image popup. Before selecting/uploading a new image, you must select a merchant first, this merchant will be the owner of any image you upload while you are creating/modifying the deal, unless you select a different merchant. You can only assign 1 image to a deal.

If you enable the `Price and discount fields` option in the component's Options, `Discount info` is shown in the form. There are 3 types of discount, plus `None`:

*   None: no discount details are shown.
*   Original price and sale price. For example: was $50, now $30.
*   Fixed discount amount. For example: $10 off orders over $50.
*   Percentage discount. For example: 10% off orders over $50.

If a deal's discount does not fit these 3 types, select `None`. The original and sale prices can be shown in the deal list and the deal popup. A fixed or percentage discount can be shown in the deal list.

If you want to publish the deal, you need to set `Status` to `Published` and `Approval` to `Yes`. However the deal is visible to users or not also depends on starting and ending time.

`Approval` is for marking that if administrator has already reviewed and approved the deal which is submitted by merchant in front-end. When deal is approved, merchant can't change deal details any more, he/she only can modify starting and ending date.

If you enable `Limit coupon quantity` in the component's configuration, `Coupon quantity` field is displayed in the form. You can enter the quantity of coupons that you allow customers to capture, if you want unlimited quantity, you can enter 0.

If you enable advance payment in [configuration](/configuration/advance-payment/), the form has an `Advance payment` field. Turn it on to ask customers to pay in advance for this deal. The `Original price` and `Sale price` fields are always shown when advance payment is enabled.

![/images/advance_payment_deal_backend_form.png](/images/advance_payment_deal_backend_form.png)


Other fields in the form: 

*   **Meta description**: Meta description for SEO. Only administrator can see this field in back-end.
*   **Meta keywords**: Meta keywords for SEO. Only administrator can see this field in back-end.
*   **Impressions**: How many times this deal has been shown in the deal list.
*   **Clicks**: How many times visitors have clicked this deal to see its details.
*   **Created Date**: The date the deal is created.
*   **Created by**: The person who creates the deal.
*   **Modified Date**: The date the deal is modified the last time.
*   **Modified by**: The person who does the last modification.
*   **ID**: The ID of the deal.

After saving the deal, it is displayed in your deal list.

![/images/deal_backend_list_saved.png](/images/deal_backend_list_saved.png)