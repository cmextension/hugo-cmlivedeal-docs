---
title: Permissions
weight: 130
---
![/images/com_cmlivedeal_permissions.png](/images/com_cmlivedeal_permissions.png)

On the left side there are tabs for user groups. On the right side, we have the options of the permissions that the users in the selected group can do.

You select your merchant group and configure the following permissions:

*   **Configure ACL & Options**: Not Allowed
*   **Access Administration Interface**: Not Allowed
*   **Create**: Allowed
*   **Delete**: Allowed
*   **Edit**: Not Allowed
*   **Edit State**: Allowed
*   **Edit Own**: Allowed
*   **Edit Custom Field Value**: Allowed. Merchants need this to fill in the custom fields you add to deals.

If merchants use the API, they also need two settings outside CM Live Deal: `Web Services Login` in Global Configuration, and the merchant group in the User - Joomla API Token plugin. See [Permission to sign in to the API](/dashboard/setup-card/#permission-to-sign-in-to-the-api).

The [Setup card](/dashboard/setup-card/) on the dashboard tells you when the merchant group is missing one of these permissions.

