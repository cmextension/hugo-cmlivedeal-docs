---
title: Events
weight: 10
---
CM Live Deal 4.0.0 and later fires events at important moments. You can write your own Joomla plugin, listen to these events, and change what CM Live Deal does. You never edit a CM Live Deal file, so your work is safe when you update the extension.

Here are some of the things you can do with events:

* Give coupons a code in your own format.
* Send a deal to another system as soon as a merchant saves it.
* Stop a customer from taking a coupon, and tell them why.
* Refuse a coupon at the counter, for example when the deal ended too long ago.
* Send order details to your accounting software once the payment is in.
* Learn when a deal is published, approved, featured or expired.

## Two kinds of event

A **before** event runs *before* CM Live Deal does something. You can change the data, and with all of these events you can also stop the action.

An **after** event runs *after* CM Live Deal has done something. You cannot change anything, so use these events to tell another system what happened.

## Write your first plugin

A plugin needs three files. Put them in a folder called `myplugin`, make a ZIP file of the folder, then install the ZIP in Joomla.

Your plugin must be in the `cmlivedeal` plugin group. After installing it, go to `System` → `Plugins`, find your plugin, and enable it.

### File 1: `myplugin.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="5.4" type="plugin" group="cmlivedeal" method="upgrade">
    <name>My Live Deal Plugin</name>
    <version>1.0.0</version>
    <author>Your Name</author>
    <namespace path="src">MyCompany\Plugin\CMLiveDeal\MyPlugin</namespace>

    <files>
        <folder plugin="myplugin">services</folder>
        <folder>src</folder>
    </files>
</extension>
```

### File 2: `services/provider.php`

```php
<?php

defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use MyCompany\Plugin\CMLiveDeal\MyPlugin\Extension\MyPlugin;

