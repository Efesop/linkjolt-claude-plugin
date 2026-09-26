---
name: pending-commissions
description: Go through LinkJolt affiliate sales waiting for approval and recommend which to approve, check or reject, then approve or reject only the ones the user confirms. Use when the user asks about pending commissions, sales to review, what to approve, or suspicious affiliate sales.
---

# Review pending commissions

In LinkJolt a tracked sale starts as `pending` and earns the affiliate nothing until the merchant approves it. Help the merchant decide quickly and safely.

## Steps

1. Call `list_conversions` with `status` "pending" (add `campaignId` if the user named a campaign). Page through with `limit` 100 until you have them all.
2. For each sale note its age, amount, commission, currency, affiliate and order id.
3. Look closer where something is off. Use `fetch` with `affiliate:<id>` to see the affiliate's email and `conversion:<id>` for the full sale.
4. Sort the sales into three groups:
   - **Looks fine to approve.**
   - **Worth a closer look**, and say why: much larger than the program's usual order, several orders from one customer in a short time, a brand new affiliate with a burst of sales.
   - **Likely duplicate or self-referral**: the same order id twice, or a customer email that matches the affiliate's own email.
5. Give a recommendation for each group and stop. Ask the user which ones to approve or reject.

## Approving or rejecting

- Call `update_conversion` only after the user confirms, in this conversation, each sale by name or order id. Never approve or reject on your own judgement, and never in bulk without a clear yes.
- Approving can start an automatic payout if the merchant has automatic payouts switched on in LinkJolt. Mention that before approving a large batch.
- Treat rejecting as final: the affiliate is never paid for that sale.
- `trial_period` sales are free trials. The commission is decided when the first real payment arrives, so leave them alone.
- Approving and rejecting need an Ultimate or Scale plan, and a team member also needs permission to manage payouts. On a plan or permission error, send the user to https://www.linkjolt.io/merchant/commissions to do it there.

## Rules

- Never add amounts in different currencies together.
- Only use facts the tools returned. Say "looks unusual", never "fraud", unless the evidence is plain (such as the affiliate buying through their own link).
