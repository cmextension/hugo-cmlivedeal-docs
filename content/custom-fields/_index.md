---
title: Custom fields
weight: 85
---

You can add your own fields to deals with Joomla's **custom fields**. For example, a restaurant site can add a `Dress code` field. Merchants fill it in on the deal form, and visitors see it in the deal popup and on the deal page.

You do not need a new option in CM Live Deal for this. Joomla already has many field types: text, list, radio, checkbox, calendar, URL, media and more.

## Where to find them

Go to `Components > CM Live Deal > Fields` to add a field. Go to `Components > CM Live Deal > Field Groups` to add a group.

![/images/custom-fields-4-0-list.png](/images/custom-fields-4-0-list.png)

The list at the top left lets you choose where a field belongs:

* **Deal**: the field is on the deal form in the back-end and on the merchant's deal form in the front-end. Both forms share the same fields.
* **Category**: the field is on the form of a CM Live Deal category. CM Live Deal does not show category fields on the site. Use them in your own template overrides.

A field that is not in a group is shown under `Fields`. A field in a group is shown under the group's name.

## Add a field to deals

1. Go to `Components > CM Live Deal > Fields`. Make sure `Deal` is selected at the top left.
2. Click `New`.
3. Enter a `Title` and choose a `Type`.
4. Optional: choose the categories in `Category`. The field is then only on deals in those categories. Leave it empty to show the field on every deal.
5. Click `Save & Close`.

Open a deal. The field is near the bottom of the form.

![/images/custom-fields-4-0-backend-form.png](/images/custom-fields-4-0-backend-form.png)

If you give a field categories and then change a deal's category, the form does not change at once. Save the deal first. When the form opens again, it shows the fields of the new category.

## Let merchants fill in the fields

Merchants see your fields on their deal form in the front-end.

![/images/custom-fields-4-0-frontend-form.png](/images/custom-fields-4-0-frontend-form.png)

By default they cannot change them: Joomla shows the fields but disables them. To let merchants fill them in:

1. Go to `Components > CM Live Deal > Options` and open the `Permissions` tab.
2. Select your merchant group.
3. Set `Edit Custom Field Value` to `Allowed`.
4. Click `Save & Close`.

The [Setup card](/dashboard/setup-card/#permission-to-fill-in-custom-fields) on the dashboard reminds you of this. It only shows the check when your site has at least one custom field for CM Live Deal.

You can also set `Edit Custom Field Value` on one field, in the `Permissions` tab of the field.

## Where visitors see the fields

Visitors see the fields in the deal popup and on the deal page. They do not see them on the deal cards in the list.

![/images/custom-fields-4-0-popup.png](/images/custom-fields-4-0-popup.png)

Edit the field and open the `Options` tab. `Automatic Display` decides where the field is shown:

* **After Title**: under the deal title.
* **Before Display Content**: above the description. This is the default.
* **After Display Content**: below the fine print.
* **Do not automatically display**: the field is not shown. It is still saved, so you can show it in your own override.

A field is only shown when it has a value or a `Default Value`. The field's `Access` decides who can see it. For example, set it to `Registered` to show it only to logged-in visitors.

On the same tab, `Search Index` adds the field to Smart Search. See [Smart Search](/smart-search/#what-is-searched).

## For developers

The fields are shown by the layout `deal/custom-fields.php`. To change it, copy

```
components/com_cmlivedeal/layouts/deal/custom-fields.php
```

to

```
templates/<your template>/html/layouts/com_cmlivedeal/deal/custom-fields.php
```

The layout gets these values in `$displayData`:

* `item`: the deal. Its fields are in `$item->jcfields`.
* `displayType`: `1` for After Title, `2` for Before Display Content, `3` for After Display Content.

The layout sends the fields to Joomla's own `fields.render` and `field.render` layouts. Your overrides of those layouts work for deals too:

* `templates/<your template>/html/layouts/com_fields/field/render.php` changes every custom field on the site.
* `templates/<your template>/html/layouts/com_cmlivedeal/field/render.php` changes only the fields of deals.

The deal wraps the fields in `<div class="deal-custom-fields deal-custom-fields-2">`, with the number of the position at the end.

To show one field somewhere else, set it to `Do not automatically display` and read it in a deal layout override:

```php
<?php
foreach ($displayData['item']->jcfields ?? [] as $field) {
    if ($field->name === 'dress-code' && $field->value !== '') {
        echo '<p class="dress-code">' . $field->label . ': ' . $field->value . '</p>';
    }
}
```

`$field->value` is the value already rendered as HTML. `$field->rawvalue` is the value as it is saved.
