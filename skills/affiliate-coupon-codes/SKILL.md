---
name: affiliate-coupon-codes
description: Give LinkJolt affiliates their own discount codes so sales using a code are credited to them. Use when the user wants to add, check or list coupon codes for affiliates, or asks why a code is not crediting an affiliate.
---

# Affiliate coupon codes

A coupon code lets an affiliate earn from sales where the customer typed a code instead of clicking a link. The discount itself lives in the merchant's payment platform. LinkJolt holds a reference to it so it knows which affiliate to credit.

## Checking codes

- `list_coupons` lists registered codes. Filter with `campaignId`, `affiliateId`, `provider` or look up one `code`.
- Use `list_campaigns` and `list_affiliates` to find the right ids from names.

## Adding a code

1. Make sure the discount already exists in the merchant's Stripe, Paddle, Polar, Creem or App Store Connect account. `create_coupon` never creates the discount. The LinkJolt dashboard can create it for merchants who connected Stripe, Polar or Creem (Campaigns, Coupons tab).
2. Confirm with the user the campaign, the affiliate, the code and where the discount lives.
3. Call `create_coupon` with `campaignId`, `affiliateId`, `code` and `provider`. Add the platform's id when the user has it: `stripePromotionCodeId` (promo_...), `paddleDiscountId` (dsc_...), `polarDiscountId` or `creemDiscountId` (dis_...). For App Store offer codes use `provider` "apple" with `appleOfferType`.
4. `percentOff`, `amountOff` and `currency` are only shown to affiliates; they do not change the discount.

## Rules

- Only add codes the user asked for, and only after the user confirms the details.
- The affiliate must be approved with the merchant's brand, or the code is refused.
- Adding codes needs an Ultimate or Scale plan. Reading them needs Professional or higher.
