---
name: sale-not-tracking
description: Find out why an affiliate sale is missing or wrong in LinkJolt and what to do about it. Use when the user or one of their affiliates says a sale did not track, a commission is missing, a coupon did not credit the affiliate, or a sale shows the wrong status.
---

# A sale did not track

Work from facts. Ask for what you need first: the affiliate, roughly when the purchase happened, the order id or customer email if known, the amount, whether the customer used a link or a coupon code, and which payment platform the merchant uses (Stripe, Paddle, Polar, Creem, Lemon Squeezy, Gumroad, GoPay, WooCommerce or the App Store).

## 1. Look for the sale

- Call `search` with the order id, customer name or customer email, then `fetch` any matching `conversion:<id>`.
- Otherwise call `list_conversions` with `affiliateId` and a `from`/`to` range around the purchase date, then the same range without `affiliateId` in case it was credited to someone else.

If it is there, explain its status:
- `pending`: tracked and waiting for the merchant's approval.
- `trial_period`: a free trial; the commission comes with the first real payment.
- `approved` or `paid`: nothing is wrong.
- `refunded`: the customer was refunded, so the commission was reversed.
- `rejected`: the merchant turned it down.

## 2. If it is not there

- Call `list_affiliates` with the campaign's `campaignId` and check the affiliate is on it. A rejected affiliate earns nothing.
- For a coupon sale, call `list_coupons` with `code`. The code has to be registered in LinkJolt for that affiliate and campaign, with the right `provider`, and the same discount has to exist in the merchant's payment platform. If it is missing, the `affiliate-coupon-codes` skill covers adding it.
- For a link sale, the usual causes are:
  - The LinkJolt tracking script is not on the site where the customer landed or started checkout, so the click was never saved.
  - The customer clicked on one device or browser and bought on another, or bought more than 30 days after the click.
  - The payment platform is not connected, or its connection needs attention. The Integrations page shows the state of each one: https://www.linkjolt.io/merchant/integration
  - It was a test purchase. Test sales are kept apart under Commissions, Test sales.
  - The customer's email matches the affiliate's own email, which LinkJolt treats as a self-referral.
  - The plan's monthly tracked-sales limit was reached. The sale is kept but held until the plan is upgraded; the dashboard shows it.

## 3. Fixing it

If the merchant confirms the sale is real and should count, `create_conversion` records it by hand. Only call it after the user confirms, and always pass the real `orderId` so it cannot be recorded twice. The affiliate is notified as for any other sale. It needs an Ultimate or Scale plan; otherwise the merchant can add the sale from the Commissions page.

## Rules

- Say what you checked and what you found. Where you are inferring a cause, say it is likely rather than certain.
- Never tell an affiliate a sale will be paid. That is the merchant's decision.
