# Cross-Asset Leverager & Deleverager

The Leverage and Deleverage features allow users to increase or decrease positions in a single transaction. Using these strategies, users can perform several operations in the same transaction: same asset leverage, same asset deleverage, cross-asset leverage, and cross-asset deleverage, saving up to 20 transactions, depending on the market, asset, ratio, and amount they choose.

## Leverage Feature

The Leverage function allows users to increase their exposure to different assets in only one transaction without having to manually perform numerous deposits and borrows.

{% embed url="https://youtu.be/ALrdOTN05M0" %}

### Single-Asset Leverage <a href="#1871" id="1871"></a>

This feature can be used to boost the return of the user’s exposure to a specific asset in just one transaction. By selecting their desired leverage factor, the [DebtManager](https://docs.exact.ly/guides/periphery/debtmanager) smart contract will increase the user’s deposit by the chosen number (by borrowing the appropriate amount).

_For instance, if a user has 100 USDC and selects a 3.0x leverage, their deposit will increase to 300 USDC, while their borrowed amount becomes 200 USDC._

### Cross-Asset Leverage <a href="#17f1" id="17f1"></a>

The cross-asset leverage enables users to leverage their positions across different assets. Typically, users deposit one asset, such as wstETH, and borrow another, such as ETH. By leveraging this feature, users can increase their exposure to the borrowed asset or even take long or short positions on specific assets.

_E.g., if a user deposits USDC and borrows ETH, which is then sold to deposit more USDC, they effectively short ETH. If the price of ETH subsequently decreases, the user can repay their borrowed ETH with less USDC, resulting in a profit._

## Deleverage Feature <a href="#20fa" id="20fa"></a>

This feature complements the Leverage function by offering users an easy way to repay their debts using their deposited assets as collateral.

{% embed url="https://youtu.be/3sM5q2BoFUg" %}

### Single-Asset Deleverage <a href="#133d" id="133d"></a>

The same asset Deleverage feature enables users to reduce their leverage exposure on the same asset.

_For instance, if a user initially leverages a 100 USDC deposit at a 4x ratio, resulting in a deposit of 400 USDC and a borrow of 300 USDC, they can utilize the same asset deleverage to move the leverage ratio from 4x to 1x. As a result, after the deleveraging transaction, the user will end up with a 100 USDC deposit._

### Cross-Asset Deleverage <a href="#2971" id="2971"></a>

Using Cross-asset deleverage, users can reduce their leverage exposure across different assets. In addition, to adjusting the leverage ratio, users can also choose to withdraw a specific amount of funds during the deleveraging process.

_For example, if a user wants to move from a 4x to a 1x leverage ratio and withdraws their entire 100 USDC deposit, the transaction would result in zero deposits, zero borrows, and 100 USDC directly available back in their wallet._

## Permit Model

These features are possible through the [DebtManager](https://docs.exact.ly/guides/periphery/debtmanager) smart contract. For security reasons, this contract uses ['permits'](https://help.1inch.io/en/articles/5435386-permit-712-signed-token-approvals-and-how-they-work-on-1inch) for approvals related to allowing you to transfer tokens on your behalf and perform withdrawals or borrows from a Market. Permits are signatures spent when the user completes the transaction, and the approved amount is always exact. After the leverage or deleverage operation is completed, the DebtManager no longer has any allowance over tokens, withdraws or borrows.
