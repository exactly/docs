# Earnings Accumulator

The yield to be accrued over time by variable pool suppliers comes from different sources of income. We can identify them between two different origins: **ordinary** and **extraordinary**.

### Ordinary

Ordinary earnings are immediately distributed to variable pool suppliers as soon as they are charged to borrowers. Every time the **variable borrowers**' debt increases, this newly assigned debt is directly distributed between current variable pool suppliers.

The same approach is used when distributing earnings that originate from fixed borrows that are being backed up by the variable pool. As previously explained in the [white paper](../../getting-started/white-paper.md#2.4-borrowing-assets-from-the-fixed-rate-pools), while the variable pool is lending assets to **fixed borrowers**, the fee charged to these borrowers is periodically accrued by the variable pool until a fixed supplier decides to deposit into this fixed pool and match the demand.

### Extraordinary

Extraordinary earnings are considered earnings that are accounted for in several mechanisms that Exactly count on to maintain the solvency and attractiveness of the protocol. Together with the ordinary earnings, these should be distributed between variable pool suppliers, **but** they may arise in large quantities in single operations.

So, to avoid opening possible MEV profits for bots or external actors to sandwich these operations by depositing to the pool with a significant amount of assets to acquire a more considerable proportion and thus earning profits for then instantly withdrawing, we've come up with an **earnings accumulator**.

This accumulator will hold earnings that come from extraordinary sources and will gradually and smoothly distribute these earnings to the pool using a distribution factor. Then incentivizing users to keep lending their liquidity while disincentivizing atomic bots that might look to profit from Exactly unfairly.

The extraordinary sources include:

#### 1. Variable Pool (Back Up) Fee

It's the fee charged to the fixed depositors once they supply assets to a fixed rate pool that the variable pool suppliers will retain for initially providing liquidity.

#### 2. Late Fixed Repayment Penalties

All penalties charged to fixed borrowers that repay **after** the corresponding maturity date.

#### 3. Free Lunch

In finance, a 'free lunch' refers to a situation where there is no cost incurred by the individual receiving the goods or services being provided.

The rate offered to a fixed supplier is directly obtained through the number of fees that were charged to previously fixed borrowers. In this way, it's known that these fixed earnings are **limited**.

That's why there's always a maximum efficient amount for fixed deposits, and over that amount, any excess will be unnecessarily locked from a supplier's perspective. Users will be warned through our web app, but there are no restrictions at a smart contract level. The protocol should have as few prohibitions and caps as possible to help with our idea of a free and permissionless market/protocol.

If this is the case, the following fixed borrower that borrows from the fixed pool where the inefficient deposit lies will borrow from this amount of assets instead of using the backup suppliers' (variable pool). Consequently, the fees charged to that borrow will be considered a 'free lunch' since they originated from a borrow that used inefficient and locked assets.

**4. Variable Pool Bonus in Liquidations**

The earnings accumulator will be used to spread the losses caused by any [bad debt clearing](automatic-bad-debt-clearing.md).

To counter these losses, a percentage of the seized collateral is added to the accumulator in **every** liquidation.
