---
title: Web Services API
weight: 20
---

CM Live Deal 4.0.0 and later can be used over Joomla's Web Services API. Your own program talks to the site over HTTP and gets JSON back, so you can:

* Build a mobile app that shows deals and lets people take coupons.
* Build your own scanner that reads a coupon code and redeems it at the counter.
* Copy deals in from another system, or copy orders out to your accounting software.
* Read merchants, cities, coupons and orders for a report.

Everything is under `/api/index.php/v1/cmlivedeal/`.

## Turn the API on

Four settings have to be right. The [Setup card](/dashboard/setup-card/) on the CM Live Deal dashboard checks the last two for you and tells you when one is missing.

### 1. Enable the Web Services plugin

Go to `System > Plugins`, search for `CM Live Deal` and open the `Web Services - CM Live Deal` plugin. It is **disabled** after a fresh install, because most sites do not need an API. Set `Status` to `Enabled` and save.

![/images/api-4-0-plugin.png](/images/api-4-0-plugin.png)

Without this plugin the routes below do not exist and every request answers `404`.

### 2. Allow Web Services Login

Go to `System > Global Configuration > Permissions`, click the group whose members will call the API, set `Web Services Login` to `Allowed` and save.

![/images/api-4-0-permissions.png](/images/api-4-0-permissions.png)

Only Super Users have this permission at first. If your merchants will use a scanner app, give it to your merchant group. If your customers will use a mobile app, give it to `Registered`.

### 3. Allow those groups to hold a token

This is a second, separate setting. Go to `System > Plugins` and open `User - Joomla API Token`. Add the same groups to `Allowed User Groups` and save.

![/images/api-4-0-token-plugin.png](/images/api-4-0-token-plugin.png)

It lists only Super Users at first. An account with `Web Services Login` but no token still gets a bare `401`.

### 4. Get a token

Every user has their own token. Log in as that user, open their profile, save it once, then open it again and read the `Joomla API Token` tab.

![/images/api-4-0-token.png](/images/api-4-0-token.png)

A few things to know about tokens:

* **You can only see your own token.** A Super User editing another account sees the `Active` and `Reset` switches but not the token itself, so each person has to fetch their own.
* Set `Reset` to `Yes` and save to replace a token, for example when a phone is lost.
* Set `Active` to `No` to block the token without deleting it.
* A token is a password. Keep it out of URLs, logs and app stores.

## Send a request

Put the token in the `X-Joomla-Token` header:

```bash
curl -H "X-Joomla-Token: YOUR_TOKEN" \
     -H "Accept: application/vnd.api+json" \
     "https://example.com/api/index.php/v1/cmlivedeal/deals?page[limit]=2"
```

