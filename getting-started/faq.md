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

Being an open-source, non-custodial, and autonomous interest rate protocol, Exactly came into existence to decentralize the credit market and complete the DeFi ecosystem.

### Who developed Exactly Protocol?

Exactly was started in July 2021 and launched on Ethereum Mainnet in November 2022 by a team of stakeholders with expertise in technology, economics, finance, and math. You can find more info about us on [Linkedin](https://linkedin.com/company/exactly-protocol) and on [GitHub](https://github.com/orgs/exactly/people).

## How it works

### What is the Variable Rate Pool?

The Variable Rate Pool is a pool containing a single type of asset without an expiration date.

This pool provides liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed to ensure they can still satisfy the demand for new loans when deposits are insufficient to cover the requested amounts. Once a new deposit is made in a Fixed Rate Pool it will automatically replace the Variable Rate Pool’s original funding, which in turn “leaves” retaining a small fraction of the interest fees as earnings for providing early liquidity in the first place.

There is one Variable Rate Pool and many Fixed Rate Pools for each of the assets allowed in the protocol.

### What is an "exaToken"?

Users can supply their assets and increase the liquidity of the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), which will, in turn, provide liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed. Each deposit will mint an "Exactly Token" (exaToken) that uses the [ERC-4626](../guides/protocol/market/erc-4626.md) standard, which will be provided to the user as a voucher for the deposited amount. These exaTokens will periodically accrue variable earnings by increasing their value when withdrawing and exchanging back for the underlying assets. Even though the main goal is to solve the problem of fragmented liquidity across different Fixed Rate Pools, it is also noteworthy that the exaToken extends on the ERC-20 standard, meaning that it can be exchangeable, adding composability across other protocols.

Therefore, exaToken holders have the capability of redeeming and receiving their original assets plus their interests at any time, subject to available liquidity in the Variable Rate Pool.

### Are exaTokens transferable?

