# LinkJolt for Claude

The official LinkJolt plugin. It connects Claude to your LinkJolt affiliate program and teaches Claude the jobs merchants ask for most.

## What you get

- **The LinkJolt connector** at `https://www.linkjolt.io/api/mcp`. You sign in with your LinkJolt account the first time Claude uses it, and Claude only sees the brand you pick.
- **Six skills**
  - **Add an affiliate program**: adds LinkJolt to your app's code. It works out how the app takes payment (Stripe, Paddle, Polar, Creem, Lemon Squeezy, Gumroad), adds the tracking script, passes the affiliate referral into checkout and tells you how to check it with a test sale.
  - **Affiliate program review**: sales, commissions, clicks and top affiliates for any period, compared with the period before.
  - **Pending commissions**: goes through sales waiting for approval, flags anything unusual, and approves or rejects only what you confirm.
  - **Who to pay**: who you owe, how much in each currency, what is already on its way, and how to pay each affiliate.
  - **Sale not tracking**: finds out why a sale is missing or wrong and what to do about it.
  - **Affiliate coupon codes**: checks and adds affiliate discount codes.

Claude can read your program and, on the Ultimate and Scale plans, record a sale, approve or reject a sale, and add a coupon code, each only when you ask. It cannot send money: payouts happen on your Payouts page.

## Plans

Reading data needs the LinkJolt Professional plan or higher. Recording sales, approving or rejecting sales and adding coupon codes need Ultimate or Scale. Team members see what their role allows.

## Install

In Claude Code:

```
/plugin marketplace add Efesop/linkjolt-claude-plugin
/plugin install linkjolt@linkjolt
```

On claude.ai and Claude Desktop, add LinkJolt from the directory, or add the connector address above under Settings, Connectors.

## What it connects to

The plugin has no code of its own. It adds one remote connector, `https://www.linkjolt.io/api/mcp`, run by LinkJolt, and the six skills above, which are written instructions for Claude. It runs nothing on your computer and sends nothing anywhere else. Every connector call is made with your own LinkJolt sign-in and answered from your own LinkJolt account, for the brand you picked when you signed in. LinkJolt keeps a log of each change the connector makes to your account, as it does for the Developer API.

## Support and privacy

- Support: [ollie@linkjolt.io](mailto:ollie@linkjolt.io)
- Privacy policy: [https://www.linkjolt.io/privacy](https://www.linkjolt.io/privacy)
- Developer docs: [https://www.linkjolt.io/developers/mcp](https://www.linkjolt.io/developers/mcp)
