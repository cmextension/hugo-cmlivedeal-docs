---
title: Advance payment
weight: 30
---

When you enable advance payment in [the component's options](/configuration/advance-payment/) and in [the deals you create in the back end](/create-new-deal-in-back-end/), customers pay part of the price to get the coupons of these deals.

The amount to pay is shown in the deal list

![/images/advance_payment_deal_list.png](/images/advance_payment_deal_list.png)

and on the deal page.

![/images/advance_payment_deal_detail.png](/images/advance_payment_deal_detail.png)

To receive payments, you need to [set up and enable a payment plugin](/configuration/payment-plugins/).

## Checkout

At checkout, the customer fills in the billing details, chooses a payment method, accepts the terms of service if you show them, and clicks **Pay**.

The payment method is required. If only one payment plugin is enabled, it is selected for the customer. You can also hide the choice then, with **Hide a single payment method** in the options.

![/images/advance_payment_checkout.png](/images/advance_payment_checkout.png)

What happens next depends on the payment method:

* **PayPal with the REST API**: The checkout page shows the PayPal buttons. The customer pays in a PayPal window and does not leave your site. The coupon is ready as soon as the payment is done.
* **PayPal Payments Standard (legacy)**: The page counts down a few seconds and then takes the customer to PayPal. After paying, the customer comes back to your site. The coupon is ready when PayPal confirms the payment to your site, usually within a few seconds.
* **Stripe**: The customer is taken to Stripe's payment page. After paying, the customer comes back to your site and the coupon is ready.

The customer also gets an order confirmation email.

## When the payment does not go through

If the customer cancels, or the payment fails, the customer comes back to the cancel page. When the payment failed, the page shows the reason, for example "Your payment was not successful. Please try again or contact us for assistance." Below it, the page shows your **Cancellation Message** from the [options](/configuration/advance-payment/).

The order stays unpaid and no coupon is created. The customer can start the checkout again.
