---
title: Translate
weight: 30
---
Open `xx-YY.com\_cmlivedeal.ini` with a text editor and translate the English strings in this file to your new language.

_Important note_: Joomla! language INI files must be saved as UTF-8 without the Byte Order Mark (BOM). For more information on Byte Order Mark see [http://unicode.org/faq/utf\_bom.html#BOM](http://unicode.org/faq/utf_bom.html#BOM)

The above instruction is for translating the front-end of CM Live Deal component. To translate the back-end and the other extensions of the package, you need to repeat the above steps for the following folders. Each folder holds the `.ini` file with the strings the extension shows, and a `.sys.ini` file with the name and the description Joomla! shows in its own lists.

Component:

*   **CM Live Deal component's back-end**: <Joomla! root folder>/administrator/components/com\_cmlivedeal/language/

Modules:

*   **Search - CM Live Deal**: <Joomla! root folder>/modules/mod\_cmlivedeal\_search/language/
*   **Categories - CM Live Deal**: <Joomla! root folder>/modules/mod\_cmlivedeal\_categories/language/
*   **Cities - CM Live Deal**: <Joomla! root folder>/modules/mod\_cmlivedeal\_cities/language/
*   **Merchants - CM Live Deal**: <Joomla! root folder>/modules/mod\_cmlivedeal\_merchants/language/

Plugins:

*   **User - CM Live Deal's Merchant Profile**: <Joomla! root folder>/plugins/user/cmldmerchant/language/
*   **Button - CM Live Deal Image**: <Joomla! root folder>/plugins/editors-xtd/cmldimage/language/
*   **Smart Search - CM Live Deal**: <Joomla! root folder>/plugins/finder/cmlivedeal/language/
*   **Privacy - CM Live Deal**: <Joomla! root folder>/plugins/privacy/cmlivedeal/language/
*   **Task - CM Live Deal**: <Joomla! root folder>/plugins/task/cmlivedeal/language/
*   **Web Services - CM Live Deal**: <Joomla! root folder>/plugins/webservices/cmlivedeal/language/
*   **PayPal - CM Live Deal**: <Joomla! root folder>/plugins/cmlivedeal/paypal/language/
*   **Stripe - CM Live Deal**: <Joomla! root folder>/plugins/cmlivedeal/stripe/language/
*   **Membership Pro - CM Live Deal Integration**: <Joomla! root folder>/plugins/osmembership/cmlivedeal/language/

The file name inside each folder follows the same pattern as the component's: the language tag, a dot, the extension's element name, eg `fr-FR.mod\_cmlivedeal\_search.ini`.

Your translation lives inside the extension's own folders, so a later update of CM Live Deal overwrites it. Keep a copy of your files outside the site, and put them back after every update. If you would rather not lose them, use Joomla!'s **Language Overrides** (System -> Language Overrides) for the strings you want to change: overrides are stored separately and survive updates.
