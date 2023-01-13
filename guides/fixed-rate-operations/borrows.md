# 📤 Borrows

Any borrower can ask for a loan at any time during the life of a Fixed Rate Pool, provided they have [enough collateral](../../getting-started/math-paper.md#4.1.-borrowing-assets-at-fixed-rates). No borrows can be made once the fixed pool matures (after the maturity date).

The interest rate to be applied to each loan is determined by an interest rate demand function that depends on the utilization rate on that specific Fixed Rate Pool. This utilization comprises two types of funding: deposits in the same Fixed Rate Pool and deposits in the Variable Rate Pool that will initially back up the borrow.

Each fixed borrow is composed of two different amounts, **principal** and **fees**. The principal is the initial amount borrowed, whereas the fee is the interest that the borrower will need to repay on top of the borrowed amount when repaying at maturity time.

## Early Repayments

Borrowers are allowed to repay at any time, even before the maturity date. The advantage of repaying early is that they can get a discount considering the utilization of the pool at the current time since repaying early is equivalent to making a fixed deposit.

_If Bob borrows 100 from a fixed pool with an interest rate to maturity of 10%, then his debt equals 110._

_Now let's say that the rate for depositing 100 is 5%; Bob could decide to repay his whole debt (110) before maturity, and he will get a 5 discount since only his principal (100) is taken into account when early repaying._

_Finally, 105 assets will already cover his borrowing of 110._

In this way, if the utilization of a pool is high, then borrowers are also encouraged and incentivized to bring back liquidity soon.

## Late Repayments

Borrowers can also repay after the maturity date, but in this case, a penalty is charged for every delayed second that went by since the maturity date.

The [penalty rate](../parameters.md#j.-penalty-rate) is charged to the whole debt (principal & fees).

_If Bob makes a fixed borrow of 100 with an interest rate to maturity of 10%, then his debt equals 110._

_Now Bob has been delayed 2 days since the maturity date. If the daily penalty rate is equal to 2%, then 4% in penalties will be added to his debt._

_Finally, 114.4 (110 + 4.4) will cover his borrow of 110._

It's worth highlighting that any penalties increase will also account as debt for the health factor calculation of the user's positions.
