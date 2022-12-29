# 📥 Deposits

Any depositor can supply assets to the pool at any time during the life of a Fixed Rate Pool. No deposits can be made once the fixed pool matures (after maturity date).

When there is a new deposit to a Fixed Rate Pool, the system determines the total amount of outstanding borrows backed by the Variable Rate Pool and calculates the interest pending payment. The deposit returns an equivalent amount of funds to the Variable Rate Pool, and its corresponding interest pending payment is assigned to the new depositor.

Each fixed deposit is composed of two different amounts, **principal** and **earnings**. The principal is the initial amount deposited, whereas the earnings are the extra interest the depositor will acquire after maturity.

It's important to highlight that fixed-rate deposits **can't** be used as collateral to borrow other assets.

### Early Withdrawals

Depositors can withdraw anytime, even before maturity, provided enough liquidity is available in the protocol. However, early withdrawals will enquire in a penalty for the user who it's trying to withdraw because it's equivalent to asking for a borrow for the resulting amount.

_Given Bob deposits 100 to a fixed pool with an interest rate to maturity of 10%, then his position is equal to 110._

_Now let's say that the rate for borrowing 110 is 5%; Bob could decide to withdraw his whole position (110) before maturity, and he will get a \~5.3 penalty since his position of 110 is divided by 1.05 (100% + 5% of borrow rate)._

_assets discounted = 110 / 1.05 \~= 104.76..._

_Finally, Bob will receive 104.76 when trying to withdraw 110 early._

### Late Withdrawals

Depositors can also withdraw once the maturity date is reached.

There's no limit to the time they have to withdraw their funds but bear in mind that these deposited assets will not generate any extra yield.
