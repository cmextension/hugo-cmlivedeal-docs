---
title: Falang
weight: 40
---
The language files translate the words of CM Live Deal itself, such as buttons and labels. To translate your own content, such as the titles and descriptions of your deals, you can use [Falang](https://www.faboba.com/en/composants/falang.html). Falang 6 works with Joomla! 5 and Joomla! 6. Its free version is enough for the steps below.

## What you can translate

| Content | Fields |
|---|---|
| Deals | Title, alias, description, fine print, meta keywords, meta description |
| Cities | Name, alias, meta keywords, meta description |
| Merchants | Name, about, address, meta keywords, meta description |
| Deal categories | Everything Falang translates for Joomla! categories |

Other data, such as email templates, coupon templates and plans, cannot be translated with Falang.

## Set up

1.  Set up your site for more than one language, as Falang's documentation describes. You need the **System - Language Filter** plugin enabled and a published content language for each language.
2.  Install Falang.
3.  In CM Live Deal, go to **Options**, open the **General** tab, set **Falang integration** to **Enabled** and save.
4.  Open CM Live Deal's **Dashboard** once.

CM Live Deal comes with the files Falang needs to translate deals, cities and merchants. Falang calls them *content elements*. CM Live Deal copies them into Falang when you install or update CM Live Deal. If you install Falang later, they are copied the next time you open CM Live Deal's Dashboard. You then see **CM Live Deal deals**, **CM Live Deal cities** and **CM Live Deal merchants** in Falang's list of content elements.

## Translate

In Falang, go to **Translation**, pick the language and pick the content element, for example **CM Live Deal deals**. Click a deal, fill in the translated fields and save. Falang shows the translation to visitors who use that language.

## Translated aliases

You can translate the alias of a deal, a city or a category. Then the links to them use the translated alias. For example, the German page of the city *New York* can be `/de/neu-york` instead of `/de/new-york`.

CM Live Deal needs the **Falang integration** option to understand these links. When the option is disabled, a link with a translated alias of a city or a category shows a "page not found" error, and a link with a translated alias of a deal does not open the deal.

Links with the original alias keep working in every language.
