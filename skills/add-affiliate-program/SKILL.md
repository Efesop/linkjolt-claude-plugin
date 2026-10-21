---
name: add-affiliate-program
description: Add a LinkJolt affiliate program to a web app or SaaS codebase. Works out how the app takes payment (Stripe Checkout, Stripe Payment Links, Paddle, Polar, Creem, Lemon Squeezy, Gumroad), adds the LinkJolt tracking script, passes the affiliate referral into checkout and tells the user how to check it with a test sale. Use when the user asks to add an affiliate program, referral program, partner program or creator links with commissions to their app, or to install LinkJolt tracking in their code.
---

# Add a LinkJolt affiliate program to an app

You are adding LinkJolt to the codebase you are working in. LinkJolt records each sale from the merchant's own payment provider, works out the commission, reverses it on a refund, gives affiliates their sign-up page and portal, and pays them through Stripe payouts or a PayPal or Wise file. The code side is one script tag and, for checkouts the server creates, one field. There is no npm package to install and no referral table, commission maths, webhook handler or payout code to write.

The full, current steps live at https://www.linkjolt.io/developers/quickstart.md, with one file per payment platform linked from it. Fetch it when you can; what follows is the short version.

## 1. Find how the app takes payment

Look at package.json, the pricing page and the checkout code:

- The stripe package and stripe.checkout.sessions.create: Stripe Checkout (platform `stripe`).
- buy.stripe.com links, a stripe-pricing-table or a stripe-buy-button: Stripe Payment Links (platform `payment-links`).
- @paddle/paddle-js and Checkout.open: Paddle Billing (platform `paddle`).
- @polar-sh/sdk or buy.polar.sh links: Polar (platform `polar`).
- The creem package or creem.io links: Creem (platform `creem`).
- lemonsqueezy.com or gumroad.com links: platform `payment-links`.
- WooCommerce, Webflow, Squarespace, Shopify app billing, Apple in-app purchases and the rest: the quickstart links the file for each.

More than one? Handle each. Nothing found? Ask the user how they take payment.

## 2. Get the LinkJolt side ready

See whether the LinkJolt connector is available (tools such as `get_setup_status`).

- **Connected.** Call `get_setup_status` first: it gives the business details, the next launch step, the tracking script tag with this account's merchant id, and each campaign's join link. If business.needsDetails is true, ask for the real business name and save it with `update_business` only after the user confirms. If there is no campaign yet, suggest a commission (`plan_affiliate_program` gives one from published benchmarks) and create it with `create_campaign` only after the user confirms. If LinkJolt answers that the account needs a plan first, pass on its answer and its link as they are. Then call `get_tracking_instructions` with the platform from step 1 for this account's exact steps.
- **Not connected.** The user needs a LinkJolt account (https://www.linkjolt.io/auth/sign-up). Offer to add the connector so you can do the LinkJolt side: a remote MCP server at https://www.linkjolt.io/api/mcp, signed in through the browser, with no key to paste (the quickstart has the command for each agent). Without the connector, ask the user for their merchant id: it is in the tracking script tag on the LinkJolt Integrations page.

Never ask for passwords or API keys in the chat.

## 3. Tell the user the one browser step

Sales reach LinkJolt once the user connects their payment provider in LinkJolt, on the Integrations page. Only they can do it:

- Stripe: Set up with Stripe, then approve once on Stripe. No webhook to create.
- Paddle: paste a Paddle API key that can write notification settings; LinkJolt creates the destination.
- Polar: approve once. Creem: paste a Creem API key. LinkJolt creates the webhook for both.
- Lemon Squeezy and Gumroad: add the webhook shown on that Integrations tab.

Say it once, then carry on with the code.

## 4. Add the tracking script

Load it on every page, from the root layout:

- Next.js App Router: next/script in app/layout.tsx with strategy afterInteractive.
- Anything else: a script tag in the head.
- Source: https://www.linkjolt.io/api/tracking.js?id=MERCHANT_ID with the account's merchant id.
- TypeScript: declare window.linkjolt with referral as string or null in a global .d.ts file.

## 5. Pass the referral to checkout

The script exposes the affiliate as window.linkjolt.referral (empty when the visitor did not come from an affiliate link). Make the one change for the platform, and never rename the field or put the referral anywhere else:

- **Stripe Checkout:** read the referral in the browser, send it with the checkout request, and set client_reference_id to the referral (or leave it unset when empty) in stripe.checkout.sessions.create. If client_reference_id already carries something else in this app, stop and ask the user: LinkJolt needs that field, or affiliates can use discount codes instead.
- **Stripe Payment Links, pricing table, buy button, Lemon Squeezy and Gumroad links:** nothing to change. The script adds the referral to those links on pages that load it, so keep them as ordinary links or embeds.
- **Paddle:** pass customData with linkjolt set to the referral when opening Checkout (customData, not passthrough).
- **Polar:** checkout links need nothing. A checkout the server creates sets metadata.reference_id to the referral (metadata.linkjolt if reference_id is already used).
- **Creem:** payment links need nothing. A checkout the server creates sets metadata.linkjolt_ref to the referral.

Renewals, refunds, trials and plan changes are tracked from the first payment, so there is nothing more to build.

## 6. Check it

1. Run the project's type check or build.
2. Tell the user how to test: open the site with ?tc=TEST&aff=TEST and run window.linkjolt.referral in the browser console (it should print tc_TEST_aff_TEST), then use the test link in the Check it works step on the Integrations page and buy in the provider's test mode. Test sales appear under Commissions, Test sales. With Set up with Stripe, a Stripe test-mode purchase counts; a Stripe sandbox is a separate account and does not.
3. With the connector, `get_setup_status` shows whether LinkJolt has seen the tracking script.

## 7. Report

List the files you changed, what the user still does in the browser (connect the payment provider, finish setting up the account) and how to run the test.
