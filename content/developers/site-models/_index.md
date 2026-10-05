---
title: Site models
weight: 25
---
CM Live Deal 4.0.0 and later lets you use its site models from your own code. You get the same deals and coupons a visitor sees on the site, with the same rules: only published, approved and running deals, and only the deals the user's access levels allow.

You can use the models outside the site too, for example in a Web Services API plugin for a mobile app, in a console command, or in a scheduled task. There is no menu item there, so the models use the component options.

## Get the models

Ask the component for its MVC factory, then create the model with `ignore_request`. With `ignore_request`, the model does not read the request or the visitor's session. You give it every filter yourself with `setState()`.

```php
use Joomla\CMS\Factory;

$factory = Factory::getApplication()
    ->bootComponent('com_cmlivedeal')
    ->getMVCFactory();

$model = $factory->createModel('Deals', 'Site', ['ignore_request' => true]);
```

## List deals

The `Deals` model gives you the deals that are on the site right now. These are the filters you can set:

| State | What it does |
|---|---|
| `filter.keyword` | Search in the deal title, the description, and the merchant's name and description |
| `filter.category` | The alias of a category. Deals in its subcategories are included |
| `city` | The alias of a city. Only deals of merchants within the city's radius |
| `filter.latitude`, `filter.longitude` | Only deals of merchants near this point. Used when no city is set |
| `radius` | The distance in kilometres for a city or a point, from the list of radius options in the component options |
| `list.ordering`, `list.direction` | The sort order: `a.starting_time`, `a.ending_time` or `distance`, and `asc` or `desc`. Any other value gives the default, `a.starting_time` and `desc`, the newest deals first |
| `list.start`, `list.limit` | Paging. A limit of `0` gives you all deals |

```php
$model = $factory->createModel('Deals', 'Site', ['ignore_request' => true]);
$model->setState('filter.keyword', 'coffee');
$model->setState('filter.category', 'food-drink');
$model->setState('city', 'ho-chi-minh');
$model->setState('list.start', 0);
$model->setState('list.limit', 20);

$deals = $model->getItems();
$total = $model->getTotal();

foreach ($deals as $deal) {
    echo $deal->id . ': ' . $deal->title . "\n";
}
```

A search near a point only works when `Geolocation service` is set to `HTML5 Geolocation` or `Maxmind` in the component options. With a city or a point, every deal also has a `distance` in kilometres, and you can sort by it with `list.ordering` set to `distance`.

When `Show featured deals first` is on in the component options, featured deals always come first, whatever the sort order.

## Get one deal

The `Deal` model gives you one deal by its id. It gives you an empty object when the deal does not exist, is not on the site, or the user may not see it, so check the id:

```php
$deal = $factory->createModel('Deal', 'Site', ['ignore_request' => true])
    ->getItem(12);

if (empty($deal->id)) {
    // The deal is not available.
}
```

## List a customer's coupons

The `Coupons` model gives you the coupons of one customer, newest first. Tell it which customer with `setCurrentUser()`. It never shows the coupons of another customer, and it shows nothing for a guest.

```php
use Joomla\CMS\User\UserFactoryInterface;

$user = Factory::getContainer()
    ->get(UserFactoryInterface::class)
    ->loadUserById($userId);

$model = $factory->createModel('Coupons', 'Site', ['ignore_request' => true]);
$model->setCurrentUser($user);
$model->setState('filter.search', '');
$model->setState('list.ordering', 'a.created');
$model->setState('list.direction', 'desc');
$model->setState('list.start', 0);
$model->setState('list.limit', 20);

foreach ($model->getItems() as $coupon) {
    echo $coupon->code . ' for ' . $coupon->deal_name . "\n";
}
```

`filter.search` finds coupons by their code or by the title of their deal. You can sort by `a.created`, `a.code` or `a.redeemed_time`.

## Place an order and take the payment yourself

Code that has its own payment screen, such as a mobile app, places the order through the same rules as the checkout page and then asks the order's gateway to start the payment. Since 4.0.0.

```php
use CMExtension\Component\CMLiveDeal\Administrator\Exception\CheckoutException;

$checkout = $factory->createModel('Checkout', 'Site', ['ignore_request' => true]);
$orders   = $factory->createModel('Order', 'Site', ['ignore_request' => true]);
$checkout->setCurrentUser($user);
$orders->setCurrentUser($user);

try {
    // The same fields as the checkout form. The amount, status and owner are set by CM Live Deal.
    $order = $checkout->placeOrder($dealId, [
        'first_name'     => 'Linh',
        'last_name'      => 'Nguyen',
        'email'          => 'linh@example.com',
        'payment_method' => 'stripe',
    ]);
} catch (CheckoutException $e) {
    // getReason() is not_found, login_required, already_captured, sold_out, no_form,
    // invalid or refused. getMessage() is a translated message for the customer.
    echo $e->getReason() . ': ' . $e->getMessage();

    return;
}

$session = $orders->createPaymentSession($order['id'], $returnUrl, $cancelUrl);

if ($session === null) {
    // This gateway cannot take a payment outside the checkout page.
}

// Later, when your payment screen says the customer has paid:
$coupon = $orders->confirmPaymentSession($order['id']);
```

`confirmPaymentSession()` asks the gateway, never your code, whether the payment went through. It gives back the coupon once the order is paid, or `null`. It is safe to call it again. To know which gateways can do this, read `CMLiveDealHelper::getPaymentMethods()`: those that can have `'sessions' => true`. See [onCMLDCreatePaymentSession](../events/#oncmldcreatepaymentsession) for how a gateway answers.

## Tips

* Always use `ignore_request`. Without it, the model reads the filters of the visitor who is on the site, which is not what you want in an API or a console command.
* Never give a customer id from the request to the `Coupons` model. Take the user from the session or the API token, so a customer can only see their own coupons.
* The deal list uses the access levels of the user the application knows. In a Web Services request, that is the user of the API token.
* Never take the order amount or the order's owner from the request. `placeOrder()` sets them itself.
