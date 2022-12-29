# Dynamic Close Factor

The fraction of a borrower's debt that liquidators can pay off in one go is referred to by Compound as the 'close factor.' Compound and Aave's close factor is currently fixed at `0.5`, meaning liquidators can pay off up to half a borrower's loan in one go, regardless of how underwater their position is. This approach has a couple of potential drawbacks.

First, allowing liquidators to liquidate half a loan could be considered excessive if a smaller liquidation would have been sufficient to bring the borrower back to health. Larger borrowers are likely to be put off by such a process. Second, a sizeable fixed discount can sometimes drive borrowers closer to insolvency and disincentivize them from repaying their loans.

Therefore, we use a dynamic close factor on Exactly to \`soft liquidate' borrowers. Specifically, we allow liquidators to repay up to the amount needed to bring a borrower with a shortfall back out of violation (plus an additional safety factor). This means that borrowers who are only slightly in violation will often have much less than half their debts repaid during a liquidation, whilst borrowers who are heavily in violation will often have much more than half their debts repaid during a liquidation (their whole position might be closed in some circumstances).
