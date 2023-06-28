# 🔣 Math Paper

## Exactly Protocol: A Model to Complete the Credit Market on the Ethereum Blockchain (Version 1.0)

Authors: [Francisco Lepone](https://github.com/FranciscoLepone) and [Gabriel Gruber](https://github.com/GabrielGruber)

[math@exact.ly](mailto:math@exact.ly)

July 2022

## Abstract

A model is proposed to complete the credit market on the Ethereum blockchain. It enables users to take loans and make deposits at fixed rates for various terms through the creation of Fixed Rate Pools.

Each Fixed Rate Pool represents a specific term. Interest rates are determined based on the utilization rate of each Fixed Rate Pool.

Additionally, the protocol offers the possibility of borrowing or parking liquidity at a floating rate through the implementation of a money market instrument in a Variable Rate Pool. The funding of the protocol is materialized through time deposits and the existence of a remunerated Smart Pool in charge of providing liquidity where required.

## 1. Introduction

Decentralized Finance (DeFi) based on blockchain technologies aims to create a new financial system by building a network of interconnected distributed apps (dApps) that use open-source and non-custodial protocols. By combining some of these dApps, we can recreate traditional financial instruments and even new ones.

Currently, there is a number of efficient protocols for borrowing and lending, such as Compound$$^1$$, AAVE$$^2$$, and Euler$$^3$$ among others. All of them have in common that they work with a short rate as a reference for lending costs. Thus, they involve binding deposits and credits to a cost of variable nature.

This work is an attempt to decentralize the credit market, bringing a missing important piece into the DeFi ecosystem, the "time value of money": That is, to be able to make deposits or take loans at fixed rates for a certain period of time, allowing users to hedge interest rate volatility and to lock costs and revenues.

Some protocols approach this problem using a peer-to-peer (P2P) strategy, where lenders and borrowers are individually matched. This strategy, while being conceptually sound and simple, entails several inefficiencies related to matching both amount and time in each transaction. Protocols like AAVE take a different direction, offering a stable (not fixed) rate if the market conditions remain within a certain threshold. While this property provides greater predictability, it does not eliminate the risk and it only works for borrowers below a certain market utilization rate.

Alternatively, a series of fixed-rate protocols were launched during the year 2021. Some of them, like Yield$$^4$$ and Notional$$^5$$, use a token to create a component resembling a "Zero Coupon Bond", while others like Element$$^6$$ and Pendle$$^7$$ use two tokens to distinguish the principal and the interest rate, much like a "Coupon Stripping" approach, to try to discover the fixed interest rate based on the price of one or more assets with a certain expiration date represented by one or more ERC-20 tokens. Unfortunately, these types of protocols have not managed to capture enough liquidity yet. This might be due to their complexity when implementing them in the Ethereum blockchain.

Furthermore, when there is a slippage in the price of these types of tokens, the interest rate to be discovered is indirectly affected. Consequently, the protocols require their own AMM implementation. This poses additional challenges since these special tokens can't be traded on AMMs such as Uniswap$$^8$$. The constant product invariant formula $$x*y=k$$ is not ideal for yield tokens, where time is an additional factor. Our goal is to devise a scheme that removes the mentioned obstacles and contributes to the growth of a complete fixed-income market in the blockchain. The protocol admits borrowing and lending at fixed interest rates for different maturities while still offering variable interest rate alternatives, thus covering the entire DeFi credit market.

## 2. Protocol Architecture

The protocol is aimed to satisfy the credit market with both variable and fixed interest rates by using two types of pools per asset: Variable Rate Pools and Fixed Rate Pools. Probably one of the biggest challenges of being able to establish an efficient (of considerable volume) fixed-term credit market on the blockchain is being able to match supply and demand simultaneously. In fact, if credit supply for a certain maturity arrives before credit demand, it is impossible to guarantee the supplier any given interest rate without putting at risk the protocol solvency. On the contrary, if credit demand arrives before credit supply, loans cannot materialize. To this respect, the role of the Variable Rate Pool is to immediately satisfy the credit demand for any maturity and stay there until there are enough deposits to match the borrowing. This is possible because any given interest rate is not promised to Variable Rate Pool liquidity contributors in advance.

## 3. Variable Rate Pool

Users can supply their assets to increase the liquidity of the Variable Rate Pool. There will be a Variable Rate Pool for each of the assets supported by the protocol. The role of the Variable Rate Pool is to provide immediate liquidity to any Fixed Rate Pool as required, ensuring that demand for new loans is satisfied. This is the mechanism Exactly protocol has to match credit supply and demand. When a new deposit is made in a certain Fixed Rate Pool, it replaces the prior contribution of the Variable Rate Pool to such pool. In turn, the Variable Rate Pool retains a small fraction of the interest fees as earnings for having provided liquidity in the first place. That way, the Variable Rate Pool liquidity is continuously reused to match the demand for new loans and afterward released in exchange for a fee, ready to be used again for future loans. The rotation speed of these dynamics plays a key role in the profitability of the Variable Rate Pool. The faster the rotation, the higher the rate at it builds up.

Furthermore, Variable Rate Pool liquidity is also available for variable rate loan funding. All the interest generated by this type of transaction is also to be collected by the Variable Rate Pool.

While the protocol does not guarantee liquidity in the pool, it relies on its interest rate model to incentivize it.

By design, the protocol sets aside a percentage of the Variable Rate Pool deposits as Liquidity Reserves. Liquidity Reserves cannot be borrowed and are only made available to meet withdrawal requests in the Variable Rate Pool.

Let $$TSS^t$$ be the total holdings in the Variable Rate Pool at any time $$t$$ and $$\eta$$ be the fraction of total holdings used as Liquidity Reserves or non-loanable holdings ($$RSS^t=\eta TSS^t$$). We define the loanable portion of the Variable Rate Pool ($$SS^t$$) as:

![math formula (1 & 2)](<../.gitbook/assets/image (23).png>)

## 4. Fixed Rate Pool

Users can supply and/or borrow assets to/from various Fixed Rate Pools depending on their time horizon preferences. Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate. Conversely, each new borrow takes out liquidity and increases the utilization rate. When there is a new transaction (deposit/borrow) in a Fixed Rate Pool, interest rates are determined based on the state of the system at this moment. Nevertheless, credit demand (borrows) and supply (deposits) rates are calculated using different principles.

Any depositor in a Fixed Rate Pool, can withdraw their assets before maturity, provided there is enough liquidity available in the protocol. Similarly, any debtor can repay their debt in advance and release their collateral.

### 4.1. Borrowing Assets at Fixed Rates

At any time during the life of a Fixed Rate Pool, any investor can ask for a loan provided they have enough collateral to back up the borrow. The collateral is calculated based on the aggregated amount of assets deposited in the various Variable Rate Pools (marked as collateral) that are not already in use to guarantee previous debts.

Let us consider the $$ith$$ Fixed Rate Pool ($$FRP_{i}$$) whose maturity operates in $$T_{i}$$. Loans inside a pool are financed by two types of funding: deposits in the same Fixed Rate Pool (loans, $$BF_{FR,i}$$) and contributions from the Variable Rate Pool (loans, $$BV_{FR,i}$$). The interest rate to be applied to each loan is determined by an interest rate demand function that depends on the utilization rate on that specific Fixed Rate Pool.

#### 4.1.1. Credit Demand Interest Rate Function

Exactly protocol has a specific interest rate demand curve for each Fixed Rate Pool and each asset. Increasing or decreasing the rate incentivizes lenders to provide additional liquidity or borrowers to request more credit, respectively. Thus, this mechanism favors the convergence towards an equilibrium between the supply and demand of credit. Since for each of the assets, there is a certain number of maturities, by observing the interest rate term structure users can also determine what pools are the most interesting to participate in.

We model interest rates as a function of the utilization rate ($$U$$) of each Fixed Rate Pool for every transaction by using a single, continuous and differentiable rational function

![math formula - (3)](<../.gitbook/assets/image (25).png>)

This function diverges asymptotically when $$U \rightarrow U_{max}$$ and it acts as a natural barrier to the credit demand as the level of utilization depletes the protocol liquidity capabilities.

The curve can be easily parametrized and adjusted to changing market conditions. In principle, there will be a demand function for each asset and each maturity. Curve parameters $$A$$, $$B$$, and $$U_{max}$$ are determined through calibration against relevant market data.

![math formula - (4 & 5)](<../.gitbook/assets/image (36).png>)

where $$U_{b}$$ conceptually represents the utilization level at the boundary between a region of normal interest rates ($$U \leq U_b$$) and a region of leveraged interest rates ($$U > U_b$$).

![math formula - ( 6 & 7)](<../.gitbook/assets/image (22).png>)

The utilization rate in each Fixed Rate Pool at any time $$t$$ is defined as

![math formula - (8)](<../.gitbook/assets/image (10).png>)

where $$TB_{FR,i}^t$$ is the total amount of outstanding borrows at time $$t$$ in the Fixed Rate Pool, $$TD_{FR,i}^t$$ is the total amount of deposits, $$⟨SS⟩^t$$ is a moving average of the total supply in the Variable Rate Pool for this asset (see 4.1.3), andFR is a configurable parameter that regulates the fraction of Variable Rate Pool total liquidity that is "naturally assigned" to each pool. In this way, and assuming no deposits are made to the Fixed Rate Pool, once all the "natural liquidity" is already borrowed ($$TB_{FR,i}^t=⟨SS⟩^t/\tau_{FR}$$) the utilization rate equals one ($$U_{FR,i}^t=1$$). For practical reasons, we will also choose $$U_b=1$$.

The utilization rate in any Fixed Rate Pool can be higher than unity, but interest rates will then rise at a faster pace beyond this value. The maximum value of allowed utilization in a given Fixed Rate Pool will be $$U_{full}=\tau_{FR}$$. That point is reached when all the Variable Rate Pool liquidity is being borrowed by a single Fixed Rate Pool (under the assumption that $$TD_{FR,i}^t=0$$).

In practice, we set

![math formula - (9)](<../.gitbook/assets/image (33).png>)

This choice enables a more flexible calibration of the curve (the lowest the value of $$\Lambda$$, the steepest the interest rates in the leveraged region).

The function was thought in such a way that it naturally divides the utilization domain into three well-differentiated areas (Fig. 1). The first region of normal rates (name it normal-regime) in which the utilization levels are well below total available liquidity. A second region in which interest rates increase at a faster pace (leveraged regime) as utilization levels start to exhaust available resources. And a third region (unreachable regime) where rate levels are even higher, eventually diverging and where it is not possible to take credit.

#### 4.1.2 The Effective Interest Rate for a Particular Loan

One of the differences between money market protocols and Exactly's approach to term borrowing/lending is that each user can receive/pay a fixed rate when they transact on our platform. Because of that, the choice of an appropriate utilization rate value becomes so crucial.

![Effective Interest Rate for a Particular Loan - figure 1](<../.gitbook/assets/image (5) (1).png>)

When at time $$t^{k+1}$$ there is a new request for a loan of size $$B_{FR,i}^{t_{k+1}}$$, if confirmed, the system would evolve from a utilization state $$U_{FR,i}^{t_{k}}$$ to a state $$U_{FR,i}^{t_{k+1}}$$ according to the following rule:

![math formula - (10)](<../.gitbook/assets/image (29).png>)

In variable rate frameworks, fixing the initial rate based on the state of utilization prior to the transaction is not a big issue because rates will rapidly accommodate the following transaction. Under a fixed rate environment, this approach might promote users to take advantage and capture all the liquidity available at current low rates. On the other hand, using an ex-post utilization to fix interest rates does not solve the problem either, as we would be overcharging costs to users. The appropriate approach to solve the problem is to make investors become indifferent to the decision of getting a loan for the total desired amount or splitting it into successive smaller loans.

To achieve that, the protocol needs to calculate the effective interest rate (2) satisfying this condition, i.e.:

![math formula - (11 & 12)](<../.gitbook/assets/image (17).png>)

![Effective interest rate in a borrow transaction](<../.gitbook/assets/image (32).png>)

#### 4.1.3 Time-Averaged Variable Rate Pool Supply

The direct use of the Variable Rate Pool supply quantity ($$SS^{t}$$) in the definition of utilization rate could expose the protocol to a type of manipulation attack. In fact, attackers can deposit to a market's Variable Rate Pool to lower the pool utilization rate to decrease the interest rate and then borrow at a cheaper rate. After that, the attackers can immediately withdraw from the Variable Rate Pool if there are enough assets available there. Although this potential manipulation does not hurt the protocol's solvency, it constitutes an unfair practice affecting users. To discourage this misbehavior, we introduce an exponential weighted moving average (EMA) of the supply in the utilization rate formula.

![math formula - (13)](<../.gitbook/assets/image (3).png>)

The idea underlying the choice of $$\alpha$$ is the following: We want the system to adapt slowly when there is an increase in the supply that carries its value above its moving average (lowering interest rates) but we want the system to adapt faster when there are withdrawals that take the supply below its average value (increasing interest rates). So we set $$\alpha$$ as:

![math formula - (14)](<../.gitbook/assets/image (38).png>)

$$\beta _{slow}$$ and $$\beta_{fast}$$ can be easily calibrated to fit a desired time decay window for each case.

### 4.2 Depositing Assets

Users can supply their assets to different Fixed Rate Pools depending on their time horizon preferences. Each new deposit generates an increase in the liquidity for that specific Fixed Rate Pool, reducing its utilization rate and the corresponding fixed interest rate for a new loan.

In economics, market clearing is the process by which the supply of whatever is traded is equated to the demand so that there is no leftover supply or demand. In the Exactly Protocol, users who are providing demand and supply of credit have access to the same information in the blockchain so there is no "friction" impending interest rate changes, thus rates will always adjust up or down to ensure market clearing.

To accomplish the market clearing condition supply interest rates are defined by the amount of interest pending payment available for distribution among Fixed Rate Pool depositors and Variable Rate Pool liquidity providers. This condition is dynamic and must hold true at all instants of time. The exact distribution among players will depend on their proportional contribution to the backing of loans.

#### 4.2.1 Supply Interest Rate

When there is a new deposit ($$D_{FR,i}^{t_{k}}$$) to a Fixed Rate Pool, the system determines the total amount of outstanding borrows backed by the Variable Rate Pool and calculates the interest pending payment. The deposit is used to return an equivalent amount of funds to the Variable Rate Pool and its corresponding interest pending payment is assigned to the new depositor. In fact, a fraction $$\delta$$ of those interests is retained by the Variable Rate Pool as a fee for its matching services. The new depositor thus gets a fraction $$(1-\delta )$$ of the original interests.

Assuming that at time $$t_{k}$$ there is a set $$\{{BV_{FR,i}^{t_{n}},n=1,\ldots N,t_{n}<t_{k}}\}$$of borrows funded by the Variable Rate Pool ($$t_{n}$$ is the time when the loan $$n$$ started). The total pending accruing interest on such borrows ($$PIBVP_{FR,i}^{t_{k}}$$) is:

![math formula - (15)](<../.gitbook/assets/image (16).png>)

Remember that $$T_{i}$$ denotes the maturity of the pool. The number of funds to be returned to the Variable Rate Pool will be

![math formula - (16)](<../.gitbook/assets/image (8) (1).png>)

and the total interest assigned to the new depositor will be

![math formula - (17)](<../.gitbook/assets/image (4).png>)

Thus, the annualized fixed interest rate on the deposit ($$RD_{FR,i}^{k,t}$$) can be calculated as follows

![math formula - (18)](<../.gitbook/assets/image (9).png>)

### 4.3 Early Withdraw

Any depositor can withdraw their assets before maturity, provided there is enough liquidity available in the protocol. Withdrawing assets implies selling the position to the Variable Rate Pool at a price equal to the deposited principal amount plus interests earned at maturity discounted at the borrowing rate prevailing at the time of withdrawal. This is equivalent to asking for a borrow in the Fixed Rate Pool for the resulting amount.

### 4.4 Early Repay

Borrowers can repay their debt before maturity and release their collateral. This implies repurchasing the debt at a price equal to de principal borrowed plus the interest owed at maturity discounted at the prevailing deposit rate at repayment time. This is equivalent to making a deposit in the Fixed Rate Pool for the resulting amount.

### 4.5 Aggregate Equations for Earned Interest on Loans in Fixed Rate Pools

Consider the time elapsed between any two operations in the protocol $$( t_{k},t_{k+1})$$. Assume there are $$l=1,2,\ldots ,L$$ coexisting Fixed Rate Pools in the period whose starting and maturing dates are $$t_{start}^{l}$$ and $$t_{mat}^{l}$$, respectively. Furthermore, assume there are $$N_{l}$$ active loans and $$M_{l}$$ deposits ($$M_{l}\leq N_{l}$$) in pool $$lth$$.

The total interest amount accumulated by the mass of loans from all the Fixed Rate Pools between $$t_{k}$$ and $$t_{k+1}$$ is given by:

![math formula - (19)](<../.gitbook/assets/image (11).png>)

The total amount of accrued interest earned by depositors is;

![math formula - (20)](<../.gitbook/assets/image (6) (2).png>)

and the total amount of accrued interest earned by the Variable Rate Pool;

![math formula - (21)](<../.gitbook/assets/image (26).png>)

## 5. Borrowing Assets at Variable Rates

Users can also take loans at variable rates similar to what they are accustomed to doing in protocols like Compound or AAVE. Under Exactly's architecture, it can be assumed that variable rate borrows take place in a special pool exclusively fed with Variable Rate Pool resources. In this pool, the only allowed operations are borrows and repayments. We use the same type of interest rate supply function as for Fixed Rate Pools.

In this case, to assure the proper behavior of the protocol, a new definition of utilization rate is needed. First, between any two transactions in the Variable Rate Pool, we define the current utilization rate as

![math formula - (22)](<../.gitbook/assets/image (14).png>)

where $$TB_{VR}^{t}$$ is the total amount of variable rate borrows outstanding at time $$t$$, and $$\tau_{VR}$$ is a numerical parameter.

Second, each time there is a new transaction in the Variable Rate Pool, a new utilization rate is calculated according to the following rule:

![math formula - (23)](<../.gitbook/assets/image (1).png>)

Here $$B_{VR}^{t+1}>0$$ means a new borrow is being made and $$B_{VR}^{t+1}<0$$ means a repay (partial or total) of an existing borrow is being made.

Defining $$U_{0}=min(U_{VR}^{t} , U_{VR}^{t+1})$$ and $$U_{1}=max(U_{VR}^{t} , U_{VR}^{t+1})$$ we can update the prevailing variable interest rate at $$t+1$$ as:

![math formula - (24 & 25)](<../.gitbook/assets/image (15).png>)

### 5.1 Repayment of a variable rate loan

To keep track of the amount that each given single borrow must repay on exit, we record the number of shares that, upon entrance, this borrow represents to the total mass of variable rate loans.

Consider a new borrow $$B_{VR}^{k,t_{k}}$$ and the total amount of outstanding debt $$TB_{VR}^{t_{k}^{-}}$$. The amount of debt-shares associated to $$B_{VR}^{k,t_{k}}$$ is given by

![math formula - (26)](<../.gitbook/assets/image (1) (1).png>)

where $$ShTB_{VR}^{t_{k}^{-}}$$ is the number of debt-shares corresponding to $$TB_{VR}^{t_{k}^{-}}$$

When the loan is canceled at a later time $$t_{k+n}$$, the amount to be repaid will be calculated as:

![math formula - (27)](<../.gitbook/assets/image (18).png>)

### 5.2 Calculating the total outstanding debt

The growth of variable rate debt is updated by calculating the interests between two consecutive transactions in the Variable Rate Pool:

![math formula - (28)](<../.gitbook/assets/image (30).png>)

So the updated debt is

![math formula - (29)](<../.gitbook/assets/image (13).png>)

### 5.3 Aggregate Equations for Interest Earned on Variable Rate Loans by the Variable Rate Pool

The total accrued interest earned by the Variable Rate Pool between $$t_{k}$$ and $$t_{k+1}$$ due to variable rate loans is given by:

![math formula - (30)](<../.gitbook/assets/image (7).png>)

## 6. Liquidations

In terms of liquidations, we adopted an approach similar to that of the Euler protocol. Consider a given user $$j$$. Let's call $$C^{j}$$ the total amount of collateral measured in USD they hold as a guarantee for their debts. Assume $$C^{j}$$ is composed of different assets.

![math formula - (31)](<../.gitbook/assets/image (12).png>)

We associate a Risk-Adjust Factor $$ho _{i}$$ to each asset in order to assess the lending power of each collateral asset. Thus the risk-adjusted collateral is given by

![math formula - (32)](<../.gitbook/assets/image (2).png>)

So, given a user collateral portfolio, the average Risk-Adjust Factor can be defined as follows

![math formula - (33)](<../.gitbook/assets/image (39).png>)

Similarly, the total amount of debt ($$D^{j}$$) and the risk-adjusted debt ($$\widetilde{D}^{j}$$) can be defined as follows

![math formula - (34)](<../.gitbook/assets/image (24).png>)

So, given asset $$k$$, the maximum amount user $$j$$ can borrow from the asset $$k$$ is:

![Max borrow](<../.gitbook/assets/image (31).png>)

The solvency condition for any given user is that their risk-adjusted collateral be greater or equal to their risk-adjusted liabilities, i.e.:

![math formula - (35)](<../.gitbook/assets/image (19).png>)

When an account becomes insolvent ($$\widetilde{C}^{j}/\widetilde{D}^{j}\leq 1$$) a liquidation process must be triggered.

In order to return the account to solvency as fast as possible and involve the least liquidation possible, we define the close factor ($$\kappa _{F}^{j}$$) as the fraction of outstanding borrows that must be repaid to return the portfolio to solvency. By design, $$\kappa_{F}^{j}$$ will be dynamic (i.e. dependent on the degree of insolvency).

Be $$\Gamma >1$$ the safe collateralization ratio that must be applied in a liquidation.

Before liquidation we have $$(\widetilde{C}^{j}/\widetilde{D}^{j} ) ^{before}\leq 1$$. Immediately after liquidation, the condition should be $$(\widetilde{C}^{j}/\widetilde{D}^{j} ) ^{after}=\Gamma$$.

In order to achieve that, liquidators repay an amount $$\kappa _{F}^{j}D^{j}$$ of debt and also pay an extra amount equal to $$\kappa_{F}^{j}\nu _{BD}D^{j}$$ to the Variable Rate Pool in concept of bad debt compensation. At the same time, they take for themselves a fraction $$\kappa_{F}^{j}(1+\nu_{BD})(1+\nu_{liq})D^{j}$$ of the collateral. The reduction in debt and collateral after liquidation is as follows

![math formula - (36 & 37)](<../.gitbook/assets/image (21).png>)

Where $$u _{liq}$$ is the liquidator commission and $$u_{BD}$$ accounts for the percentage of extra liquidation that is retained to the Variable Rate Pool as compensation for absorbing bad debt residuals after all the collateral is liquidated. Using the solvency condition

![math formula - (38)](<../.gitbook/assets/image (35).png>)

which provides the close factor as a function of the under collateralization ratio

![math formula - (39)](<../.gitbook/assets/image (37).png>)

## Acknowledgments

A very special thanks to Danilo Neves Cruz and Santiago Sánchez Ávalos for the many hours of fruitful exchange of ideas we have spent during the exciting development of this task.

## References

1. Compound (2019), [https://compound.finance/docs](https://compound.finance/docs)
2. AAVE (2020), [https://docs.aave.com](https://docs.aave.com)
3. Euler (2021), [https://www.euler.finance](https://www.euler.finance)
4. Yield (2020), [https://docs.yieldprotocol.com](https://docs.yieldprotocol.com)
5. Notional (2020), [https://docs.notional.finance](https://docs.notional.finance)
6. Element (2021), [https://paper.element.fi](https://paper.element.fi)
7. Pendle (2021), [https://docs.pendle.finance](https://docs.pendle.finance)
8. Uniswap (2018), [https://docs.uniswap.org](https://docs.uniswap.org)
9. Liquity (2020), [https://docs.liquity.org](https://docs.liquity.org)

## Model parameters

Below is the list of parameters in the model.

$$\eta$$: Fraction of total Variable Rate Pool supply selected as Liquidity Reserve.

$$R_{0}$$: Interest Rate value associated with a utilization rate equal to zero.

$$R_{b}$$: Interest Rate value associated with a utilization rate equal to $$U_{b}$$ (generally $$U_{b}=1$$).

$$\tau _{FR}$$: Fraction of Variable Rate Pool total liquidity a priori assigned to each Fixed Rate Pool.

$$\tau _{VR}$$: Fraction of Variable Rate Pool total liquidity a priori assigned to Variable Rate Pool.

$$\Lambda$$: Scale factor used to define $$U_{max}$$.

$$\alpha$$: Weighted factor in the Variable Rate Pool supply EMA.

$$\beta _{slow}$$: Time decay parameter used when supply is above average.

$$\beta _{fast}$$: Time decay parameter used when supply is below average.

$$\delta$$: Fraction of term-loan interests retained by the Variable Rate Pool upon leaving the Fixed Rate Pool.

$$\kappa _{F}$$: Close factor, a fraction of debt to be repaid by liquidators.

$$\Gamma$$: Target solvency ratio after liquidation.

$$u _{liq}$$: Liquidator incentive.

$$u _{SM}$$: Variable Rate Pool compensation for bearing bad debt.

## Disclaimer

This paper is only for general information purposes. It does not constitute investment advice or a recommendation or solicitation to buy or sell any investment instrument and should not be used in the evaluation of the merits of making any investment decision. It should not be relied upon for accounting, legal or tax advice, or investment recommendations. This paper reflects the current personal opinions of the authors and is subject to change without being updated.

## Download the Math Paper

[https://github.com/exactly/papers/blob/main/ExactlyMathPaperV1.pdf](https://github.com/exactly/papers/blob/main/ExactlyMathPaperV1.pdf)
