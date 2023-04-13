# 📥 Deposits

Any depositor can supply assets to the pool at any time during the life of a Fixed Rate Pool. No deposits can be made once the fixed pool matures (after the maturity date).

When there is a new deposit to a Fixed Rate Pool, the system determines the total amount of outstanding borrows backed by the Variable Rate Pool and calculates the interest pending payment. The deposit returns an equivalent amount of funds to the Variable Rate Pool, and its corresponding interest pending payment is assigned to the new depositor.

Each fixed deposit is composed of two different amounts, **principal** and **earnings**. The principal is the initial amount deposited, whereas the earnings are the extra interest the depositor will acquire after maturity.

It's important to highlight that **fixed-rate deposits can't be used as collateral to borrow other assets.**

## Early Withdrawals

Depositors can withdraw anytime, even before maturity, provided enough liquidity is available in the protocol. So, early withdrawals are equivalent to requesting a fixed rate borrow for the total deposit (principal+earnings) in the same fixed rate pool.

_For example, Alice deposits $100 into a 1-year fixed-rate pool with an interest rate of 10% APR. Then her total deposit at maturity equals $110 ($100+$10)._

_Now let's say that a few hours later, the utilization of the pools goes down, and the 1-year fixed rate for borrowing $110 is also 10% APR; Alice could decide to make an early withdrawal of her fixed rate deposit, and she will get back now the present value of her total deposit at maturity ($110) discounted by the current fixed borrow rate (10% APR), in this case, equal to the original $100 that she deposited ($110/1.1)._

## Late Withdrawals

Depositors can also withdraw once the maturity date is reached.

There's no limit to the time they have to withdraw their funds but bear in mind that these deposited assets will not generate any extra interest rate fees.
