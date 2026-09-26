---
name: who-to-pay
description: Work out which LinkJolt affiliates are owed money, how much in each currency, what is already on its way, and how to pay them. Use when the user asks who they owe, what payouts are due, how much to pay affiliates, or how to pay their affiliates.
---

# Who to pay

LinkJolt never holds money. The merchant pays affiliates from their own account, either with Stripe payouts or by sending the money themselves (PayPal or Wise). This connector can show who is owed, but it cannot send money. Paying happens on the Payouts page.

## Steps

1. Call `list_conversions` with `status` "approved" and page through with `limit` 100 until you have them all. Approved and not yet paid is what is owed.
2. Call `list_payouts` to see automatic payouts already queued. Sales with a queued payout that is `pending`, `scheduled`, `processing` or `debit_clearing` are already on their way, so do not count them as owed again. A payout that is `awaiting_funds` is stuck until the merchant adds a card or turns on payout charges in Payouts settings; say so.
3. Group what is owed by affiliate and by currency. Use `fetch` with `affiliate:<id>` for each affiliate's name and how they asked to be paid (`payoutMethod`, and `payoutEmail` for PayPal or Wise).
4. Report one line per affiliate: name, amount owed per currency, number of sales, and how they want to be paid. Then the total per currency.

## How the merchant pays

Send the merchant to https://www.linkjolt.io/merchant/payouts and explain the way that fits each affiliate:

- **Stripe payouts** for affiliates who connected Stripe: LinkJolt charges the merchant's card and sends the money to the affiliate's Stripe account. Stripe's processing fee (2.9% + 30 cents on cards) is passed on at cost, and LinkJolt keeps nothing. This works for affiliates in the US, UK, EEA, Canada and Switzerland, paid in USD, GBP or EUR.
- **PayPal or Wise**: the merchant downloads the payout file or copies each email and amount, pays from their own PayPal or Wise account, then marks those commissions as paid in LinkJolt. There is no LinkJolt fee.

## Rules

- Never add amounts in different currencies together, and never convert an amount the merchant has to pay.
- Never say a payout was sent unless the tools show it (`paid` status or a completed payout).
- Reading payouts needs the Professional plan or higher. On a permission error, the user's team role may not include payouts.