The answer is [JSON:API](https://jsonapi.org/):

```json
{
  "links": {
    "self": "https://example.com/api/index.php/v1/cmlivedeal/deals?page[limit]=2",
    "next": "https://example.com/api/index.php/v1/cmlivedeal/deals?page[limit]=2&page[offset]=2",
    "last": "https://example.com/api/index.php/v1/cmlivedeal/deals?page[limit]=2&page[offset]=36"
  },
  "data": [
    {
      "type": "deals",
      "id": "9044",
      "attributes": {
        "id": 9044,
        "title": "Any large coffee",
        "alias": "any-large-coffee-451",
        "user_id": 451,
        "category_id": 8,
        "starting_time": "2025-09-14 10:03:44",
        "ending_time": "2026-10-19 10:03:44",
        "published": 1,
        "approved": 1,
        "category_title": "Food & Drink",
        "merchant_username": "merchant07",
        "captured": 34,
        "redeemed": 18,
        "dress-code": ""
      },
      "relationships": {
        "category": {"data": {"type": "categories", "id": "8"}},
        "merchant": {"data": {"type": "users", "id": "451"}}
      }
    }
  ],
  "meta": {"total-pages": 19}
}
```

Send a body as plain JSON with `Content-Type: application/json`. You do **not** wrap it in `data`/`attributes`:

```bash
curl -X PATCH \
     -H "X-Joomla-Token: YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"title": "A new title"}' \
     "https://example.com/api/index.php/v1/cmlivedeal/deals/9054"
```

## The routes

`{base}` below is `https://example.com/api/index.php/v1`.

### Deals

| Method | Route | Who |
| --- | --- | --- |
| `GET` | `{base}/cmlivedeal/deals` | Manage |
| `GET` | `{base}/cmlivedeal/deals/{id}` | Manage |
| `POST` | `{base}/cmlivedeal/deals` | Create |
| `PATCH` | `{base}/cmlivedeal/deals/{id}` | Edit, or Edit Own for their own deal |
| `DELETE` | `{base}/cmlivedeal/deals/{id}` | Delete |
| `GET` | `{base}/cmlivedeal/deals/categories` | Manage |
| `POST` `PATCH` `DELETE` | `{base}/cmlivedeal/deals/categories[/{id}]` | The matching category permission |

### Coupons

| Method | Route | Who |
| --- | --- | --- |
| `GET` | `{base}/cmlivedeal/coupons` | Manage |
| `GET` | `{base}/cmlivedeal/coupons/{id}` | Manage, the customer holding it, or the merchant whose deal it is for |
| `GET` | `{base}/cmlivedeal/coupons/code/{code}` | The same three |
| `POST` | `{base}/cmlivedeal/deals/{deal_id}/coupons` | Any logged-in account: takes a coupon for itself |
| `PATCH` | `{base}/cmlivedeal/coupons/{id}/redeem` | The merchant whose deal it is, or Edit State |
| `PATCH` | `{base}/cmlivedeal/coupons/{id}/unredeem` | The same |
| `POST` `PATCH` `DELETE` | `{base}/cmlivedeal/coupons[/{id}]` | Create / Edit / Delete |

### Everything else

| Method | Route | Who |
| --- | --- | --- |
| `GET` `POST` `PATCH` `DELETE` | `{base}/cmlivedeal/cities[/{id}]` | Manage to read, the matching permission to write |
| `GET` `POST` `PATCH` `DELETE` | `{base}/cmlivedeal/orders[/{id}]` | The same |
| `GET` `POST` `PATCH` `DELETE` | `{base}/cmlivedeal/plans[/{id}]` | The same |
| `GET` | `{base}/cmlivedeal/merchants` | Manage |
| `GET` | `{base}/cmlivedeal/merchants/{id}` | Manage |

Merchant profiles are **read only** over the API. A profile is written by the `User - CM Live Deal's Merchant Profile` plugin when the user account is saved, so there is no form to write against. `{id}` is the profile's own id, not the user id — use `filter[merchant]` to find a profile by user id.

### Custom fields

Joomla's own field routes, bound to CM Live Deal's two contexts:

| Route | What |
| --- | --- |
| `{base}/fields/cmlivedeal/deals[/{id}]` | Fields for deals |
| `{base}/fields/groups/cmlivedeal/deals[/{id}]` | Field groups for deals |
| `{base}/fields/cmlivedeal/categories[/{id}]` | Fields for deal categories |
| `{base}/fields/groups/cmlivedeal/categories[/{id}]` | Field groups for deal categories |

Create your fields on the `Fields` screen (see [Custom fields](/custom-fields/)) and use these routes to read them. The values themselves come back with each deal, so you rarely need them. See [Custom fields in a deal](#custom-fields-in-a-deal) below.

## Who can call what

Reads are **not** open to everybody, because an order carries a buyer's name, email address and postal address. A list needs the `Access Administration Interface` permission (`core.manage`) for CM Live Deal — the same permission that lets someone open the component in the administrator.

| The caller | What they can do |
| --- | --- |
| No token | Nothing: `401` |
| A registered customer | Take a coupon; read a coupon they hold, by id or by code |
| A merchant | The same, plus read and redeem coupons for their own deals. Reading the deal, coupon and order lists is refused unless you grant them `Access Administration Interface` |
| A Super User, or anyone with CM Live Deal permissions | Everything the permissions allow |

So a customer reading a list gets:

```json
{"errors": [{"title": "Access Denied", "code": 403}]}
```

**Be careful with `Edit State`.** That permission for CM Live Deal lets an account redeem **any** coupon on the site, not only coupons for its own deals. Do not give it to your merchant group unless you mean it.

## Read a list

### Filters

Pass them as `filter[name]=value`. A name that is not in this table is ignored.

| Resource | Filters |
| --- | --- |
| `deals` | `search` (title), `published` (`0`/`1`), `merchant` (user id), `access` (view level id) |
| `coupons` | `search` (code), `redeemed` (`0`/`1`), `deal` (deal id), `customer` (user id), `captured` (`YYYY-MM-DD`) |
| `orders` | `search`, `status` (`unpaid`, `paid` or `refunded`) |
| `cities` | `search`, `published` |
| `plans` | `search` |
| `merchants` | `search` (name or address), `merchant` (user id) |

An order's `search` looks through the order number, the buyer's name, email address and postal address, the payment method, the coupon code and the deal title. On deals, coupons and orders you can also search `id:12` to go straight to one record.

```bash
curl -H "X-Joomla-Token: YOUR_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/coupons?filter[deal]=9053&filter[redeemed]=0"
```

### Paging

`page[limit]` and `page[offset]`, the same as the rest of the Joomla API. The default is 20 per page. `meta.total-pages` and the `next` and `last` links tell you where you are.

```bash
curl -H "X-Joomla-Token: YOUR_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/deals?page[limit]=50&page[offset]=50"
```

### Order

`list[ordering]` and `list[direction]` (`ASC` or `DESC`). A column that is not allowed is **ignored**, not refused, so check your spelling if the order looks wrong.

| Resource | Columns you can order by |
| --- | --- |
| `deals` | `a.id`, `a.title`, `a.starting_time`, `a.ending_time`, `a.impressions`, `a.clicks`, `a.published`, `a.approved`, `a.featured`, `a.access`, `captured`, `redeemed` |
| `coupons` | `a.id`, `a.code`, `a.redeemed`, `a.redeemed_time`, `a.created` |
| `orders` | `a.id`, `a.order_number`, `a.created`, `a.completed` |
| `cities` | `a.id`, `a.name`, `a.published` |
| `plans` | `a.id`, `a.title`, `a.deal_quantity`, `a.length`, `a.ordering` |
| `merchants` | `a.id`, `a.user_id`, `a.name`, `a.address` |

```bash
curl -H "X-Joomla-Token: YOUR_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/deals?list[ordering]=a.ending_time&list[direction]=ASC"
```

### Relationships

A deal points at its category and at the Joomla user account of its merchant. A coupon points at its deal and at the customer, and an order points at its deal and its coupon. A coupon taken by a guest has no customer, and an order that was never paid has no coupon, so those relationships are simply missing.

## Write a deal

`POST` to `{base}/cmlivedeal/deals` with at least a title, a merchant, a category and the two times:

```bash
curl -X POST \
     -H "X-Joomla-Token: YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "title": "Two coffees for one",
           "user_id": 452,
           "category_id": 8,
           "starting_time": "2026-09-15 00:00:00",
           "ending_time": "2026-12-31 23:59:59",
           "published": 1,
           "approved": 1
         }' \
     "https://example.com/api/index.php/v1/cmlivedeal/deals"
```

Leave a required field out and you get the form's own message:

```json
{"errors": [{"title": "Field required: Title\nField required: Starting time\nField required: Ending time"}]}
```

Things worth knowing:

* Times are the site's own time zone, in `YYYY-MM-DD HH:MM:SS`.
* `user_id` is the merchant's **Joomla user id**, not their merchant profile id.
* A price is only stored when you also set `discount_info`. Use `prices` with `original_price` and `sale_price`, `fixed` with `fixed_value`, `percent` with `fixed_percent`, or `none` (the default) for a deal with no price shown. Send a price without `discount_info` and it is saved as `0.00`.
* `approved` is what the `Deal Approval` option controls on the site. A deal created over the API is not approved unless you say so.
* `DELETE` answers `204` with no body, and a second `DELETE` of the same id answers `404`.

### Custom fields in a deal

Send a custom field by its **name**, at the top level of the body, next to `title`:

```bash
curl -X PATCH \
     -H "X-Joomla-Token: YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"dress-code": "Smart casual"}' \
     "https://example.com/api/index.php/v1/cmlivedeal/deals/9054"
```

Values come back the same way, as one more attribute on the deal.

## Coupons

### Take a coupon

`POST` to `{base}/cmlivedeal/deals/{deal_id}/coupons`. The coupon goes to the account the token belongs to — there is no way to take one for somebody else, and there are no guest coupons over the API.

```bash
curl -X POST -H "X-Joomla-Token: CUSTOMER_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/deals/9053/coupons"
```

```json
{
  "data": {
    "type": "coupons",
    "id": "666",
    "attributes": {
      "id": 666,
      "code": "M1MQH",
      "user_id": 426,
      "deal_id": 9053,
      "redeemed": 0,
      "created": "2026-09-16 01:19:31"
    }
  }
}
```

The same rules as the site apply, and each one has its own answer:

| What happened | Answer |
| --- | --- |
| The deal does not exist, is unpublished, is not approved, or is not running now | `404` `Resource not found` |
| The merchant tried to take a coupon for their own deal | `403` |
| This account already has a coupon for this deal | `400` `You already have a coupon for this deal.` |
| The deal has run out of coupons | `400` `...out of stock` |
| The deal has to be paid for | `400` `This deal requires payment. Send the customer to the checkout page on the site, because the API cannot take payments.` |
| One of your own plugins stopped it | `400`, carrying your plugin's own message |

Advance payment is the one thing the API cannot do, because paying goes through PayPal or Stripe in a browser. Send the customer to the deal page on the site instead. See [Advance payment](/configuration/advance-payment/).

### Find a coupon by its code

A scanner reads a code, not an id:

```bash
curl -H "X-Joomla-Token: MERCHANT_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/coupons/code/M1MQH"
```

An unknown code answers `404`. Codes are letters and digits only.

### Redeem and cancel

```bash
curl -X PATCH -H "X-Joomla-Token: MERCHANT_TOKEN" \
     "https://example.com/api/index.php/v1/cmlivedeal/coupons/666/redeem"
```

Both `redeem` and `unredeem` give you the whole coupon back, with `redeemed` set to `1` or `0`.

| What happened | Answer |
| --- | --- |
| The coupon is already redeemed | `400` `Coupon M1MQH has already been redeemed.` |
| The coupon was not redeemed and you called `unredeem` | `400` `This coupon has not been redeemed, so there is no redemption to cancel.` |
| The caller is not the merchant for that deal, and has no `Edit State` | `403` |
| One of your own plugins refused the redemption | `400`, carrying your plugin's own message |

The refusal message is the reason your own plugin gave, so a rule you write with [the events](/developers/events/) reaches the app at the counter, word for word.

## Errors

| Code | What it means |
| --- | --- |
| `401` | No token, a wrong token, a switched-off token, or the account may not log in to Web Services |
| `403` | The token is good but the account is not allowed to do this |
| `404` | No such route, no such record, or the Web Services plugin is disabled |
| `400` | The request was understood and refused; read `errors[0].title` for the reason |

Messages come back in the site's language, so an app can show them as they are.

## A worked example: a scanner app

A merchant's app scans a QR code, shows the customer the coupon, and redeems it when the merchant taps a button. Three calls, with the merchant's own token:

```bash
TOKEN="MERCHANT_TOKEN"
BASE="https://example.com/api/index.php/v1/cmlivedeal"
CODE="M1MQH"

# 1. The code the camera read. 404 means "no such coupon".
curl -s -H "X-Joomla-Token: $TOKEN" "$BASE/coupons/code/$CODE"

# 2. The deal it is for, to show the merchant what to hand over.
curl -s -H "X-Joomla-Token: $TOKEN" "$BASE/deals/9053"

# 3. Redeem it. 400 means it was already used.
curl -s -X PATCH -H "X-Joomla-Token: $TOKEN" "$BASE/coupons/666/redeem"
```

CM Live Deal also has a scanner built in, which needs no app at all. See [QR code scanner](/qr-code-scanner/).
