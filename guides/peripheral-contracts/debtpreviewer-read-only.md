# 👓 DebtPreviewer (read-only)

This is a smart contract designed to abstract the `DebtManager` logic, it provides read-only functions ready to be consumed by Exactly’s web app.

**This contract has mainly 2 purposes:**

* Retrieve information about any account collateral and debt for any pair of markets in exactly.
* Allow accounts to preview their positions, simulating a leverage or deleverage to a certain ratio with a desired health factor.

Please be advised that the  `DebtPreviewer` contract has not undergone a formal security audit. As such, we strongly discourage integrations involving this contract, since it is exclusively intended for read-only purposes by the [Exactly Web App](https://app.exact.ly/). Using unaudited contracts for any other purpose may expose users to potential security risks or other unexpected behavior.

**GitHub URL:** [https://github.com/exactly/protocol/blob/main/contracts/periphery/DebtPreviewer.sol](https://github.com/exactly/protocol/blob/main/contracts/periphery/DebtPreviewer.sol)
