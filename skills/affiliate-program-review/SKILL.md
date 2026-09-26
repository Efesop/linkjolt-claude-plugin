---
name: affiliate-program-review
description: Review how a LinkJolt affiliate program is doing, covering sales, commissions, clicks, top affiliates and anything that needs attention. Use when the user asks how their affiliate program, partners or campaigns are performing, wants a weekly or monthly recap, or asks which affiliates are doing best.
---

# Affiliate program review

Give the merchant a short, honest picture of their LinkJolt affiliate program for a window of time. Default to the last 7 days when they do not name one, and compare it with the 7 days before.

## Steps

1. Call `get_stats` for the window, then again for the earlier window of the same length. Read money from `byCurrency`.
2. Call `list_conversions` with `from` and `to` for the window (use `limit` 100 and page through if needed). Note refunds, rejected sales, one unusually large order, or a sudden spike from one affiliate.
3. Call `list_affiliates` and match `affiliateId` on the conversions to names, so you can say who drove the results.
4. If the user wants a visual summary of this month, `get_program_overview` shows an interactive card.

## How to report

- Lead with the headline: sales, revenue and commission for the window, then how that compares with the earlier window.
- Name the top 3 affiliates with what they earned, then anything trending up or down, then one suggested action.
- Keep it short and in plain language. No tables unless the user asks.

## Rules

- Amounts are in each record's own currency. Never add amounts in different currencies together; list them side by side (for example "$1,200 and 340 EUR").
- Never invent a percentage. If the earlier window had no sales, say what the current window holds instead of a growth figure. If the current window is empty, say so rather than "-100%".
- Revenue counts approved and paid sales. Pending sales are still waiting for the merchant's approval, so say how many there are separately.
- Only use numbers the tools returned.
- If a tool answers with a plan or permission error, tell the user their LinkJolt plan or team role does not include it. Reading data needs the Professional plan or higher.
