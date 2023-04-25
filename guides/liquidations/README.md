# ♻ Liquidations

The health of Exactly's Protocol depends on the 'health' of the loans within the system, also known as the 'health factor'. When the [health factor](../price-feeds.md#health-factor) of an account's total loans is below `1`, anyone can make a `liquidate()` call to the [Market](../protocol/market/) contract, paying back part of the debt owed, and receiving discounted collateral in return (also known as the liquidation bonus as [previously seen here](../parameters.md#h.-liquidation-bonuses)).

This incentivizes third parties to participate in the solvency of the overall protocol by acting in their interest (to receive the discounted collateral) and, as a result, ensure loans are sufficiently collateralized.

## Instructions

In order to know whether an account can be liquidated or not, the `accountLiquidity` function in the [Auditor](../protocol/auditor.md) contract can be called.

As for the `marketToSimulate` parameter, the [address zero](https://etherscan.io/address/0x0000000000000000000000000000000000000000) should be passed, and zero (`0`) should also be sent as `withdrawAmount`.

### accountLiquidity

```solidity
function accountLiquidity(address account, contract Market marketToSimulate, uint256 withdrawAmount) external view returns (uint256 sumCollateral, uint256 sumDebtPlusEffects)
```

**Parameters**

| Name             | Type            | Description                                        |
| ---------------- | --------------- | -------------------------------------------------- |
| account          | address         | account in which the liquidity will be calculated. |
| marketToSimulate | contract Market | market in which to simulate withdraw operation.    |
| withdrawAmount   | uint256         | amount to simulate as withdraw.                    |

**Returns**

| Name               | Type    | Description                                                                            |
| ------------------ | ------- | -------------------------------------------------------------------------------------- |
| sumCollateral      | uint256 | sum of all collateral, already multiplied by each adjust factor (denominated in base). |
| sumDebtPlusEffects | uint256 | sum of all debt divided by adjust factor considering withdrawal (denominated in base). |

This function will return the total adjusted collateral and debt of the account. Then the collateral can be divided by the debt to get the health factor.

If its value is lower than `1`, then the liquidate function will not revert when being called for that user.

Other more efficient ways of checking for account positions require monitoring block-to-block transactions. This is the approach followed by our [open-source and team-driven bot](exactlys-bot.md).

Once there's an account opened to be liquidated, the `liquidate` function can be called to successfully repay the debt of that user and seize a portion of its collateral with a discount.

The function expects and returns the following arguments:

### liquidate

```solidity
function liquidate(address borrower, uint256 maxAssets, contract Market seizeMarket) external nonpayable returns (uint256 repaidAssets)
```

**Parameters**

| Name        | Type            | Description                                                                       |
| ----------- | --------------- | --------------------------------------------------------------------------------- |
| borrower    | address         | wallet that has an outstanding debt across fixed and variable pools.              |
| maxAssets   | uint256         | maximum amount of debt that the liquidator is willing to accept. (it can be less) |
| seizeMarket | contract Market | market from which the collateral will be seized to give the liquidator.           |

**Returns**

| Name         | Type    | Description           |
| ------------ | ------- | --------------------- |
| repaidAssets | uint256 | actual amount repaid. |

It's important to highlight that `maxAssets` should also include an [extra bonus](../parameters.md#h.-liquidation-bonuses) that the variable pool of the Market will keep that the debt is being repaid.

## Health Factor

The health factor is calculated from the user's collateral balance (in `ETH`) multiplied by each asset's adjust factor divided by the user's debt which is also divided by this adjust factor.

For example: given an `ETH` adjusted factor of `0.84`, a deposit of `100 ETH` and borrow of `50 ETH`:

The adjusted collateral will be equal to `100*0.84 = 84.00`

The adjusted debt will be equal to `50/0.84 = 59.52`&#x20;

So the health factor will be equal to `84.00/59.52 = 1.41`&#x20;

Below a health factor of `1`, the user will be considered with shortfall and open to [liquidation](../../getting-started/math-paper.md#6.-liquidations).
