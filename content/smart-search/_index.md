---
title: Smart Search
weight: 225
---

CM Live Deal adds deals to Joomla's **Smart Search**. When a visitor searches your site, the results show deals next to articles and other content.

![/images/smart-search-4-0-results.png](/images/smart-search-4-0-results.png)

## Turn it on

The package enables the plugin **Smart Search - CM Live Deal** when it installs it. Two more things are needed. Both are part of Joomla:

1. Go to `System > Plugins` and check that **Content - Smart Search** is enabled. Without it, Smart Search does not hear about deals that are saved, published or deleted.
2. Go to `Components > Smart Search` and click **Index** to build the index. You only do this once. After that, the index is updated when a deal is saved, published, unpublished, approved, disapproved or deleted.

You can also build the index from the command line:

```
php cli/joomla.php finder:index
```

Visitors search with the Smart Search module or a `Smart Search > Search` menu item.

## Which deals are found

A visitor finds a deal only when all of these are true:

* The deal is published.
* The deal is approved.
* The time is between the deal's starting time and ending time.
* The deal's category is published.
* The visitor can see the deal's `Access` level and its category's access level.

Nothing needs to run when a deal expires. It drops out of the results on its own after its ending time. If you change a deal's starting or ending time, save the deal and the index is updated.

Smart Search uses only the starting and ending time. A deal with a weekly schedule is still found between its time slots. See [Create new deal in back-end](/create-new-deal-in-back-end/).

## What is searched

Smart Search looks for the words in:

* The deal title.
* The description and the fine print.
* The meta keywords and meta description.
* The merchant's name and address.
* [Custom fields](/custom-fields/) whose `Search Index` option is `Make searchable` or `Make searchable and add as taxonomy`.

The result shows the deal title, the description, and the date the deal was created. If `Result Image` is on in the Smart Search options, it shows the deal image too.

## Where a result goes

The link of a result opens the deal:

* When the deal detail is shown in a popup, the link opens the deal list with the deal's popup.
* When the deal detail is shown on its own page, the link opens that page. It uses a `Deal Detail` menu item if you have one.

The deal detail option is in [the Deal options](/configuration/deal/).

The link needs a CM Live Deal menu item, such as `Deals`. Without one, it uses your home page. If you add or change a CM Live Deal menu item, or change the deal detail option, build the index again.

## Search filters

Each deal is filed in 3 branches. Visitors can use them to narrow the results, and you can use them in a Smart Search filter:

* **Type**: `Deal`.
* **Category**: the deal's category.
* **Merchant**: the merchant's name.

To choose the branches, edit the plugin **Smart Search - CM Live Deal** and change **Search Filters**. Build the index again after you change them.

![/images/smart-search-4-0-plugin.png](/images/smart-search-4-0-plugin.png)

For example, to make a search page that only finds deals, go to `Components > Smart Search > Search Filters`, create a filter, and tick `Deal` under **Search by Type**. Then choose the filter in your Smart Search menu item.

![/images/smart-search-4-0-filter.png](/images/smart-search-4-0-filter.png)

A custom field with `Add as taxonomy` adds its own branch.

There is no City branch. A deal belongs to a merchant, and a city is found by distance from the merchant's address, so it cannot be stored in the index.
