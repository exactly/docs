# Rollover

The Rollover brings an enhanced level of flexibility and control to Exactly Protocol's users, allowing them to strategically decide when and where they want to rollover their loans.

{% embed url="https://youtu.be/knGj2uTOyos" %}

## **What Exactly can users do using rollover?** <a href="#1504" id="1504"></a>

* **Transition from uncertainty to certainty:** Users can convert any variable-rate loan into a fixed-rate loan with a specific maturity date.
* **Embrace flexibility:** Users can convert any fixed-rate loan into a variable pool, transforming their position into a variable-rate loan.
* **Opt for a Different Maturity Date:** Users can move any fixed-rate loan into another fixed-rate loan as long as the new fixed-rate pool has a different maturity date from the current one. \
  _For instance, if you hold a fixed-rate loan maturing on July 05, 2023, you can refinance this loan by shifting your debt to another fixed-rate pool maturing on September 27, 2023._

## Partial Loan Refinancing <a href="#1230" id="1230"></a>

One point to be highlighted is that our users can also choose whether they want to refinance the total amount of their loan or just a portion of it.

For instance, if you hold a 100 USD fixed-rate loan due to mature on July 05, you can decide to refinance only a fraction of the total loan amount. Let’s say you choose to rollover just 40 USDC; you can smoothly transfer this selected part to another fixed-rate pool, perhaps one with a later maturity date.

## Permit Model

This feature is possible through the [DebtManager](https://docs.exact.ly/guides/periphery/debtmanager) smart contract. For security reasons, this contract uses ['permits'](https://help.1inch.io/en/articles/5435386-permit-712-signed-token-approvals-and-how-they-work-on-1inch) for approvals related to allowing you to transfer tokens on your behalf and perform withdrawals or borrows from a Market. Permits are signatures spent when the user completes the transaction, and the approved amount is always exact. After the leverage or deleverage operation is completed, the DebtManager no longer has any allowance over tokens, withdraws or borrows.
