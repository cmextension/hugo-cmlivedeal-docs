---
title: CMLD Merchant plugin
weight: 160
---
The `User - CM Live Deal's Merchant Profile` plugin adds merchant profile fields to Joomla user accounts. Merchants fill in these fields when they register.

The plugin is enabled when you install CM Live Deal. If you turn it off, later updates leave it off.

Make sure Joomla allows user registration. Go to `Users` -> `Manage`, then click the `Options` button on the toolbar.

![/images/plg_user_option.png](/images/plg_user_option.png)

Set `Allow User Registration` to `Yes`.

![/images/plg_user_option_allow_user_registration.png](/images/plg_user_option_allow_user_registration.png)

Click `Save`. You see the message `Configuration saved`.

![/images/plg_user_option_allow_user_registration_saved.png](/images/plg_user_option_allow_user_registration_saved.png)

To configure the plugin, go to `System` -> `Manage` -> `Plugins`.

![/images/plg_user_cmldmerchant_menu.png](/images/plg_user_cmldmerchant_menu.png)

Search for `CM Live Deal`. Click `User - CM Live Deal's Merchant Profile` to edit its settings.

![/images/merchant-plugin-4-0-options.png](/images/merchant-plugin-4-0-options.png)

*   **Registration page**: The menu item of the merchant registration page that you created before. The merchant profile fields are only shown on this page.

Some profile fields have a setting with 3 values:

*   **Required**: The field is shown and the merchant must fill it in.
*   **Optional**: The field is shown and the merchant can leave it empty.
*   **Disabled**: The field is not shown.

| Field | Default |
| --- | --- |
| Website | Optional |
| Facebook | Optional |
| X | Optional |
| Pinterest | Optional |
| Instagram | Optional |
| About | Required |

`Business name`, `Address` and `Phone` have no setting. They are always required. The profile form also has a map where the merchant places their location.

After saving the plugin, you see the message `Plugin saved`.

![/images/plg_user_cmldmerchant_saved.png](/images/plg_user_cmldmerchant_saved.png)

Visit the merchant registration page on your site to check that the merchant profile fields are shown there. The page could look like the following screenshot.

![/images/plg_user_cmldmerchant_frontend.png](/images/plg_user_cmldmerchant_frontend.png)
