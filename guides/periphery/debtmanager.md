# 🌀 DebtManager

This contract allows users to handle their positions in a more flexible way. Making use of [Balancer's flash loans](https://docs.balancer.fi/reference/contracts/flash-loans.html) (currently, there are 0 fees for flash loans), it allows users to rollover their debts or leverage their positions. The DebtManager contract facilitates same-asset leverage-deleverage functions.&#x20;

This contract also uses ['permits'](https://help.1inch.io/en/articles/5435386-permit-712-signed-token-approvals-and-how-they-work-on-1inch) for approvals related to allowing you to transfer tokens on your behalf and perform withdrawals or borrows from a Market. Permits are signatures spent when the user completes the transaction, and the approved amount is always exact. After the leverage or deleverage operation is completed, the DebtManager no longer has any allowance over tokens, withdraws or borrows.

**GitHub URL:** [https://github.com/exactly/protocol/blob/main/contracts/periphery/DebtManager.sol](https://github.com/exactly/protocol/blob/main/contracts/periphery/DebtManager.sol)
