# 📃 White Paper

## Exactly Protocol

**Decentralizing the time value of money**

Authors: [Gabriel Gruber](https://github.com/GabrielGruber) and [Francisco Lepone](https://github.com/FranciscoLepone)

Version 1.1 (September 2022)

[whitepaper@exact.ly](mailto:whitepaper@exact.ly)

## 0. Abstract

In this white paper, we introduce a new decentralized, non-custodial, and open-source protocol that will provide an autonomous interest rate market to lenders and borrowers while setting interest rates based on credit supply and demand, enabling users to frictionlessly exchange the time value of their crypto assets at both variables and fixed interest rates for the first time in DeFi.

Aside from taking loans and making deposits at variable interest rates from a Variable Rate Pool, this protocol enables users to do so at fixed rates as well through the interaction with several Fixed Rate Pools, each one representing a specific maturity date. Interest rates are determined based on the credit utilization rate of each Fixed Rate Pool.

## 1. Introduction

Decentralized Finance (DeFi) is a new initiative based on blockchain technologies that aim to create a new financial system by building a network of interconnected distributed apps (or dApps) that use open-source and non-custodial protocols. Combining some of these dApps, not only can we create traditional financial instruments but also new ones that were not possible before.

Exactly’s mission is to develop a protocol that brings a missing important piece into the ecosystem: the "**Time Value of Money**"; that is, to be able to make a deposit or a loan at a fixed rate for a certain period of time, which is known in the traditional financial world as "fixed income". That way, users will have an efficient means to hedge the interest rate volatility.

Some protocols approach this problem using a peer-to-peer (P2P) strategy, where lenders and borrowers are individually matched. This approach, while conceptually simple, entails several inefficiencies related to matching both amount and time in each transaction. Protocols such as AAVE take a different direction, offering a stable (not fixed) rate if the market conditions remain within a certain threshold. While this property provides greater predictability, it does not eliminate the risk and it only works for borrowers below a certain market utilization rate.

Alternatively, a series of fixed-rate protocols were launched in the year 2021. Some of them, like Yield and Notional, use a token to create a component resembling a "Zero Coupon Bond", while others like Element and Pendle use two tokens to distinguish the principal and the interest rate, much like a "Coupon Stripping" approach, to try to discover the fixed interest rate based on the price of one or more assets with a certain expiration date represented by one or more ERC-20 tokens. These types of protocols have not managed to capture enough liquidity yet, probably due to their complexity when it comes to implementing them in the Ethereum blockchain.

Furthermore, when there is a slippage in the price of these types of tokens, the interest rate to be discovered is indirectly affected, resulting in the protocols requiring their own Automated Market Makers (AMMs) implementation. This poses additional challenges since these special tokens can’t be traded on AMMs such as Uniswap. The constant product invariant formula $x*y=k$ is not ideal for yield tokens, where time is an additional factor.

Liquidity is like water coming down a hill, and our goal is to remove the obstacles to make it flow fast and easily through the river. Our approach is based on finding a solution to the problem of discovering fixed interest rates, considering all the challenges that appear when bringing traditional finance to blockchain technologies while offering variable interest rate alternatives as well, thus covering the entire DeFi credit market. That is why Exactly protocol discovers the fixed interest rate directly from the supply and demand of credit that exists in each of our Fixed Rate Pools for each asset, for each maturity term.

## 2. The Exactly Protocol Architecture

The protocol completes the DeFi credit market with both variable and fixed interest rates using two types of pools per asset: the **Variable Rate Pool** and the **Fixed Rate Pool**.

### 2.1 Supplying Assets to the Variable Rate Pool

Users can supply their assets and increase the liquidity of the "Variable Rate Pool" (pools containing a single type of asset without an expiration date) that will in turn provide liquidity to all the different Fixed Rate Pools as needed. Each deposit will mint an "Exactly Token" (exaToken) that uses the [ERC-4626](../guides/protocol/market/erc-4626.md) standard, which will be provided to the user as a voucher for the deposited amount. These exaTokens will periodically accrue variable earnings by increasing their value at the time of withdrawing and exchanging back for the underlying assets. Even though the main goal is to solve the problem of fragmented liquidity across different Fixed Rate Pools, it is also noteworthy that the exaToken extends on the ERC-20 standard, meaning that it can be exchangeable, adding composability across other protocols.

Therefore, exaToken holders have the capability of redeeming and receiving their original assets plus their interests at any time, subject to available liquidity in the Variable Rate Pool.

The main purpose of a Variable Rate Pool is to provide immediate liquidity to any Fixed Rate Pool, to ensure it can still satisfy the demand for new loans when deposits are not enough to cover the requested amounts. Once a new deposit is made in a Fixed Rate Pool it will automatically replace Variable Rate Pool’s original funding, which in turn "leaves" retaining a small fraction of the interest fees as earnings for providing early liquidity in the first place. There is one Variable Rate Pool and many Fixed Rate Pools for each of the assets allowed in the protocol.

The liquidity of the Variable Rate Pool is being continuously used to match the demand for new loans in exchange for a small fee, so they can be reused in future loans. Naturally, rotation speed in fee collection is a key factor that serves as the foundation for the interest rate liquidity providers in the Variable Rate Pool will receive in the future: the faster the rotation, the higher their pick up rate. The Variable Rate Pool will also receive interest rate fees from users that borrowed assets at a variable rate.

Exactly protocol does not guarantee liquidity for withdrawals in any pool, but relies on its interest rate model to incentivize it. The protocol will also have a **Liquidity Reserve Requirement** as a fraction of the Variable Rate Pool deposits that cannot be borrowed and will only be available for withdrawals in the Variable Rate Pool.

### 2.2 Supplying Assets to the Fixed Rate Pools

Users can supply their assets to different "Fixed Rate Pools" (pools with a maturity date containing a single type of asset) depending on their term horizon preference. Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate and its fixed interest rate for new loans as a consequence.

The protocol also offers Flexible Fixed Rate Deposits: users are able to make a partial or total withdrawal of their deposit before maturity if there is enough available liquidity in that specific Fixed Rate Pool or in the Variable Rate Pool. Withdrawing a deposit before maturity will be comparable to making a loan request on the same Fixed Rate Pool (same asset and maturity) according to the interest rates prevailing at the time of withdrawal. This may result in getting back the original deposit with a discounted penalty depending on the market conditions at the time of the transaction.

### 2.3 Borrowing Assets from the Variable Rate Pool

exaTokens can be used as collateral for a variable interest rate loan taken from the Variable Rate Pool. Each asset supported in our protocol has its own **Risk-Adjust Factor**, which represents the proportion of the asset value to be used as collateral. For example, if a user supplies 100 ETH as collateral, and the Risk-Adjust Factor for ETH is 50%, then that user can borrow a maximum of 50 ETH worth of any other asset in any Variable Rate Pool.

### 2.4 Borrowing Assets from the Fixed Rate Pools

exaTokens can be used as the collateral for a fixed rate loan in any of the Fixed Rate Pools. Once the user defines the amount, asset, and maturity date, they will get the specific fixed interest rate to be paid for that loan at maturity.

The protocol also offers Flexible Fixed Rate Loans the user will be able to repay earlier or after the maturity date (with an extra penalty fee in the latest case). If an early repayment is made, the user might be able to repay less of the expected amount depending on the market conditions at the time of the transaction.

![Variable and Fixed Rate Pools](https://lh6.googleusercontent.com/ZyJV5sTjoxZA0TX53EbaR4bgvenFsUdfR7EKu9zoB4iBQeDjhE8ix\_1JC3NaG1HWMlJc6eQzfx0QhjPKtqduVGf44I7GBoZOR-EkhcpfJPQ1SeoikSPV6OKwal-\_w\_sh\_9Ua-8LMeP637yd50EZ3xmg)

### 2.5 Liquidations

If the user’s Health Factor is below 1, meaning that his outstanding borrowing exceeds the sum of all his exaTokens multiplied by each Risk-Adjust Factor, a portion of the outstanding borrowing may be repaid by any third party in exchange for the user’s proportional exaToken collateral at a discount price. Additionally, a small fee will be received by the Variable Rate Pool as compensation for absorbing "bad debt" residuals after all the proportional collateral is liquidated.

Any user may invoke the liquidation function in a permissionless way. In order to return the borrower’s account to solvency as fast as possible, and involving as few liquidations as possible, the protocol has a **Dynamic Close Factor** (based on the user’s degree of insolvency) that is the proportion of outstanding borrows that must be repaid in order to return a user to a solvency situation.

## 3. The Exactly Interest Rate Model

Besides an interest rate function for the Variable Rate Pool, the Exactly protocol has a demand curve for interest rates that is based on the Utilization Rate of each Fixed Rate Pool. Increasing and decreasing the interest rate incentivizes lenders to provide additional liquidity and borrowers to request more credit, ​​respectively. Thus, this mechanism favors the convergence towards an equilibrium between supply and demand. Since each of the assets are a certain number of maturities, by observing the interest rate term structure users can determine which pools are most interesting to participate in.

Exactly adopts for lending rates a continuous and differentiable function of the Utilization Rate. The function was designed to diverge asymptotically for a certain boundary value of utilization so that it acts as a natural barrier for credit demand as the level of utilization depletes the protocol liquidity capabilities. The curve can be easily parametrized to adjust it to changing market conditions. In principle, there will be a demand function for each asset and each maturity.

Conceptually, the function was designed in such a way that it naturally divides the utilization domain into three well-differentiated regions. The first region (I) of normal rates is called the "normal regime" where the utilization levels are well below the available liquidity; a second region (II) is called the "leveraged regime" where interest rates increase as utilization levels start to exhaust the available resources; and a third region (III) called "unreachable-regime" where rate levels are even higher, eventually diverging, and where it is not possible to take credits.

![](https://lh6.googleusercontent.com/EFCQHnok7Cv0hc3GwCV5GHEUDlBu\_fehGEKDJpchqxCqAZWomWOwLfHc1uPcjiZ6jdrYyjoh4RSaeNQkLM1YUNp1CmIruAFwHjxYiX2Pcfen8Fd-v7znIL9iZu\_unTC8ifllr2Dilk5NMbtc8ZaJBkg)

### 3.1 Borrow Interest Rate in Fixed Rate Pools

The borrow interest rate for each Fixed Rate Pool is a rational function that aims to incentivize liquidity on each of the Fixed Rate Pools but doesn’t guarantee it. The function takes the following form:

$$
\begin{align*}
  R(U) = \frac{A}{(U_{max} - U)} + B
\end{align*}
$$

Where $U$ is the Utilization Rate and $A$, $B$ and $U_{max}$ are parameters whose values are obtained either from calibration against relevant market data or defined by the Risk Management Committee multisig (see section 4, "Governance").

The Utilization Rate in each of the Fixed Rate Pools at any time $t$ is defined as:

$$
\begin{align*}
  U_{FR,i}^{t} = \frac{TB_{FR,i}^t}{TD_{FR,i}^t + \frac{⟨SS^t⟩}{\tau_{FR}}}
\end{align*}
$$

Where $TB_{FR,i}^t$ is the total amount of outstanding borrows at time $t$ in the Fixed Rate Pool, $TD_{FR,i}^t$ is the total outstanding deposits, $⟨SS^t⟩$ is a moving average of the total supply in the Variable Rate Pool for this particular asset and $\tau_{FR}$ is a customizable parameter that regulates the fraction of liquidity from the Variable Rate Pool that is a priori assigned to each Fixed Rate Pool.

One of the main differences between present money market protocols and Exactly’s approach to fixed interest rates is that each user receives or pays a fixed rate on maturity when they transact on our platform. Therefore, the choice of the appropriate Utilization Rate becomes so important.

In existing variable rate frameworks, fixing the initial rate based on the state of liquidity before the transaction is made is not a concern because rates will be adjusted in the next transaction. Under a fixed rate environment, this approach might promote users to take advantage and capture all the liquidity available at a current low rate. Using an ex-post Utilization Rate to fix the interest rate does not solve the problem since we would be overcharging costs to users. The most appropriate approach to solve this problem is making investors indifferent to the decision of getting a loan for the total desired amount or splitting it into successive smaller loans. To do that, the protocol will need to determine the effective interest rate that satisfies the condition, i.e.:

$$
\begin{align*}
  ⟨R⟩_{FR,i}^{t_{k+1}} = \frac{\int_{U_{FR,i}^{t_k}}^{U_{FR,i}^{t_{k+1}}} R(u) du}{({U_{FR,i}^{t_{k+1}}} - {U_{FR,i}^{t_k}})}
\end{align*}
$$

![](https://lh6.googleusercontent.com/M8KyNtB5\_2A7U8selT0CM2JWi5wnIiaXfaPlqN0NS6VgqFh471LrD2useUVhgPNzkP1efjwV8L7Zvbwb4SsIAGnqlrrldBPac0S-y-CZ4vjd1ksOxmHC-aMVH7Ms7JVt76RlXWoLtfFP4gRdjfIYG4k)

### 3.2 Borrow Interest Rate in the Variable Rate Pool

Users can also take loans at variable rates in a similar way to existing money market protocols. Under the Exactly architecture, variable rate borrows take place in a special pool exclusively designed to use the Variable Rate Pool liquidity. In this pool, the only allowed operations are borrows and repayments.

In order to assure the optimal behavior of the protocol, a different definition of Utilization Rate is needed in this case. Between any two operations in the Variable Rate Pool, we define the Utilization Rate as follows:

$$
\begin{align*}
  U_{VR}^{t} = \frac{TB_{VR}^t}{⟨SS^t⟩ / \tau_{VR}}
\end{align*}
$$

Where $TB_{VR}^t$ is the total amount of variable rate borrowed outstanding at time $t$, and $\tau_{VR}$ is a customizable parameter that regulates the fraction of liquidity from the Variable Rate Pool that is assigned to variable rate loans.

### 3.3 Supply Interest Rate in a Fixed Rate Pool

In economics, market clearing is the process by which the supply of whatever is traded is equated to the demand so that there is no leftover supply or demand. Under the Exactly Protocol, users supplying and demanding credit have access to the same information on the blockchain, so there is no friction preventing interest rate changes thus they will always adjust up or down to ensure market clearing.

To accomplish the market clearing condition, supply interest rates are determined by the number of pending interest payments available to be distributed between the Fixed Rate Pool depositors and the Variable Rate Pool. This condition is dynamic and must hold true at any time. The exact distribution among each pool will depend on their proportional contribution to the backing of loans.

#### Example

Assume a Fixed Rate Pool with maturity at $t=1$. At $t=0$ there is a 10M lending request at a 10% interest rate (Request A). Because there is no external supply in this Fixed Rate Pool, the operation is funded by the Variable Rate Pool. At $t=0.4$ there is a 10M deposit made by a user in this Fixed Rate Pool.

The Variable Rate Pool will leave the Fixed Rate Pool recovering its original deposit and having earned 400k of accrued interest plus 10% of the pending interests as earnings for providing liquidity in the first place. Thus, the net interest rate to be paid to the user’s deposit in the Fixed Rate Pool at maturity is effectively 9%. At $t=0.5$ a new loan request of 3M (Request B) is backed again by the Variable Rate Pool at a 6% interest rate.

After that, there is a new deposit at $t=0.7$ that replaces the Variable Rate Pool like in the previous case, for a remaining net interest rate of 5.4% (that’s the original 6% minus 0.6% retained by the Variable Rate Pool). Finally, at $t=0.8$ there is a last lending request for 5M (Request C) satisfied by the Variable Rate Pool at a rate of 8%.

At $t=1$ all lending requests are repaid as follows:

1. 10M + 10% = 11M.Variable Rate Pool earnings: 460K, Depositor earnings: 540k
2. 3M + 6%/2 = 3.09M. Variable Rate Pool earnings: 41.4K, Depositor earnings: 48.6k
3. 5M + 8%/5 = 5.080M. Variable Rate Pool earnings: 80.0K

![](https://lh6.googleusercontent.com/7gAv-LxzScdXFHImxtg8ce5CakXNWJ9871vFOqo7tYL6ToM0B3GjyuCXVMYbiF9t211jWJTyr103-YcEr1L3Q39uj74i\_r34WnEL6pJYPsTVIqNRACpNPDbktUkeqYkMe3ay6L6AaBE-AMyKepPBIA)

## 4. Governance

The protocol parameters could be updated by the Risk Management Committee multisig. The main customizable parameters are the following:

* List a new asset
* Update the interest rate model per market
* Update the Risk-Adjust Factor per asset
* Update the liquidator incentive
* Update the liquidity reserve

The multisig has a timelock in order to communicate and give time to the users using the protocol to react to any change made by the Risk Management Committee.

Exactly began with centralized governance of the protocol’s parameters in order to get product market fit and will transition to a complete decentralization over time where the Risk Management Committee multisig will be replaced by a Decentralized Autonomous Organization (DAO) and Exactly tokens will be minted to the community.

## 5. Summary

With an innovative approach, the protocol allows users to lend and borrow assets at fixed and variable rates in a more efficient way through the implementation of the ERC-4626 and a new interest rate model with a continuous and differentiable (not linear) function that will set the basis for the development of a fixed income derivative market.

The Exactly value proposition:

* Simplicity: Traders can arbitrage between fixed and variable rates for various time periods and hedge the interest rate risk for their long or short positions, with or without leverage.
* Frictionless: Investors and DAOs can receive fixed and variable rates on their deposits. End-users can take fixed-interest rate loans for longer time periods with certainty.
* Efficiency: Fixed and variable interest rates live in the same protocol with a new approach towards multiple interest rate discovery through the Utilization Rate of each Fixed Rate Pool.

Being an open-source, non-custodial, and autonomous interest rate protocol, Exactly came into existence to decentralize the time value of money and complete the DeFi credit market.

## 6. References

1. Uniswap (2018), [https://docs.uniswap.org](https://docs.uniswap.org)
2. Compound (2019), [https://compound.finance/docs](https://compound.finance/docs)
3. AAVE (2020), [https://docs.aave.com](https://docs.aave.com)
4. Yield (2020), [https://docs.yieldprotocol.com](https://docs.yieldprotocol.com)
5. Notional (2020), [https://docs.notional.finance](https://docs.notional.finance)
6. Liquity (2020) [https://docs.liquity.org/](https://docs.liquity.org/)
7. Element (2021), [https://paper.element.fi](https://paper.element.fi)
8. Pendle (2021), [https://docs.pendle.finance](https://docs.pendle.finance)

## 7. Disclaimer

This white paper is for general information purposes only. It does not constitute investment advice or a recommendation or a solicitation to buy or sell any investment and should not be used in the evaluation of the merits of making any investment decision. It should not be relied upon for accounting, legal or tax advice, or investment recommendations. This white paper reflects the current personal opinions of the authors and is subject to change without being updated.

## Download the White Paper

[https://github.com/exactly/papers/blob/main/ExactlyWhitePaperV1.1.pdf](https://github.com/exactly/papers/blob/main/ExactlyWhitePaperV1.1.pdf)
