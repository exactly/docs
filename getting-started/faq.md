# ❔ FAQ

## General

### What is Exactly?

Exactly is a new decentralized, non-custodial and open-source protocol that provides an autonomous interest rate market to lenders and borrowers while setting interest rates based on credit supply and demand, enabling users to frictionlessly exchange the time value of their crypto assets at both variables and fixed interest rates for the first time in DeFi.

Aside from taking loans and making deposits at variable interest rates from a Variable Rate Pool, this protocol enables users to do so at fixed rates through interaction with several Fixed Rate Pools, each representing a specific maturity date. Interest rates are determined based on the credit utilization rate of each Fixed Rate Pool.

### Why does Exactly matter?

With an innovative approach, the protocol allows users to lend and borrow assets at fixed and variable rates in a more efficient way through the implementation of the [ERC-4626](../guides/protocol/#erc-4626) and a new interest rate model with a continuous and differentiable (non-linear) function that will set the basis for the development of a fixed income derivative market.

**The Exactly value proposition:**

* **Simplicity**: Traders can arbitrage between fixed and variable rates for various time periods and hedge the interest rate risk for their long or short positions, with or without leverage.
* **Frictionless**: Investors and DAOs can receive fixed and variable deposit rates. End-users can take fixed-interest rate loans for more extended time periods with certainty.
* **Efficiency**: Fixed and variable interest rates live in the same protocol with a new approach towards multiple interest rate discovery through the Utilization Rate of each Fixed Rate Pool.

Being an open-source, non-custodial, and autonomous interest rate protocol, Exactly came into existence to decentralize the time value of money and complete the DeFi credit market.

### Who developed Exactly Protocol?

Exactly was started in July 2021 and launched on Ethereum Mainnet in November 2022 by a team of stakeholders with expertise in technology, economics, finance, and math. You can find more info about us on [Linkedin.](https://linkedin.com/company/exactly-protocol)

## How it works

### What is the Variable Rate Pool?

The Variable Rate Pool is a pool containing a single type of asset without an expiration date.

This pool provides liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed to ensure they can still satisfy the demand for new loans when deposits are insufficient to cover the requested amounts. Once a new deposit is made in a Fixed Rate Pool it will automatically replace the Variable Rate Pool’s original funding, which in turn “leaves” retaining a small fraction of the interest fees as earnings for providing early liquidity in the first place.

There is one Variable Rate Pool and many Fixed Rate Pools for each of the assets allowed in the protocol.

### What is an "exaToken"?

Users can supply their assets and increase the liquidity of the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), which will, in turn, provide liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed. Each deposit will mint an "Exactly Token" (exaToken) that uses the [ERC-4626](../guides/protocol/market/erc-4626.md) standard, which will be provided to the user as a voucher for the deposited amount. These exaTokens will periodically accrue variable earnings by increasing their value when withdrawing and exchanging back for the underlying assets. Even though the main goal is to solve the problem of fragmented liquidity across different Fixed Rate Pools, it is also noteworthy that the exaToken extends on the ERC-20 standard, meaning that it can be exchangeable, adding composability across other protocols.

Therefore, exaToken holders have the capability of redeeming and receiving their original assets plus their interests at any time, subject to available liquidity in the Variable Rate Pool.

### What is a Fixed Rate Pool?

A Fixed Rate Pool is a pool that has a maturity date (term horizon) containing a single type of asset. Users can supply or borrow assets from these pools once they put their [collateral](faq.md#what-is-the-health-factor) on the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool). Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate and its fixed interest rate for new loans as a consequence.

### How to borrow an asset?

In order to borrow an asset in the protocol you should first deposit any asset in the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), enable that asset as collateral in [your dashboard](https://app.exact.ly/dashboard), and then you will be able to borrow any asset paying a variable or a fixed interest rate according to your preference.

### What is the Health Factor?

This is how “safe” your loan is, calculated as the proportion of collateral deposited versus the amount borrowed. A health factor above 1.25 is recommended to avoid liquidation.

### What happens if the price of my collateral changes?

When the price of your collateral changes, your [Health Factor](faq.md#what-is-the-health-factor) changes. The minimum collateralization ratio that you need to maintain will vary depending on the asset that you're borrowing and the collateral type you're using.

For example, the minimum collateralization ratio for a USDC loan against ETH could be 3:2, or 150%. If your collateralization ratio falls below the minimum 150%, you become eligible for [liquidation](faq.md#what-is-a-liquidation).

### What is collateral liquidation?

During liquidation, a liquidator purchases a portion of a user's collateral at a discount to the on-chain oracle price and repays some of the liquidated user's debt.

The liquidator can purchase some part of the user's collateral depending on the Dynamic Close Factor, even if the user is only slightly undercollateralized.

### What happens if the price of my collateral changes?

In order to return the borrower's account to solvency as fast as possible, and involving as few liquidations as possible, the protocol has a Dynamic Close Factor (based on the user’s degree of insolvency) that is the proportion of outstanding borrows that must be repaid in order to return a user to a solvency situation.

### Can I repay or withdraw my fixed position earlier?

You can exit your fixed deposit or repay your fixed loan at any time subject to the liquidity that protocol has at that moment that will determinate the correspondent market interest rate for discounting the present value of your deposit or your loan.

Read more [here](../guides/fixed-rate-operations/).

### What happens if I don't repay my fixed borrow at maturity?

There is a penalty fee percentage for not repaying your borrow on the maturity date.

Read more [here](../guides/fixed-rate-operations/borrows.md).

### What is the best collateral ratio to borrow at?

The collateral ratio you choose determines the likelihood that your collateral gets liquidated. The lower your collateral ratio, the greater your risk of liquidation. Choosing the right collateral ratio for you depends on how much risk you want to take and how actively you plan on managing your positions.

### Which are the revenue sources of the Variable Rate Pool?

Liquidity providers receive earnings from four different sources:

1. Variable interest rate fees paid by borrowers on the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool)
2. Commissions for providing [early liquidity](white-paper.md#2.1-supplying-assets-to-the-variable-rate-pool) on [Fixed Rate](faq.md#what-is-a-fixed-rate-pool) loans (a.k.a.: "exit commissions")
3. Penalties paid by users who repay their debts after maturity on [Fixed Rate](faq.md#what-is-a-fixed-rate-pool) loans
4. A profit share of the [liquidation](faq.md#what-is-a-liquidation) fee

Incentives 2, 3, and 4 are extraordinary events and generate earnings that are gradually distributed to [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool) depositors.

## Community Involvement

### Where is the developer documentation?

We're continuously updating our developer documentation in [protocol](../guides/protocol/ "mention").

### Where are your branding guidelines/materials?

You can check our branding guidelines and materials in [brand-assets.md](../resources/brand-assets.md "mention").

### Where can I propose new ideas?

We have a [forum on Discord](https://exact.ly/discord/) where you can share and discuss your thoughts with the Exactly community.

## Exactly dApp

### What oracles does Exactly use?

We use an aggregation of decentralized data feeds from [Chainlink](https://data.chain.link/).

### Markets

#### What does the "Deposit APR" or "Borrow APR" mean in the Variable Rate Pool board table?

It means is the change in the value of the underlying share in the last hour, annualized.

You can learn more about the incentives for depositors in [#which-are-the-incentives-for-providing-liquidity-to-the-variable-rate-pool](faq.md#which-are-the-incentives-for-providing-liquidity-to-the-variable-rate-pool "mention")

#### What does the "Deposit APR" or "Borrow APR" mean in the Fixed Rate Pool board table?

It means the marginal interest rate for a $1 deposit (or borrow) in that particular Fixed Rate Pool.

#### What does the "Best Deposit APR" or "Best Borrow APR" mean in the Fixed Rate Pool board table?

It means the highest fixed interest rate APR for a $1 deposit (or the lowest borrow APR) in all the available Fixed Rated Pools.

## Governance Token

### Is there a protocol token? How can I earn it?

Exactly Protocol does not currently have a governance token.

### Is there a TGE/ICO/IDO?

There is no public sale.

### Do testnet users receive rewards?

No, testnet is to preview upcoming features and for users to learn about the protocol without any Mainnet gas fees.

### Someone messaged me promising free tokens/ICO/etc, is it real?

No, that is fake. No one related to Exactly Protocol will ever message anyone directly, nor offer free tokens or investments of any kind.

## Partnerships

### Who can I contact about partnerships/integrations?

Feel free to reach out through [Discord](https://discord.gg/eNTyPvgA4P) or other platforms in the [Quick Links](quick-links.md) section.
