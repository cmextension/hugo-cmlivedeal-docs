---
title: Merchant profile
weight: 70
---
When merchant subscribes for his/her membership, the membership component asks merchant for basic information in the subscription form, you can get this information and put into merchant's profile.

If merchant renews his/her membership, the data in CMLiveDeal profile will not be modified unless it is empty, for example if merchant deletes the phone number in CMLiveDeal profile, he/she provides the phone number when renews the membership, then this new phone number will be automatically updated in CMLiveDeal profile and it will be displayed in the deals of this merchant.

To setup profile in Membership Pro component, in Custom Fields section, you edit/create your fields. The first column `Name` contains the required information you need to put into CMLiveDeal's configuration.

![/images/com_osmembership_field_list.webp](/images/com_osmembership_field_list.webp)

Go to `Merchant` tab in CMLiveDeal's configuration, set `Get merchant profile from another source` to `Get from the integrated membership component`, enter the field name you have in the column `Name` in Membership Pro's Custom Fields section into the fields at the bottom of the page.

![/images/com_cmlivedeal_membership_profile.webp](/images/com_cmlivedeal_membership_profile.webp)