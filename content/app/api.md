---
title: App API
weight: 10
---

This page is for the developer of the mobile app. It describes every endpoint of the [CM Live Deal App API](/app/) add-on.

All endpoints are under `https://<your site>/api/index.php/v1/cmlivedealapp/`. They answer in [JSON:API](https://jsonapi.org/) format. Send request bodies as JSON (`Content-Type: application/json`); a normal form post works too.

## Sign in

```bash
curl -X POST https://example.com/api/index.php/v1/cmlivedealapp/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"linh","password":"secret","platform":"android","app_version":"1.0.0","device_name":"Pixel 8"}'
```

`platform` is `android` or `ios`. `app_version` and `device_name` are shown on the site's Devices list. The answer is `201`:

```json
{
  "data": {
    "type": "sessions",
    "id": "12",
    "attributes": {
      "id": 12,
      "token": "cmlda_3f1c…",
      "user_id": 427,
      "name": "Linh Nguyen",
      "username": "linh",
      "email": "linh@example.com"
    }
  }
}
```

Keep the token in the phone's secure storage. Send it with every request:

```
Authorization: Bearer cmlda_3f1c…
```

The token works until the customer signs out, the site signs the device out, or the account is blocked. Then every request answers `401` and the app should show the sign-in screen again. The site stores only a hash of the token.

To sign out, send `DELETE auth/session` with the token. It answers `204`.

## Errors

Every error has a `code` that does not change, so the app can choose what to show. `title` is a message in the site's language that you can show as it is.

```json
{"errors": [{"status": "409", "code": "already_captured", "title": "You already have a coupon for this deal."}]}
```

| Code | Status | When |
|---|---|---|
| `missing_credentials` | 400 | No username or password |
| `invalid_credentials` | 401 | Wrong username or password |
| `rate_limited` | 429 | Too many failed sign-ins. `Retry-After` gives the seconds to wait |
| `account_unavailable` | 403 | The account is blocked, not activated, or must reset its password |
| `mfa_enabled` | 403 | The account uses two-factor authentication. Send the customer to the website |
| `admin_account` | 403 | The account can sign in to the administrator |
| `api_login_denied` | 403 | The account's group may not sign in to the API |
| `session_ended` | 401 | The token no longer works. Sign in again |
| `not_app_session` | 400 | The request has no app token (for example a Joomla API token) |
| `not_found` | 404 | The deal, coupon or order is not available to this customer |
| `free_deal` | 409 | The deal is free. Take the coupon instead of paying |
| `own_deal` | 409 | Merchants cannot buy their own deals |
| `already_captured` | 409 | The customer already has a coupon for this deal |
| `sold_out` | 409 | The deal has no coupons left |
| `invalid_billing` | 422 | A billing field is missing or wrong. `title` says which |
| `method_not_native` | 409 | This payment method works only on the website. Use `me/checkout` |
| `order_refused` | 409 | A plugin on the site stopped the order. `title` says why |
| `payment_unavailable` | 502 | The payment service did not start the payment. Try again, or use the website |
| `not_paid` | 409 | The payment service has not confirmed the payment yet |

## Deals, cities and categories

These work without a token unless the site switched **Browse Without Signing In** off. With a token, the customer also sees deals limited to their access level, and each deal says whether they have its coupon (`captured`).

### GET deals

| Parameter | Meaning |
|---|---|
| `filter[search]` | Words to search for, like the website's search box |
| `filter[category]` | A category alias, from `GET categories` |
| `filter[city]` | A city alias, from `GET cities` |
| `filter[latitude]`, `filter[longitude]`, `filter[radius]` | Deals near the phone. The radius is one of the site's radius options, in km. The position is ignored when `filter[city]` is set |
| `sort` | `newest`, `ending_soon` or `nearest` (needs the position) |
| `page[offset]`, `page[limit]` | Paging. At most 50 per page |

`meta` has `total`, `offset` and `limit`. `links.next` is the next page, when there is one.

Each deal has `id`, `title`, `featured`, `category` (`alias`, `title`), `image`, `checkout` (`free` or `paid`), `price` (paid deals: `original`, `sale`, `advance`, `remaining`, each also `_formatted`), `discount`, `starting_time`, `ending_time`, `live`, `live_until`, `next_live`, `schedule`, `coupons_left`, `distance`, `merchant` (`name`, `address`, `latitude`, `longitude`) and `url`, the deal's page on the website. Times are ISO 8601 in UTC.

### GET deals/:id

One deal, with `description`, `fine_print` and the merchant's `about`, `phone`, `website` and social links as well. A deal the website would not show (unpublished, not approved, not started, ended, or above the customer's access level) answers `404 not_found`.

### GET cities and GET categories

The lists for the filters. Cities have `alias`, `name`, `latitude` and `longitude`; categories have `alias`, `title`, `parent_id` and `level`. Categories follow the customer's access level.

## The customer

These need a token.

