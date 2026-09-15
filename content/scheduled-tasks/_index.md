---
title: Scheduled tasks
weight: 229
---

Some jobs must happen when time passes, not when somebody clicks something. CM Live Deal runs them with Joomla's **Scheduled Tasks**:

* **Expire deals - CM Live Deal**: unpublishes deals that have ended.
* **Expiry reminders - CM Live Deal**: emails merchants and customers a few days before a deal ends.

The package enables the plugin **Task - CM Live Deal** when it installs it. But **nothing runs until you create a task**. Create one task for each job you want.

## Create a task

1. Go to `System > Manage > Scheduled Tasks` and click `New`.
2. Type `CM Live Deal` in the search box and click the task you want.

   ![/images/scheduled-tasks-4-0-select.png](/images/scheduled-tasks-4-0-select.png)

3. Enter a **Title**.
4. Choose an **Execution Rule**. For both tasks, `Interval, Days` with `1` is a good choice: the task runs once a day.
5. Set the **Task Parameters**. They are explained below.
6. Click `Save & Close`.

To try a task now, click `Run Task` next to it in the list.

## How tasks run

A task runs only when Joomla's scheduler is started. There are three ways. Pick one:

* **Lazy Scheduler** (on by default): Joomla checks for due tasks when somebody visits the site. It needs no setup, but on a site with few visitors a task can run late.
* **Web Cron**: turn it on in `System > Manage > Scheduled Tasks > Options > Web Cron`, then let a cron service call the link shown there.
* **Cron job on your server**: the most reliable way. Run this every few minutes:

```
php /path/to/your/site/cli/joomla.php scheduler:run --all
```

The Lazy Scheduler and Web Cron need the plugin **System - Schedule Runner** to be enabled.

You can also run one task from the command line. Use the ID from the task list:

```
php cli/joomla.php scheduler:run --id=2
```

To see what a task did, open it and click the **Execution History** tab. Each run also writes a line to `administrator/logs/joomla_scheduler.php`, for example:

```
INFO	Task> Unpublished 6 deals that have ended.
```

## Expire deals

![/images/scheduled-tasks-4-0-expire.png](/images/scheduled-tasks-4-0-expire.png)

This task unpublishes every published deal whose ending time has passed.

Visitors already stop seeing a deal when it ends, with or without this task. The task keeps the rest of the site in step: the deal list in the administration area, the merchant's own deal list and [Smart Search](/smart-search/) all show the deal as unpublished.

* **Grace Period (hours)**: how long an ended deal stays published before it is unpublished. With `0`, it is unpublished on the first run after it ends.

The task never deletes anything. The deal, its coupons and its orders stay. To bring a deal back, move its ending time to a later date and publish it again.

Developers: the task fires `onCMLDAfterChangeDealState` with the action `expire`. See [Events](/developers/events/).

## Expiry reminders

![/images/scheduled-tasks-4-0-reminder.png](/images/scheduled-tasks-4-0-reminder.png)

This task emails people before a deal ends.

* **Remind Before (days)**: how many days before a deal ends to send the reminder. The default is `3`.
* **Send To**: who gets reminders.
  * **Merchant**: the merchant of each published, approved deal that ends within that many days. One email per deal.
  * **Customer**: each registered user who holds an unredeemed coupon for such a deal. One email per coupon.

Each reminder is sent **only once**. The task remembers it, so running the task again, or moving the deal's ending time, does not send it again.

A coupon captured by a guest gets no reminder, because there is no account to email. A redeemed coupon gets no reminder either.

Run the task at least once a day. If it runs less often than **Remind Before (days)**, a deal can end before the task sees it.

The text of the emails comes from [Email templates](/email-templates/): **Your deal {deal} ends soon** for merchants and **Your coupon for {deal} expires soon** for customers. The emails are sent from the address in `System > Global Configuration > Server > Mail`.

With the default templates, a merchant gets this email:

```
Subject: Your deal Sample Deal 09 ends soon

Dear Merchant 03,

Your deal Sample Deal 09 on CM Live Deal ends on Thursday, 17 September 2026 12:48.

If you would like to keep it running, open the deal and move its ending time.

Best regards,
CM Live Deal
```

And the customer who holds a coupon for that deal gets this one:

```
Subject: Your coupon for Sample Deal 09 expires soon

Dear Customer 02,

Your coupon JBBEX for Sample Deal 09 from Merchant 03 expires on Thursday, 17 September 2026 12:48.

Use it before then, or it is gone.

Best regards,
CM Live Deal
```

If one of the two templates is missing, the task stops and the log says `The email template "..." is missing from CM Live Deal`.
