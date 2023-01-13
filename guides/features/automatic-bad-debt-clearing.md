# Automatic Bad Debt Clearing

## What is bad debt?

Traditional lending protocols are vulnerable to accumulating **bad debt** due to the way that liquidation mechanisms are structured. On Aave and similar platforms, a certain Loan-to-Value (**LTV**) threshold needs to be upheld, which is a ratio between the collateral a user puts up and the amount borrowed. Usually, when an account breaches a certain LTV ratio, they are liquidated by a third party (e.g., a bot) for an incentive, and the debt is made whole again so that the protocol and LPs do not bear the losses. However, situations arise when a position is not liquidated on time (due to sharp fall in prices, absence of DEX liquidity, blockchain congestion/downtime, etc.) — if this occurs and the liquidated collateral is ultimately not able to cover a position's debt, the protocol is left with **bad debt** (users' loans that are assumed that won't be repaid).

## Exactly's Solution

We implemented an improved liquidate function. This function first checks with the Auditor contract how many assets the liquidator can repay, considering the borrower's collateral and the dynamic close factor calculation. The close factor is calculated considering the shortfall of the borrower and the positive target health that the position should move to after it's liquidated to avoid cascade liquidations and not to harsh the borrower that much.

Then it proceeds to iterate over all fixed loans where the borrower borrowed, first repaying the oldest ones. If extra repayment is needed, it continues with the variable debt. After the liquidator seizes what corresponds, a call to the Auditor's **`handleBadDebt`** function is made. This function checks if the borrower doesn't have any more collateral and in case it doesn't, then it forwards the execution to the `clearBadDebt` function in each Market. This one finally deletes the debt and spreads the losses subtracting them from some general accumulated earnings.

The **`handleBadDebt`** function is external and **permissionless**. It can not only be called by a liquidation but can be triggered by anyone at any time. Even though there is no real incentive for a user to call it, it is a way to take care of the solvency of the protocol; that's why Exactly will also count on a community/team-driven bot in charge of this. As compensation for the earnings that are subtracted from the accumulator when the clearing happens, in every liquidation, the liquidator also transfers into the protocol an incentive (%) for them.
