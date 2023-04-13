# 📤 Borrows

Any borrower can ask for a loan at any time during the life of a Fixed Rate Pool, provided they have [enough collateral](../../getting-started/math-paper.md#4.1.-borrowing-assets-at-fixed-rates) in the Variable Rate Pool. No new borrows can be made after the maturity date of the Fixed Rate Pool.

The interest rate to be applied to each loan is determined by an interest rate demand function that depends on the utilization rate on that specific Fixed Rate Pool at that moment. This utilization comprises two types of funding: deposits in the same Fixed Rate Pool and deposits in the Variable Rate Pool that will initially back up the borrow.

Each fixed borrower comprises two different amounts, **principal** and **interest rate** **fees**. The principal is the initial amount borrowed, whereas the fee is the interest the borrower will need to repay on top of the borrowed amount when repaying at maturity time.

## Early Repayments

Borrowers are allowed to repay at any time, even before the maturity date. The advantage of repaying early is that they can get a discount considering the utilization of the pool at the current time since repaying early is equivalent to making a fixed deposit.

_For example: If Bob borrows $100 from a 1 year fixed pool with a fixed interest rate of 5% APR, then his debt equals $105 at maturity._

_Now let's say that a few hours later, the fixed rate for depositing $100 (in the same fixed rate pool) increases to 5% APR; Bob could decide to repay his whole debt ($105) before maturity without any extra cost since in this example the present value of its total debt discounted at new fixed deposit rate ($105/1.05) is equal to his new deposit ($100)._

In this way, if the utilization of a pool gets higher, the new fixed deposit rate will be higher, and the fixed-rate borrowers are incentivized to bring back liquidity before the maturity date.

## Late Repayments

Borrowers can also repay after the maturity date, but in this case, a penalty is charged for every delayed second that went by since the maturity date.

The [penalty rate](../parameters.md#j.-penalty-rate) is charged to the whole debt (principal and fixed interest fees).

_For example, Bob makes a fixed borrow for 3 months and has a total debt at maturity of $100 ($97 of his initial loan + $3 in fixed interest rate fees). Then Bob delayed his payment by 10 days since the maturity date. If the daily penalty rate equals 0.45%, then $4.5 in penalties, so his new total debt will be $104.5 ($97 + $3 + $4.5)_

It's worth highlighting that any penalties increase will also account as debt for the health factor calculation of the user's positions.

## Preventing Manipulation

The protocol takes several measures to prevent manipulation of the fixed borrow rate. One of these measures is calculating the utilization rate for fixed borrows by taking an average of the variable pool deposits, known as `floatingAssetsAverage`, and passing it to the [InterestRateModel](../protocol/interestratemodel.md). Without this method, a user could deposit a large sum into the variable pool, lower the utilization rate, and then request a low fixed borrow before withdrawing their initial deposit. This is a potential flash loan attack scenario, but the `floatingAssetsAverage` is updated with a damp speed algorithm to adjust the value of the actual variable pool deposits gradually (`floatingAssets`) over a short period of time. This ensures that even if a user deposits to the variable pool and requests a fixed borrow in the same transaction, the average will still account for the outdated value. More details about this can be found in the [Math Paper](../../getting-started/math-paper.md#4.1.3-time-averaged-variable-rate-pool-supply) section.

Another way the protocol prevents manipulation is through the `maxSlippage` mechanism. This feature allows users to specify the maximum deviation in the rate they are willing to accept when requesting a fixed borrow, limiting the opportunity for malicious actors to front run a fixed borrow operation.