* `GET me`: the customer's `name`, `username` and `email`.
* `GET me/coupons`: their coupons, newest first, paged like deals. Each has `code`, `status` (`active`, `redeemed` or `expired`), `captured`, `redeemed_time`, `expires` and the `deal` (`id`, `title`, `image`).
* `GET me/coupons/:id`: one coupon, with `qr_payload`, the text to show as a QR code at the counter, and the deal's description and the merchant's address and phone.
* `GET me/orders`: their orders, with `number`, `status` (`unpaid`, `paid` or `refunded`), `amount`, `payment_method`, `created`, `completed`, `coupon_id` (once paid) and the `deal`. `filter[status]` narrows the list.

## Take a free coupon

Use CM Live Deal's own API with the app token:

```bash
curl -X POST https://example.com/api/index.php/v1/cmlivedeal/deals/7/coupons \
  -H 'Authorization: Bearer cmlda_3f1c…'
```

The answer holds the new coupon and its `code`. The coupon is counted as taken in the app.

## Pay for a deal

First ask which payment methods the site has:

```bash
curl https://example.com/api/index.php/v1/cmlivedealapp/me/payment-methods \
  -H 'Authorization: Bearer cmlda_3f1c…'
```

```json
{"data": [
  {"type": "payment-methods", "id": "stripe", "attributes": {"id": "stripe", "name": "Stripe", "native": true}},
  {"type": "payment-methods", "id": "paypal", "attributes": {"id": "paypal", "name": "PayPal", "native": true}}
]}
```

`native: true` means the app can take the payment itself. For a method with `native: false`, use [the website checkout](#pay-on-the-website).

### 1. Place the order

```bash
curl -X POST https://example.com/api/index.php/v1/cmlivedealapp/me/payments \
  -H 'Authorization: Bearer cmlda_3f1c…' -H 'Content-Type: application/json' \
  -d '{"deal_id":7,"payment_method":"stripe","first_name":"Linh","last_name":"Nguyen","email":"linh@example.com","tos":true}'
```

Send the billing fields the site's checkout form asks for: `first_name`, `last_name` and `email`, and, when the site asks for them, `address_1`, `address_2`, `address_3`, `postal_box`, `city`, `state`, `postal_code`, `country` and `tos` (the Terms of Service tick). The site sets the amount itself. The answer is `201`:

```json
{"data": {"type": "payments", "id": "422", "attributes": {
  "id": 422, "number": 207, "deal_id": 7, "status": "unpaid",
  "amount": 4.5, "amount_formatted": "$4.50", "payment_method": "stripe", "coupon_id": null,
  "session": {"gateway": "stripe", "type": "stripe_payment_sheet", "payment_intent": "pi_…",
              "client_secret": "pi_…_secret_…", "publishable_key": "pk_live_…", "amount": 450, "currency": "usd"}
}}}
```

### 2. Take the payment

* `stripe_payment_sheet`: give `publishable_key` and `client_secret` to Stripe's payment sheet (`@stripe/stripe-react-native`).
* `paypal_approval`: the session has `approve_url`. Open it in an in-app browser tab. After the customer approves or cancels, PayPal returns to the site, which opens `<scheme>://payment/success?order=422` or `<scheme>://payment/cancel?order=422` in the app.

### 3. Confirm

```bash
curl -X POST https://example.com/api/index.php/v1/cmlivedealapp/me/payments/422/confirm \
  -H 'Authorization: Bearer cmlda_3f1c…'
```

The site asks Stripe or PayPal whether the payment went through. When it did, the answer is `200` with `"status": "paid"` and the `coupon_id`; read the coupon with `GET me/coupons/:id`. Before that it answers `409 not_paid`. You can call it again at any time; it never takes a payment twice.

If the app is closed before it confirms, the site still completes the order from the payment service's webhook. When the app opens again, read `GET me/orders`.

## Pay on the website

For a payment method with `native: false`, ask for a checkout link:

```bash
curl -X POST https://example.com/api/index.php/v1/cmlivedealapp/me/checkout \
  -H 'Authorization: Bearer cmlda_3f1c…' -H 'Content-Type: application/json' -d '{"deal_id":7}'
```

The answer has `url` and `expires`. Open `url` at once in an in-app browser tab (a Custom Tab on Android). It works once, for about a minute, and signs the customer in on the website, on the deal's checkout. When the checkout ends, the site opens the app with:

| Link | When |
|---|---|
| `<scheme>://checkout/coupon` | The deal was free after all, and the customer got the coupon |
| `<scheme>://checkout/success?order=421` | The customer paid; the coupon follows when the payment service confirms it |
| `<scheme>://checkout/cancel?order=421` | The customer cancelled, or the payment failed |

The links carry no coupon code, because another app could claim the same scheme. Read `GET me/orders` and `GET me/coupons` when the app opens again.

`<scheme>` is the site's **App Link Scheme**, `cmlivedeal` unless the site changed it. Register the same scheme in your app.
