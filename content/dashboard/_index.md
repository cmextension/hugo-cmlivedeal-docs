---
title: Dashboard
weight: 25
---
The dashboard shows how your deals and coupons are doing. To open it, go to `Components` -> `CM Live Deal` -> `Dashboard`.

If your site still needs some settings, the Setup card is at the top of the dashboard. See [The Setup card](/dashboard/setup-card/).

The page opens first and the numbers load a moment later. Each part loads on its own, so a slow part does not hold up the rest.

## The numbers at the top

![/images/dashboard-4-0-overview.png](/images/dashboard-4-0-overview.png)

* **Total deals**: every deal on the site, in any state.
* **Live deals**: deals that visitors can see right now. A deal is live when it is published, approved, and the current time is between its `Starting time` and `Ending time`. A deal that repeats weekly is only live during one of its time slots. The front-end deal list uses the same rule.
* **Total merchants**: users in the merchant user group. You choose this group under `Options` -> `Merchant`.
* **Captured coupons**: every coupon customers have taken.
* **Redeemed coupons**: coupons that merchants have redeemed.
* **Redemption rate**: redeemed coupons as a percentage of captured coupons.

## Deals and coupons over time

This chart shows each month:

* **New deals** (bars, counted on the right axis): deals created that month.
* **Captured coupons** (line): coupons taken that month.
* **Redeemed coupons** (line): coupons redeemed that month.

## Orders and advance payments over time

This chart only shows when `Advance payment` is on (`Options` -> `Advance payment`). It shows each month:

* **Paid orders** (bars): how many orders were paid.
* **Advance payments** (line, counted on the right axis): how much money those orders brought in.

An order counts in the month it was paid, not the month it was created. Unpaid and refunded orders are not counted.

## How far back the charts go

The two charts above have a list in their top right corner. Choose `6 months`, `12 months`, `24 months` or `All time`. The default is `12 months`.

* Both charts use the same choice. When you change one, the other changes too.
* When the charts cover more than 24 months, they show one point per year instead of one per month.
* CM Live Deal remembers your choice until you log out.

The other charts always cover everything your site has recorded.

## Deals, coupons, merchants and cities

![/images/dashboard-4-0-charts.png](/images/dashboard-4-0-charts.png)

* **Deals by state**: how many deals are `Online`, `Scheduled` (they start later), `Expired`, `Unpublished` or `Waiting for approval`. Every deal is in exactly one of these. This chart only looks at the starting and ending time, so a weekly deal between two time slots still counts as `Online` here.
* **Coupon redemption**: captured coupons split into `Redeemed coupons` and `Not redeemed yet`.
* **Redeemed coupons by merchant**: the five merchants whose deals had the most coupons redeemed.
* **Deals by city**: the five published cities with the most deals. A deal counts for a city when its merchant's location is inside the city's radius, the same way the front-end city filter works. So one deal can count for more than one city.

## Top deals

![/images/dashboard-4-0-top-deals.png](/images/dashboard-4-0-top-deals.png)

This card lists the ten leading deals, with one tab for each measure:

* **Clicks**: how many times visitors opened the deal from a deal list, in the popup or with the `View deal` button.
* **Captured**: how many coupons customers took.
* **Redeemed**: how many coupons merchants redeemed.
* **Purchases** and **Advance payments**: how many paid orders the deal had and how much money they brought in. These two tabs only show when `Advance payment` is on.

If you can edit deals, click a deal title to open the deal.

## When there is nothing to show

On a new site, a chart with no data shows the message `No data to show yet`.

If a part of the dashboard cannot load, it shows `This part of the dashboard could not be loaded. Reload the page to try again.` The rest of the dashboard still works.

## Coming from CM Live Deal 3.x

The `Statistics`, `Top merchants` and `Top users` tables are gone. The numbers at the top, the charts and the Top deals card replace them.
