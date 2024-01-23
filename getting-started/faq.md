# ❔ FAQ

## General

### What is Exactly?

Exactly is a new decentralized, non-custodial, and open-source protocol that provides an autonomous interest rate market to lenders and borrowers while setting interest rates based on credit supply and demand, enabling users to frictionlessly exchange the time value of their crypto assets at both variables and fixed interest rates for the first time in DeFi.

Aside from taking loans and making deposits at variable interest rates from a Variable Rate Pool, this protocol enables users to do so at fixed rates through interaction with several Fixed Rate Pools, each representing a specific maturity date. Interest rates are determined based on the credit utilization rate of each Fixed Rate Pool.

### Why does Exactly matter?

With an innovative approach, the protocol allows users to lend and borrow assets at fixed and variable rates in a more efficient way through the implementation of the [ERC-4626](https://docs.exact.ly/guides/protocol/market/erc-4626) and a new [interest rate model](https://docs.exact.ly/guides/protocol/interestratemodel) with a continuous and differentiable (non-linear) function that will set the basis for the development of a fixed income derivative market.

**The Exactly value proposition:**

* **Simplicity**: Traders can arbitrage between fixed and variable rates for various periods and hedge the interest rate risk for their long or short positions, with or without leverage.
* **Frictionless**: Investors and DAOs can receive fixed and variable deposit rates. End-users can take fixed-interest rate loans for more extended periods with certainty.
* **Efficiency**: Fixed and variable interest rates live in the same protocol with a new approach towards multiple interest rate discovery through the Utilization Rate of each Fixed Rate Pool.

Being an open-source, non-custodial, and autonomous interest rate protocol, Exactly began to decentralize the credit market and complete the DeFi ecosystem.

### Who developed Exactly Protocol?

Exactly was started in July 2021 and launched on Ethereum Mainnet in November 2022 by a team of stakeholders with expertise in technology, economics, finance, and math. You can find more info about us on [Linkedin](https://linkedin.com/company/exactly-protocol) and on [GitHub](https://github.com/orgs/exactly/people).

## How it works

### What is the Variable Rate Pool?

The Variable Rate Pool contains a single type of asset without an expiration date.

This pool provides liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed to ensure they can still satisfy the demand for new loans when deposits are insufficient to cover the requested amounts. Once a new deposit is made in a Fixed Rate Pool, it will automatically replace the Variable Rate Pool’s original funding, which in turn “leaves” retaining a small fraction of the interest fees as earnings for providing early liquidity in the first place.

There is one Variable Rate Pool and many Fixed Rate Pools for each asset allowed in the protocol.

### What is an "exaVoucher"?

Users can supply their assets and increase the liquidity of the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), which will, in turn, provide liquidity to all the different [Fixed Rate Pools](faq.md#what-is-a-fixed-rate-pool) as needed. Each deposit will mint an "Exactly Voucher" (exaVoucher) that uses the [ERC-4626](../guides/protocol/market/erc-4626.md) standard, which will be provided to the user as a voucher for the deposited amount. These exaVouchers will periodically accrue variable earnings by increasing their value when withdrawing and exchanging back for the underlying assets. Even though the main goal is to solve the problem of fragmented liquidity across different Fixed Rate Pools, it is also noteworthy that the exaVoucher extends on the ERC-20 standard, meaning that it can be exchangeable, adding composability across other protocols.

Therefore, exaVoucher holders have the capability of redeeming and receiving their original assets plus their interests at any time, subject to available liquidity in the Variable Rate Pool.

### Are exaVouchers transferable?

Yes, they can be transferred. Transferring the exaVouchers would mean transferring the variable deposit position, and this can be done with any amount, not necessarily the whole position. Nevertheless, if the transferred amount causes a shortfall in the original address ([Health Factor < 1](faq.md#what-is-the-health-factor)), the transaction will be reverted.

### What is a Fixed Rate Pool?

A Fixed Rate Pool is a pool that has a maturity date (term horizon) containing a single type of asset. Users can supply or borrow assets from these pools once they put their [collateral](faq.md#what-is-the-health-factor) on the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool). Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate and its fixed interest rate for new loans as a consequence.

### How to borrow an asset?

To borrow an asset in the protocol, you should first deposit any asset in the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool), enable that asset as collateral in [your dashboard](https://app.exact.ly/dashboard), and then you will be able to borrow any asset paying a variable or a fixed interest rate according to your preference.

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

### How can I determine the maximum borrowing capacity based on my collateral deposit?

This is also known as maximum Loan-to-Value.

To calculate the maximum Loan-to-Value (LTV) for a deposit you need to consider the Risk-Adjust Factors of the assets involved in the transaction, both for the deposited asset (collateral) and the borrowed asset. The formula to calculate LTV is as follows:

LTV = Risk-Adjust Factor deposit \* Risk-Adjust Factor borrow.

Here's an example: let's say you want to deposit `ETH` as collateral and borrow `USDC`. In this case, the Risk-Adjust Factors are 0.84 for `ETH` and 0.91 for `USDC`. To calculate the LTV, you would multiply these factors:

LTV = 0.84 \* 0.91 = 0.7644

Next, you should divide the LTV by your desired [Health Factor](faq.md#what-is-the-health-factor), which represents the safety margin for your loan, with higher values indicating a lower risk of liquidation. In our example, let's assume a Health Factor of 1.05:

Adjusted LTV = LTV / Health Factor = 0.7644 / 1.05 = 0.728

Now, to determine the amount you can borrow, multiply the adjusted LTV by the value of your deposited collateral. If you deposit $10,000 worth of `ETH`, you can borrow:

Amount to borrow = Adjusted LTV \* Deposit Amount = 0.728 \* $10,000 = $7,280

In this example, depositing $10,000 worth of `ETH` allows you to borrow $7,280 worth of `USDC`, given the Risk-Adjust Factors and a Health Factor of 1.05.

You can find the Risk-Adjust Factors for each asset in the [parameters.md](../guides/parameters.md "mention") section.

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

There is a [daily penalty fee rate](https://docs.exact.ly/guides/parameters#j.-penalty-rate) for not repaying your borrow before the maturity date.

Read more [here](../guides/fixed-rate-operations/borrows.md).

### What is the best collateral ratio to borrow at?

The collateral ratio you choose determines the likelihood that your collateral gets liquidated. The lower your collateral ratio, the greater your risk of liquidation. Choosing the right collateral ratio depends on how much risk you want to take and how actively you plan to manage your positions.

### Which are the revenue sources of the Variable Rate Pool?

Liquidity providers receive earnings from four different sources:

1. Variable interest rate fees paid by borrowers on the [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool)
2. Commissions for providing [early liquidity](../resources/white-paper.md#2.1-supplying-assets-to-the-variable-rate-pool) on [Fixed Rate](faq.md#what-is-a-fixed-rate-pool) loans (a.k.a.: "exit commissions")
3. Penalties paid by users who repay their debts after maturity on [Fixed Rate](faq.md#what-is-a-fixed-rate-pool) loans
4. A profit share of the [liquidation](faq.md#what-is-a-liquidation) fee

Incentives 2, 3, and 4 are extraordinary events and generate earnings that are gradually distributed to [Variable Rate Pool](faq.md#what-is-the-variable-rate-pool) depositors.

### How does the protocol generate revenue?

Exactly Protocol generates revenue since the [Treasury Fee](https://docs.exact.ly/guides/parameters#b.-treasury-fee) parameter was activated on OP Mainnet in June 2023. The Treasury Fee refers to the percentage of interest rate charges paid by borrowers that the protocol retains for its treasury.

All collected Treasury Fees can be tracked in the "Treasury Fee" section in our official Dune Analytics dashboard: [https://dune.com/exactly/exactly](https://dune.com/exactly/exactly)

### What is the process for transferring assets from Ethereum Mainnet to OP Mainnet?

"Bridging" is the process of transferring tokens from Ethereum (Mainnet) to OP Mainnet. This allows users to take advantage of faster and cheaper transactions on this network, which is a layer 2 scaling solution.

Here's an outline of how to bridge assets:

1. Choose a bridge service that supports OP Mainnet. Some popular bridges include the following:
   * Hop Exchange ([https://hop.exchange/](https://hop.exchange/))
   * Optimism Bridge ([https://app.optimism.io/bridge](https://app.optimism.io/bridge/deposit))
2. Connect your Ethereum wallet (e.g., MetaMask) to the web3-enabled application. Ensure you have some ether (ETH) in your wallet for transaction fees.
3. Choose the token you want to bridge from Ethereum to OP Mainnet, such as ETH, or an ERC-20 token like DAI, and specify the amount you want to transfer.
4. Review the transaction details, including any fees associated with the bridge. Confirm the transaction in your wallet, and the bridge will initiate the transfer.
5. The transfer may take some time to complete depending on the bridge and network conditions. Once the transaction is confirmed on Ethereum and OP Mainnet, your assets will be available.
6. To interact with your assets on OP Mainnet, you'll need to switch your wallet's network to Optimistic Ethereum. In MetaMask, you can add the OP Mainnet network by following these steps:
   * Click on the network dropdown at the top of MetaMask.
   * Select "Custom RPC."
   * Enter the following network details:
     * Network Name: Optimistic Ethereum
     * New RPC URL: [https://mainnet.optimism.io](https://mainnet.optimism.io/)
     * Chain ID: 10
     * Currency Symbol: ETH
     * Block Explorer URL: [https://optimistic.etherscan.io](https://optimistic.etherscan.io/)
   * Save the new network.

You can now interact with your bridged assets on OP Mainnet using web3-enabled applications that support OP Mainnet, including [our own app](https://app.exact.ly/?n=optimism). Remember to switch back to Ethereum Mainnet when you need to interact with assets or applications on the main Ethereum network.

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

### What does Total Available refer to?

The Total Available value displayed in the web app is the sum of the variable and fixed pools available for withdrawals.

### What does Total Utilization refer to?

Total Utilization is the total utilization of the variable and fixed pool. Up to 90% can be lent out given the current reserve factor of [10%](https://docs.exact.ly/guides/parameters#a.-reserve-factor).&#x20;

### Why is the web app not available for US Persons?

As US Persons are prohibited from accessing and using the Digital Asset Services in any way, the platform does not allow its use by, or operation in any way with, US Persons. If we have reasonable grounds to suspect that you are a US Person, we reserve the right to take whatever action we deem appropriate to prohibit your access to the Digital Asset Services

## EXA Governance Token

### Is there a governance token?

Yes, the [EXA token](https://docs.exact.ly/guides/exa-token) is the Exactly Protocol's governance token. With the EXA token, community members can actively participate in the Protocol’s governance by voting on proposals for changes and upgrades.

More information about the EXA token can be found in the [EXA Token section](https://docs.exact.ly/guides/exa-token).

### What is the total circulating supply of the EXA token?

The total Circulating Supply and token holders can be found [here](https://optimistic.etherscan.io/token/tokenholderchart/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b).

### Do Testnet users receive rewards?

No, testnet is to preview upcoming features and for users to learn about the protocol without any Mainnet gas fees.

### Someone messaged me promising free tokens/ICO/etc, is it real?

No, that is fake. No one related to Exactly Protocol will ever message anyone directly nor offer free tokens or investments.

## Technical

### Is it possible to read the Variable Supply and Borrowing Rates from Smart Contracts?

The Variable Supply Rate cannot be read on-chain as it is an average of the earnings that the pool has generated over the last 15 minutes. These earnings come from different sources of income, such as:

* Debt charged to variable borrowers.
* Earnings that originate from Fixed Rate borrows are being backed up by the [Variable Rate Pool](https://docs.exact.ly/resources/white-paper#3.-the-exactly-interest-rate-model).
* Accumulator -> [Earnings Accumulator](https://docs.exact.ly/guides/features/earnings-accumulator).

The **Variable Borrow Rate** can be queried on-chain through the following steps:

1. Head to the Market's `floatingAssets` and `floatingDebt` view functions and query both values (i.e. [MarketUSDC](https://optimistic.etherscan.io/address/0x81C9A7B55A4df39A9B7B5F781ec0e53539694873#readProxyContract)).
2. Head to the Market's Interest Rate Model (IRM) `floatingRate` read function (i.e. [MarketUSDC’s IRM](https://optimistic.etherscan.io/address/0x8C2F35c8076bCb5D4b696bAE11AcA0ac0Dd873e4#readContract)).
3. For the `utilization` argument, enter the division between `floatingAssets` and `floatingDebt` (the result of this division needs to be then multiplied by `1e18` -> `1000000000000000000`).
4. Query the `floatingRate` function with the just calculated value. The result will be the current rate, represented with `18` decimals.

## Partnerships

### Who can I contact about partnerships/integrations?

Feel free to reach out through [Discord](https://discord.gg/eNTyPvgA4P) or other platforms in the [Quick Links](quick-links.md) section.

## Exactly Protocol Multisig Addresses

### What are the Exactly multisigs addresses?

The Exactly protocol holds the following multisig addresses:

| Multisig                                   | Address                                    |
| ------------------------------------------ | ------------------------------------------ |
| Exactly Protocol Owner on Ethereum Mainnet | 0x7A65824d74B0C20730B6eE4929ABcc41Cbe843Aa |
| Exactly Protocol Owner on OP Mainnet       | 0xC0d6Bc5d052d1e74523AD79dD5A954276c9286D3 |
| Exactly Treasury on OP Mainnet             | 0x23fD464e0b0eE21cEdEb929B19CABF9bD5215019 |



* Protocol Owner Multisig: This Multisig holds control over the entire protocol, including functions such as contract upgrades, parameter adjustments, and protocol pauses.
* Treasury Multisig: This Multisig is responsible for managing the funds in the DAO treasury, which includes activities like distributing EXA rewards, among others.

## Exactly Protocol Risk Assessment

### What is the Exactly risk assessment framework used for current assets?

The collateral risk assessment framework for current assets uses Conditional Value at Risk (CVaR), commonly called the expected shortfall. CVaR is a risk assessment metric employed to quantify the amount of tail risk associated with an investment portfolio. It is derived by computing a weighted average of the "extreme" losses within the tail section of the distribution of potential returns beyond the value at risk (VaR) cutoff point.

For detailed information on the current risk adjustment factors utilized within this framework, please refer to the following link: [Risk Adjustment Factors.](https://docs.exact.ly/guides/parameters#d.-risk-factors)

Concerning new assets, they must possess a Chainlink oracle price feed and maintain sufficient liquidity within the OP Mainnet for potential liquidations. As an illustrative example, the top 10 assets on Velodrome/Uniswap are considered suitable. It is important to note that, in the current protocol design, all assets can be utilized as collateral.