return new class () implements ServiceProviderInterface {
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin = new MyPlugin(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('cmlivedeal', 'myplugin')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

### File 3: `src/Extension/MyPlugin.php`

```php
<?php

namespace MyCompany\Plugin\CMLiveDeal\MyPlugin\Extension;

defined('_JEXEC') or die;

use CMExtension\Component\CMLiveDeal\Administrator\Event\GenerateCouponCodeEvent;
use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;

final class MyPlugin extends CMSPlugin implements SubscriberInterface
{
    public static function getSubscribedEvents(): array
    {
        return [
            'onCMLDGenerateCouponCode' => 'makeMyCode',
        ];
    }

    public function makeMyCode(GenerateCouponCodeEvent $event): void
    {
        $event->setCode('SHOP-' . strtoupper(bin2hex(random_bytes(3))));
    }
}
```

Every new coupon now gets a code in your own format.

To use more events, add more lines to `getSubscribedEvents()`, with one method for each event.

## The context

Every event has a **context**. The context tells you where the event comes from, for example `com_cmlivedeal.deal`. Use it when you only want your code to run in one place:

```php
if ($event->getContext() !== 'com_cmlivedeal.deal') {
    return;
}
```

## Stop an action

Every before event lets you stop the action: call `cancel()` and give a reason, and CM Live Deal shows that reason to the person who tried.

```php
$event->cancel('Sorry, you cannot take this coupon.');
```

Put your text in a language file so that it works in every language:

```php
$event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_COUPON_REFUSED'));
```

## All events

Every event below is available since CM Live Deal 4.0.0, except the four payment events, which are older.

| Event | When it runs | What you can do |
|---|---|---|
| `onCMLDBeforeSaveDeal` | Before a deal is saved | Change the deal, or stop the save |
| `onCMLDAfterSaveDeal` | After a deal has been saved | Read only |
| `onCMLDBeforeChangeDealState` | Before a deal is published, approved or featured | Stop the change |
| `onCMLDAfterChangeDealState` | After the state changed, expiry included | Read only |
| `onCMLDBeforeCaptureCoupon` | Before a customer takes a coupon | Stop the capture |
| `onCMLDGenerateCouponCode` | When a new coupon needs its code | Give your own code |
| `onCMLDAfterCaptureCoupon` | After the coupon exists | Read only |
| `onCMLDBeforeRedeemCoupon` | Before a coupon is redeemed, or a redemption undone | Stop the change |
| `onCMLDAfterRedeemCoupon` | After the coupon was redeemed, or the redemption undone | Read only |
| `onCMLDBeforeCreateOrder` | Before a new order is saved | Change the order, or stop it |
| `onCMLDAfterCreateOrder` | After a new order has been saved | Read only |
| `onCMLDAfterChangeOrderStatus` | After the status of an order changed | Read only |
| `onCMLDAfterOrderPaid` | After an order was paid and its coupon exists | Read only |
| `onCMLDGetPaymentIdentity` | When CM Live Deal asks which gateways exist | Offer your own gateway |
| `onCMLDGetPaymentForm` | When the customer has to pay | Render your payment form |
| `onCMLDValidateCallback` | When the customer comes back from the gateway | Say whether the payment is good |
| `onCMLDProcessWebhook` | When the gateway calls the site directly | Handle the notification |

All event classes are in the namespace `CMExtension\Component\CMLiveDeal\Administrator\Event`.

Every event also has the method `getContext()`.

## Deal events

### onCMLDBeforeSaveDeal

Runs before a deal is saved. It runs wherever the save comes from: the back end, the merchant's own deal form on the site, and the Web Services API.

**Methods:** `getDeal()`, `isNew()`, `cancel($reason)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.deal`

`getDeal()` gives you the deal table object. Change its properties and your change is saved with the deal. `isNew()` is `true` for a new deal and `false` when an existing one is edited.

```php
public function tidyDeal(BeforeSaveDealEvent $event): void
{
    $deal = $event->getDeal();

    $deal->title = ucfirst(trim($deal->title));

    if (str_contains(strtolower($deal->title), 'free beer')) {
        $event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_TITLE_NOT_ALLOWED'));
    }
}
```

### onCMLDAfterSaveDeal

Runs after the deal has been saved.

**Methods:** `getDeal()`, `isNew()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.deal`

```php
public function announceDeal(AfterSaveDealEvent $event): void
{
    if (!$event->isNew()) {
        return;
    }

    $this->postToMyApi(['id' => $event->getDeal()->id, 'title' => $event->getDeal()->title]);
}
```

### onCMLDBeforeChangeDealState

Runs before a deal is published, unpublished, approved, disapproved, featured or unfeatured. One event covers all the deals the administrator selected.

**Methods:** `getDealIds()`, `getAction()`, `getValue()`, `cancel($reason)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.deal`

`getAction()` is `publish`, `approve` or `feature`. `getValue()` is the value the state is changing to, so `publish` with `0` means the deal is being taken off the site.

```php
public function keepDealsOnline(BeforeChangeDealStateEvent $event): void
{
    if ($event->getAction() === 'publish' && $event->getValue() === 0) {
        $event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_NO_UNPUBLISHING'));
    }
}
```

### onCMLDAfterChangeDealState

Runs after the state changed. The Task Scheduler fires it too when it takes deals off the site because they have ended; the action is then `expire`.

**Methods:** `getDealIds()`, `getAction()`, `getValue()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.deal`

```php
public function noticeExpiry(AfterChangeDealStateEvent $event): void
{
    if ($event->getAction() !== 'expire') {
        return;
    }

    foreach ($event->getDealIds() as $id) {
        $this->postToMyApi(['deal' => $id, 'state' => 'expired']);
    }
}
```

## Coupon events

### onCMLDBeforeCaptureCoupon

Runs before a customer takes a coupon. CM Live Deal has already checked its own rules, so the coupon really is about to be created. It runs for a free deal, for a paid deal once the payment is in, and for a capture made through the Web Services API.

**Methods:** `getDealId()`, `getUserId()`, `cancel($reason)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.coupon`

`getUserId()` is `0` when the site allows guests to take coupons.

```php
public function limitCoupons(BeforeCaptureCouponEvent $event): void
{
    if ($this->couponsToday($event->getUserId()) >= 3) {
        $event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_TOO_MANY_TODAY'));
    }
}
```

### onCMLDGenerateCouponCode

Runs when a new coupon needs its code.

**Methods:** `getDealId()`, `getUserId()`, `getCode()`, `setCode($code)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.coupon`

If you do nothing, CM Live Deal generates the code itself, using the length and the characters set in the component options. If you call `setCode()`, CM Live Deal uses your code instead.

CM Live Deal refuses your code and generates its own when the code is empty, when it is longer than 50 characters, or when another coupon already has it. A coupon always ends up with a code.

### onCMLDAfterCaptureCoupon

Runs after the coupon has been created.

**Methods:** `getCoupon()`, `getDealId()`, `getUserId()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.coupon`

`getCoupon()` gives you the whole coupon row, so `$event->getCoupon()->code` is the code the customer sees.

### onCMLDBeforeRedeemCoupon

Runs before a coupon is marked as redeemed, and before a redemption is taken back. It covers the merchant's scanner, the customer's own scanner and the Web Services API.

**Methods:** `getCoupon()`, `isRedeeming()`, `cancel($reason)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.coupon`

`isRedeeming()` is `true` when the coupon is being redeemed, and `false` when a redemption is being taken back.

This example refuses a coupon more than 30 days after the deal ended:

```php
public function checkAge(BeforeRedeemCouponEvent $event): void
{
    if (!$event->isRedeeming()) {
        return;
    }

    $coupon = $event->getCoupon();

    if (Factory::getDate($coupon->deal_ending_time) < Factory::getDate('-30 days')) {
        $event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_COUPON_TOO_OLD'));
    }
}
```

### onCMLDAfterRedeemCoupon

Runs after the coupon was redeemed, or after the redemption was taken back.

**Methods:** `getCoupon()`, `isRedeeming()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.coupon`

## Order events

Orders only exist when advance payment is switched on in the component options.

### onCMLDBeforeCreateOrder

Runs before a new order is saved. The order is not paid yet.

**Methods:** `getData()`, `setData(array $data)`, `cancel($reason)`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.order`

```php
public function checkOrder(BeforeCreateOrderEvent $event): void
{
    $data = $event->getData();

    if (str_ends_with($data['email'], '@blocked-domain.test')) {
        $event->cancel(Text::_('PLG_CMLIVEDEAL_MYPLUGIN_EMAIL_NOT_ALLOWED'));

        return;
    }

    $data['last_name'] = ucwords(strtolower($data['last_name']));

    $event->setData($data);
}
```

### onCMLDAfterCreateOrder

Runs after the new order has been saved. The customer is about to be sent to the payment gateway.

**Methods:** `getOrderId()`, `getOrderNumber()`, `getData()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.order`

### onCMLDAfterChangeOrderStatus

Runs whenever the status of an order changes: when the payment comes in, and when an administrator changes the status by hand in the back end.

**Methods:** `getOrderId()`, `getStatus()`, `getPreviousStatus()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.order`

```php
public function followStatus(AfterChangeOrderStatusEvent $event): void
{
    Log::add(
        'Order ' . $event->getOrderId() . ': '
        . $event->getPreviousStatus() . ' to ' . $event->getStatus(),
        Log::INFO,
        'my-plugin'
    );
}
```

### onCMLDAfterOrderPaid

Runs after the order was paid, its coupon was created and its email was sent. This is the best event to use for talking to another system.

**Methods:** `getOrderId()`, `getTransactionId()`, `getCoupon()`.

**Since:** 4.0.0

**Context:** `com_cmlivedeal.order`

`onCMLDAfterChangeOrderStatus` also runs for the same payment. Use that one to follow the status, and this one when you need the coupon as well.

```php
public function sendToAccounting(AfterOrderPaidEvent $event): void
{
    $this->postToMyApi([
        'order'          => $event->getOrderId(),
        'transaction_id' => $event->getTransactionId(),
        'coupon'         => $event->getCoupon()->code,
    ]);
}
```

## Payment events

These four events are how the PayPal and Stripe plugins are built, and you can write a plugin for another gateway the same way. The easiest start is to copy `plugins/cmlivedeal/stripe` and extend `CMLiveDealPaymentPlugin`, which subscribes to all four for you.

One rule to know: CM Live Deal reads the gateway id out of the URL with Joomla's `word` filter, which keeps only letters. Give your gateway an id with no digits in it, or the customer's return from the gateway will not be recognised.

### onCMLDGetPaymentIdentity

Runs when CM Live Deal builds the list of payment methods for the checkout form.

**Methods:** `addPaymentMethod(array $method)`.

**Since:** 3.0.0

The array needs an `id` and a `name`.

### onCMLDGetPaymentForm

Runs when the customer has to pay. Give back the HTML that sends them to your gateway.

**Methods:** `getArgument('data')`, `setFormHtml($html)`.

**Since:** 3.0.0

The `data` argument holds the billing details, the deal name, the total and the order number.

### onCMLDValidateCallback

Runs when the customer comes back from the gateway.

**Methods:** `setResult(array $result)`.

**Since:** 3.0.0

The result needs `valid`, `gateway_id`, `order_id` and `transaction_id`. CM Live Deal completes the order only when `valid` is `true` and `gateway_id` is the gateway in the URL.

### onCMLDProcessWebhook

Runs when the gateway calls the site directly, without a browser. Your plugin decides the HTTP status code, because only it can tell whether the request is authentic.

**Methods:** `setResult(array $result)`.

**Since:** 4.0.0

The result needs `handled`, `gateway_id`, `complete_order`, `order_id`, `transaction_id` and `status`.

## Tips

* In a payment plugin, take the amount to charge from the order row, never from the request. The browser can change anything it sends. Before you mark an order as paid, check the paid amount and currency against the same row:

  ```php
  $query = $db->getQuery(true)
      ->select($db->quoteName(['id', 'amount', 'order_status']))
      ->from($db->quoteName('#__cmlivedeal_orders'))
      ->where($db->quoteName('order_number') . ' = :number')
      ->bind(':number', $orderNumber);
  $order = $db->setQuery($query)->loadObject();
  ```

* Keep your plugin fast. Every event runs while somebody is waiting for a page.
* Only stop an action when you have a good reason. The customer or the merchant sees the result.
* Use `getContext()` to make sure your code runs only where you want it.
* Test your plugin on a test site before you use it on a live site.