Yes, they can be transferred. Transferring the exaTokens would mean transferring the variable deposit position, and this can be done with any amount, not necessarily the whole position. Nevertheless, if the transferred amount causes a shortfall in the original address ([Health Factor < 1](faq.md#what-is-the-health-factor)) the transaction will be reverted.

### What is a Fixed Rate Pool?

A Fixed Rate Pool is a pool that has a maturity date (term horizon) containing a single type of asset. Users can supply or borrow assets from these pools once they put their [collateral](faq.md#what-is-the-health-factor) on the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool). Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate and its fixed interest rate for new loans as a consequence.

### How to borrow an asset?

In order to borrow an asset in the protocol you should first deposit any asset in the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), enable that asset as collateral in [your dashboard](https://app.exact.ly/dashboard), and then you will be able to borrow any asset paying a variable or a fixed interest rate according to your preference.

### What is the Health Factor?

The Health Factor represents how “safe” your leverage portfolio is, defined as the risk-adjusted proportion of collateral deposited divided by the borrowed risk-adjusted amount. A health factor below 1x will be considered with a shortfall and open to liquidation.

### How is the Health Factor calculated?

The Health Factor is calculated from the user's collateral balance (in ETH) multiplied by each asset's [adjust factor](../guides/parameters.md#d.-risk-factors), divided by the user's debt which is also divided by this adjust factor.

#### Example:

Given an ETH adjusted factor of 0.84, a deposit of 100 ETH, and borrow of 50 ETH, the Health Factor will be 1.41:

$$
\frac{100 * 0.84}{50 \div 0.84}=\frac{84.00}{59.52}=1.41
$$

Below a Health Factor of 1.00, the user will be considered in a shortfall and open to [liquidation](../guides/liquidations/).

### What happens if the price of my collateral changes?

When the price of your collateral changes, your [Health Factor](faq.md#what-is-the-health-factor) changes. The minimum collateralization ratio you need to maintain will vary depending on the asset you're borrowing and the collateral type you use.

### What is collateral liquidation?

During liquidation, a liquidator purchases a portion of a user's collateral at a discount to the on-chain oracle price and repays some of the liquidated user's debt.

The liquidator can purchase some part of the user's collateral depending on the Dynamic Close Factor, even if the user is only slightly undercollateralized.

### What is the Dynamic Close Factor?

To return the borrower's account to solvency as fast as possible and involving as few liquidations as possible, the protocol has a Dynamic Close Factor (based on the user’s degree of insolvency) that is the proportion of outstanding borrows that must be repaid to return a user to a solvency situation.

### Can I repay or withdraw my fixed position earlier?

You can exit your fixed deposit or repay your fixed loan at any time, subject to the liquidity of the protocol that will determine the correspondent market interest rate for discounting the present value of your deposit or your loan.

Read more [here](../guides/fixed-rate-operations/).

### What happens if I don't repay my fixed borrow at maturity?

There is a daily penalty fee rate for not repaying your borrow before the maturity date.

Read more [here](../guides/fixed-rate-operations/borrows.md).

### What is the best collateral ratio to borrow at?

The collateral ratio you choose determines the likelihood that your collateral gets liquidated. The lower your collateral ratio, the greater your risk of liquidation. Choosing the right collateral ratio depends on how much risk you want to take and how actively you plan to manage your positions.

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

## Exactly Web App

### What oracles does Exactly use?

We use an aggregation of decentralized data feeds from [Chainlink](https://data.chain.link/).

### How are interest rates calculated?

#### Borrow Interest Rates

* **Variable Interest Rate**: It's a [rational function](https://docs.exact.ly/guides/interest-rates-curves) that depends on the Utilization Rate of the Variable Rate Pool in every block.
  * We display on [Markets](https://app.exact.ly/) (Simple or Advanced View) the interest rate from the current utilization rate of the Variable Rate Pool.
  * Then, when entering the amount (Simple View or Modal), we show the new variable interest rate based on how the pool's utilization has changed, given the input.
* **Fixed Interest Rates**: It's also a rational function but depends on the Utilization Rate of the specific Fixed Rate Pool after the amount borrowed.
  * We display by default on Markets (Advanced View) the best-fixed rate for a marginal change in the fixed rate pool's utilization rate.
  * Then, when entering the specific amount (Simple View or modal window in the Advanced View), we calculate the average value of the integral of the rational function taking into account the change in the utilization after the amount borrowed, to get the specific fixed interest rate that the user will have to pay until the pool's maturity.

#### Deposit Interest Rates

* **Variable Interest Rate**: It's the change in the value of the shares of the Variable Rate Pool, based on its different revenue sources: variable rate interest fees + fixed-rate interest fees and commissions, + liquidation fee to compensate for bad debt.
  * We display on Markets by default (Simple View and Advanced View) the annual rate that arises from the change in the value of Variable Rate Pool shares during the last 15 minutes.
  * Then, when entering the specific amount (Simple View or modal window in the Advances View), we show the new interest rate based on how the utilization has changed, given the input.
* **Fixed Interest Rates**: The user will get an annual rate based on his deposit amount and the current fixed interest rate fees from borrows that the Fixed Rate Pool will collect until maturity.
  * We display on Markets the best-fixed rate (Advanced View) for a marginal change in utilization in the Fixed Rate Pools or just the current marginal utilization for each Fixed Rate Pool (Simple View)
  * Then, when entering the amount (Simple View or modal window in the Advance View), we calculate the specific fixed interest rate based on his deposit amount and the current fixed interest rate fees from borrows that the Fixed Rate Pool will collect until maturity.

## Governance Token

### Is there a protocol token? How can I earn it?

Exactly Protocol does not currently have a governance token.

### Is there a TGE/ICO/IDO?

There is no public sale.

### Do Testnet users receive rewards?

No, testnet is to preview upcoming features and for users to learn about the protocol without any Mainnet gas fees.

### Someone messaged me promising free tokens/ICO/etc, is it real?

No, that is fake. No one related to Exactly Protocol will ever message anyone directly nor offer free tokens or investments.

## Partnerships

### Who can I contact about partnerships/integrations?

Feel free to reach out through [Discord](https://discord.gg/eNTyPvgA4P) or other platforms in the [Quick Links](quick-links.md) section.
