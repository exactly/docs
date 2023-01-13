# Market

The **Market** is the main contract of the protocol. It exposes all user-oriented actions for fixed and variable borrows, deposits, repayments, and withdrawals.\
The contract is also an [ERC20](https://eips.ethereum.org/EIPS/eip-20) token itself. Following the [ERC4626](erc-4626.md) standard, `exaTokens` are minted to the user once he deposits underlying assets into the variable pool. These tokens are then burned once the underlying assets are withdrawn. If transferred, the variable deposit position is also transferred.

### Public State Variables

#### accounts

```solidity
function accounts(address) external view returns (uint256 fixedDeposits, uint256 fixedBorrows, uint256 floatingBorrowShares)
```

Tracks fixed deposit and borrow map and floating borrow shares of an account.

#### asset

```solidity
function asset() external view returns (contract ERC20)
```

Address of underlying ERC20 asset.

#### auditor

```solidity
function auditor() external view returns (contract Auditor)
```

Auditor contract that validates health factor of accounts that operate with this Market.

#### backupFeeRate

```solidity
function backupFeeRate() external view returns (uint256)
```

Rate charged to the fixed pool to be retained by the floating pool for initially providing liquidity.

#### dampSpeedDown

```solidity
function dampSpeedDown() external view returns (uint256)
```

Damp speed factor to update `floatingAssetsAverage` when `floatingAssets` is lower.

#### dampSpeedUp

```solidity
function dampSpeedUp() external view returns (uint256)
```

Damp speed factor to update `floatingAssetsAverage` when `floatingAssets` is higher.

#### earningsAccumulator

```solidity
function earningsAccumulator() external view returns (uint256)
```

Accumulated earnings from extraordinary sources to be gradually distributed.

#### earningsAccumulatorSmoothFactor

```solidity
function earningsAccumulatorSmoothFactor() external view returns (uint128)
```

Factor used for gradual accrual of earnings to the floating pool.

#### fixedBorrowPositions

```solidity
function fixedBorrowPositions(uint256, address) external view returns (uint256 principal, uint256 fee)
```

Tracks account's fixed borrow positions by maturity, account and position.

#### fixedDepositPositions

```solidity
function fixedDepositPositions(uint256, address) external view returns (uint256 principal, uint256 fee)
```

Tracks account's fixed deposit positions by maturity, account and position.

#### fixedPools

```solidity
function fixedPools(uint256) external view returns (uint256 borrowed, uint256 supplied, uint256 unassignedEarnings, uint256 lastAccrual)
```

Tracks fixed pools state by maturity.

#### floatingAssets

```solidity
function floatingAssets() external view returns (uint256)
```

Amount of floating assets deposited to the pool.

#### floatingAssetsAverage

```solidity
function floatingAssetsAverage() external view returns (uint256)
```

Average of the floating assets to get fixed borrow rates and prevent rate manipulation.

#### floatingBackupBorrowed

```solidity
function floatingBackupBorrowed() external view returns (uint256)
```

Amount of assets lent by the floating pool to the fixed pools.

#### floatingDebt

```solidity
function floatingDebt() external view returns (uint256)
```

Amount of assets lent by the floating pool to accounts.

#### floatingUtilization

```solidity
function floatingUtilization() external view returns (uint256)
```

Current floating utilization used to get the new floating borrow rate.

#### interestRateModel

```solidity
function interestRateModel() external view returns (contract InterestRateModel)
```

Interest rate model contract used to get the borrow rates.

#### lastAccumulatorAccrual

```solidity
function lastAccumulatorAccrual() external view returns (uint32)
```

Last time the accumulator distributed earnings.

#### lastAverageUpdate

```solidity
function lastAverageUpdate() external view returns (uint32)
```

Last time the floating assets average was updated.

#### lastFloatingDebtUpdate

```solidity
function lastFloatingDebtUpdate() external view returns (uint32)
```

Last time the floating debt was updated.

#### maxFuturePools

```solidity
function maxFuturePools() external view returns (uint8)
```

Number of fixed pools to be active at the same time.

#### penaltyRate

```solidity
function penaltyRate() external view returns (uint256)
```

Rate per second to be charged to delayed fixed pools borrowers after maturity.

#### reserveFactor

```solidity
function reserveFactor() external view returns (uint128)
```

Percentage factor that represents the liquidity reserves that can't be borrowed.

#### treasury

```solidity
function treasury() external view returns (address)
```

Address of the treasury that will receive the allocated earnings.

#### treasuryFeeRate

```solidity
function treasuryFeeRate() external view returns (uint256)
```

Rate to be charged by the treasury to floating and fixed borrows.

#### treasury

```solidity
function treasury() external view returns (address)
```

Address of the treasury that will receive the allocated earnings.

#### treasuryFeeRate

```solidity
function treasuryFeeRate() external view returns (uint256)
```

Rate to be charged by the treasury to floating and fixed borrows.

### View Methods

#### accountSnapshot

```solidity
function accountSnapshot(address account) external view returns (uint256, uint256)
```

Gets current snapshot for an account across all maturities.

**Parameters**

| Name    | Type    | Description                                                       |
| ------- | ------- | ----------------------------------------------------------------- |
| account | address | account to return status snapshot in the specified maturity date. |

**Returns**

| Type    | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| uint256 | The amount of assets the account deposited to the floating pool     |
| uint256 | The amount of debt the account owes from fixed and floating borrows |

#### convertToAssets

```solidity
function convertToAssets(uint256 shares) external view returns (uint256)
```

Returns the amount of assets that would be exchanged by the pool for the amount of shares provided.

**Parameters**

| Name   | Type    | Description       |
| ------ | ------- | ----------------- |
| shares | uint256 | amount of shares. |

**Returns**

| Type    | Description                 |
| ------- | --------------------------- |
| uint256 | amount of exchanged assets. |

#### convertToShares

```solidity
function convertToShares(uint256 assets) external view returns (uint256)
```

Returns the amount of shares that would be exchanged by the vault for the amount of assets provided.

**Parameters**

| Name   | Type    | Description       |
| ------ | ------- | ----------------- |
| assets | uint256 | amount of assets. |

**Returns**

| Type    | Description                 |
| ------- | --------------------------- |
| uint256 | amount of exchanged shares. |

#### maxRedeem

```solidity
function maxRedeem(address owner) external view returns (uint256)
```

Returns the maximum amount of shares that can be redeem from the owner balance through a redeem call.

**Parameters**

| Name  | Type    | Description          |
| ----- | ------- | -------------------- |
| owner | address | owner of the shares. |

**Returns**

| Type    | Description                      |
| ------- | -------------------------------- |
| uint256 | max amount of redeemable shares. |

#### maxWithdraw

```solidity
function maxWithdraw(address owner) external view returns (uint256)
```

Returns the maximum amount of underlying assets that can be withdrawn from the owner balance with a single withdraw call.

**Parameters**

| Name  | Type    | Description          |
| ----- | ------- | -------------------- |
| owner | address | owner of the assets. |

**Returns**

| Type    | Description                        |
| ------- | ---------------------------------- |
| uint256 | max amount of withdrawable assets. |

#### previewBorrow

```solidity
function previewBorrow(uint256 assets) external view returns (uint256)
```

Simulates the effects of a borrow at the current time, given current contract conditions.

**Parameters**

| Name   | Type    | Description                 |
| ------ | ------- | --------------------------- |
| assets | uint256 | amount of assets to borrow. |

**Returns**

| Type    | Description                                                             |
| ------- | ----------------------------------------------------------------------- |
| uint256 | amount of shares that will be assigned to the account after the borrow. |

#### previewDebt

```solidity
function previewDebt(address borrower) external view returns (uint256 debt)
```

Gets all borrows and penalties for an account.

**Parameters**

| Name     | Type    | Description                                                       |
| -------- | ------- | ----------------------------------------------------------------- |
| borrower | address | account to return status snapshot for fixed and floating borrows. |

**Returns**

| Name | Type    | Description                                      |
| ---- | ------- | ------------------------------------------------ |
| debt | uint256 | the total debt, denominated in number of assets. |

#### previewDeposit

```solidity
function previewDeposit(uint256 assets) external view returns (uint256)
```

Allows users to simulate the effects of their deposit at the current block.

**Parameters**

| Name   | Type    | Description             |
| ------ | ------- | ----------------------- |
| assets | uint256 | assets to be deposited. |

**Returns**

| Type    | Description                                        |
| ------- | -------------------------------------------------- |
| uint256 | shares to receive in exchange of deposited assets. |

#### previewFloatingAssetsAverage

```solidity
function previewFloatingAssetsAverage() external view returns (uint256)
```

Gets the current `floatingAssetsAverage` without updating the storage variable.

**Returns**

| Type    | Description                        |
| ------- | ---------------------------------- |
| uint256 | projected `floatingAssetsAverage`. |

#### previewMint

```solidity
function previewMint(uint256 shares) external view returns (uint256)
```

Allows users to simulate the effects of their mint at the current block.

**Parameters**

| Name   | Type    | Description          |
| ------ | ------- | -------------------- |
| shares | uint256 | shares to be minted. |

**Returns**

| Type    | Description                                     |
| ------- | ----------------------------------------------- |
| uint256 | assets to deposit in exchange of minted shares. |

#### previewRedeem

```solidity
function previewRedeem(uint256 shares) external view returns (uint256)
```

Allows users to simulate the effects of their redemption at the current block.

**Parameters**

| Name   | Type    | Description          |
| ------ | ------- | -------------------- |
| shares | uint256 | shares to be redeem. |

**Returns**

| Type    | Description                                     |
| ------- | ----------------------------------------------- |
| uint256 | assets to receive in exchange of burned shares. |

#### previewRefund

```solidity
function previewRefund(uint256 shares) external view returns (uint256)
```

Simulates the effects of a refund at the current time, given current contract conditions.

**Parameters**

| Name   | Type    | Description                                                |
| ------ | ------- | ---------------------------------------------------------- |
| shares | uint256 | amount of shares to subtract from caller's accountability. |

**Returns**

| Type    | Description                           |
| ------- | ------------------------------------- |
| uint256 | amount of assets that will be repaid. |

#### previewRepay

```solidity
function previewRepay(uint256 assets) external view returns (uint256)
```

Simulates the effects of a repay at the current time, given current contract conditions.

**Parameters**

| Name   | Type    | Description                |
| ------ | ------- | -------------------------- |
| assets | uint256 | amount of assets to repay. |

**Returns**

| Type    | Description                                                                |
| ------- | -------------------------------------------------------------------------- |
| uint256 | amount of shares that will be subtracted from the account after the repay. |

#### previewWithdraw

```solidity
function previewWithdraw(uint256 assets) external view returns (uint256)
```

Allows users to simulate the effects of their withdrawal at the current block.

**Parameters**

| Name   | Type    | Description             |
| ------ | ------- | ----------------------- |
| assets | uint256 | assets to be withdrawn. |

**Returns**

| Type    | Description                                    |
| ------- | ---------------------------------------------- |
| uint256 | burned shares in exchange of withdrawn assets. |

#### totalAssets

```solidity
function totalAssets() external view returns (uint256)
```

Calculates the floating pool balance plus earnings to be accrued at current timestamp from maturities and accumulator.

**Returns**

| Type    | Description                                                             |
| ------- | ----------------------------------------------------------------------- |
| uint256 | actual floatingAssets plus earnings to be accrued at current timestamp. |

#### totalFloatingBorrowAssets

```solidity
function totalFloatingBorrowAssets() external view returns (uint256)
```

Calculates the total floating debt, considering elapsed time since last update and current interest rate.

**Returns**

| Type    | Description                                   |
| ------- | --------------------------------------------- |
| uint256 | actual floating debt plus projected interest. |

#### totalFloatingBorrowShares

```solidity
function totalFloatingBorrowShares() external view returns (uint256)
```

Total amount of floating borrow shares assigned to floating borrow accounts.

### Write Methods

#### borrow

```solidity
function borrow(uint256 assets, address receiver, address borrower) external nonpayable returns (uint256 borrowShares)
```

Borrows a certain amount from the floating pool.

**Parameters**

| Name     | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| assets   | uint256 | amount to be sent to receiver and repaid by borrower. |
| receiver | address | address that will receive the borrowed assets.        |
| borrower | address | address that will repay the borrowed assets.          |

**Returns**

| Name         | Type    | Description                                  |
| ------------ | ------- | -------------------------------------------- |
| borrowShares | uint256 | shares corresponding to the borrowed assets. |

#### borrowAtMaturity

```solidity
function borrowAtMaturity(uint256 maturity, uint256 assets, uint256 maxAssets, address receiver, address borrower) external nonpayable returns (uint256 assetsOwed)
```

Borrows a certain amount from a maturity.

**Parameters**

| Name      | Type    | Description                                                   |
| --------- | ------- | ------------------------------------------------------------- |
| maturity  | uint256 | maturity date for repayment.                                  |
| assets    | uint256 | amount to be sent to receiver and repaid by borrower.         |
| maxAssets | uint256 | maximum amount of debt that the account is willing to accept. |
| receiver  | address | address that will receive the borrowed assets.                |
| borrower  | address | address that will repay the borrowed assets.                  |

**Returns**

| Name       | Type    | Description                                                        |
| ---------- | ------- | ------------------------------------------------------------------ |
| assetsOwed | uint256 | total amount of assets (principal + fee) to be repaid at maturity. |

#### clearBadDebt

```solidity
function clearBadDebt(address borrower) external nonpayable
```

Clears floating and fixed debt for an account spreading the losses to the `earningsAccumulator`.

_Can only be called from the auditor._

**Parameters**

| Name     | Type    | Description                                                  |
| -------- | ------- | ------------------------------------------------------------ |
| borrower | address | account with insufficient collateral to be cleared the debt. |

#### deposit

```solidity
function deposit(uint256 assets, address receiver) external nonpayable returns (uint256 shares)
```

**Parameters**

| Name     | Type    | Description                                                 |
| -------- | ------- | ----------------------------------------------------------- |
| assets   | uint256 | amount of assets to deposit.                                |
| receiver | address | address of the account that will receive the minted shares. |

**Returns**

| Name   | Type    | Description              |
| ------ | ------- | ------------------------ |
| shares | uint256 | amount of minted shares. |

#### depositAtMaturity

```solidity
function depositAtMaturity(uint256 maturity, uint256 assets, uint256 minAssetsRequired, address receiver) external nonpayable returns (uint256 positionAssets)
```

Deposits a certain amount to a maturity.

**Parameters**

| Name              | Type    | Description                                                                            |
| ----------------- | ------- | -------------------------------------------------------------------------------------- |
| maturity          | uint256 | maturity date where the assets will be deposited.                                      |
| assets            | uint256 | amount to receive from the msg.sender.                                                 |
| minAssetsRequired | uint256 | minimum amount of assets required by the depositor for the transaction to be accepted. |
| receiver          | address | address that will be able to withdraw the deposited assets.                            |

**Returns**

| Name           | Type    | Description                                                           |
| -------------- | ------- | --------------------------------------------------------------------- |
| positionAssets | uint256 | total amount of assets (principal + fee) to be withdrawn at maturity. |

#### liquidate

```solidity
function liquidate(address borrower, uint256 maxAssets, contract Market seizeMarket) external nonpayable returns (uint256 repaidAssets)
```

Liquidates undercollateralized position(s).

_Msg.sender liquidates borrower's position(s) and repays a certain amount of debt for multiple maturities, seizing a part of borrower's collateral._

**Parameters**

| Name        | Type            | Description                                                                       |
| ----------- | --------------- | --------------------------------------------------------------------------------- |
| borrower    | address         | wallet that has an outstanding debt across all maturities.                        |
| maxAssets   | uint256         | maximum amount of debt that the liquidator is willing to accept. (it can be less) |
| seizeMarket | contract Market | market from which the collateral will be seized to give the liquidator.           |

**Returns**

| Name         | Type    | Description           |
| ------------ | ------- | --------------------- |
| repaidAssets | uint256 | actual amount repaid. |

#### mint

```solidity
function mint(uint256 shares, address receiver) external nonpayable returns (uint256 assets)
```

**Parameters**

| Name     | Type    | Description                                             |
| -------- | ------- | ------------------------------------------------------- |
| shares   | uint256 | amount of shares to mint.                               |
| receiver | address | address of account that will receive the minted shares. |

**Returns**

| Name   | Type    | Description                 |
| ------ | ------- | --------------------------- |
| assets | uint256 | amount of deposited assets. |

#### redeem

```solidity
function redeem(uint256 shares, address receiver, address owner) external nonpayable returns (uint256 assets)
```

Redeems the owner's floating pool assets to the receiver address.

_Makes sure that the owner doesn't have shortfall after withdrawing._

**Parameters**

| Name     | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| shares   | uint256 | amount of shares to be redeemed for underlying asset. |
| receiver | address | address to which the assets will be transferred.      |
| owner    | address | address which owns the floating pool assets.          |

**Returns**

| Name   | Type    | Description                                    |
| ------ | ------- | ---------------------------------------------- |
| assets | uint256 | amount of underlying asset that was withdrawn. |

#### refund

```solidity
function refund(uint256 borrowShares, address borrower) external nonpayable returns (uint256 assets, uint256 actualShares)
```

Repays a certain amount of shares to the floating pool.

**Parameters**

| Name         | Type    | Description                                                 |
| ------------ | ------- | ----------------------------------------------------------- |
| borrowShares | uint256 | shares to be subtracted from the borrower's accountability. |
| borrower     | address | address of the account that has the debt.                   |

**Returns**

| Name         | Type    | Description                                                  |
| ------------ | ------- | ------------------------------------------------------------ |
| assets       | uint256 | subtracted assets from the borrower's accountability.        |
| actualShares | uint256 | actual subtracted shares from the borrower's accountability. |

#### repay

```solidity
function repay(uint256 assets, address borrower) external nonpayable returns (uint256 actualRepay, uint256 borrowShares)
```

Repays a certain amount of assets to the floating pool.

**Parameters**

| Name     | Type    | Description                                                 |
| -------- | ------- | ----------------------------------------------------------- |
| assets   | uint256 | assets to be subtracted from the borrower's accountability. |
| borrower | address | address of the account that has the debt.                   |

**Returns**

| Name         | Type    | Description                                                     |
| ------------ | ------- | --------------------------------------------------------------- |
| actualRepay  | uint256 | the actual amount that should be transferred into the protocol. |
| borrowShares | uint256 | subtracted shares from the borrower's accountability.           |

#### repayAtMaturity

```solidity
function repayAtMaturity(uint256 maturity, uint256 positionAssets, uint256 maxAssets, address borrower) external nonpayable returns (uint256 actualRepayAssets)
```

Repays a certain amount to a maturity.

**Parameters**

| Name           | Type    | Description                                                                |
| -------------- | ------- | -------------------------------------------------------------------------- |
| maturity       | uint256 | maturity date where the assets will be repaid.                             |
| positionAssets | uint256 | amount to be paid for the borrower's debt.                                 |
| maxAssets      | uint256 | maximum amount of debt that the account is willing to accept to be repaid. |
| borrower       | address | address of the account that has the debt.                                  |

**Returns**

| Name              | Type    | Description                                               |
| ----------------- | ------- | --------------------------------------------------------- |
| actualRepayAssets | uint256 | the actual amount that was transferred into the protocol. |

#### seize

```solidity
function seize(address liquidator, address borrower, uint256 assets) external nonpayable
```

Public function to seize a certain amount of assets.

_Public function for liquidator to seize borrowers assets in the floating pool. This function will only be called from another Market, on `liquidation` calls. That's why msg.sender needs to be passed to the private function (to be validated as a market)_

**Parameters**

| Name       | Type    | Description                                      |
| ---------- | ------- | ------------------------------------------------ |
| liquidator | address | address which will receive the seized assets.    |
| borrower   | address | address from which the assets will be seized.    |
| assets     | uint256 | amount to be removed from borrower's possession. |

#### withdraw

```solidity
function withdraw(uint256 assets, address receiver, address owner) external nonpayable returns (uint256 shares)
```

Withdraws the owner's floating pool assets to the receiver address.

_Makes sure that the owner doesn't have shortfall after withdrawing._

**Parameters**

| Name     | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| assets   | uint256 | amount of underlying to be withdrawn.            |
| receiver | address | address to which the assets will be transferred. |
| owner    | address | address which owns the floating pool assets.     |

**Returns**

| Name   | Type    | Description                                     |
| ------ | ------- | ----------------------------------------------- |
| shares | uint256 | amount of shares redeemed for underlying asset. |

#### withdrawAtMaturity

```solidity
function withdrawAtMaturity(uint256 maturity, uint256 positionAssets, uint256 minAssetsRequired, address receiver, address owner) external nonpayable returns (uint256 assetsDiscounted)
```

Withdraws a certain amount from a maturity.

_It's expected that this function can't be paused to prevent freezing account funds._

**Parameters**

| Name              | Type    | Description                                                                         |
| ----------------- | ------- | ----------------------------------------------------------------------------------- |
| maturity          | uint256 | maturity date where the assets will be withdrawn.                                   |
| positionAssets    | uint256 | position size to be reduced.                                                        |
| minAssetsRequired | uint256 | minimum amount required by the account (if discount included for early withdrawal). |
| receiver          | address | address that will receive the withdrawn assets.                                     |
| owner             | address | address that previously deposited the assets.                                       |

**Returns**

| Name             | Type    | Description                                                             |
| ---------------- | ------- | ----------------------------------------------------------------------- |
| assetsDiscounted | uint256 | amount of assets withdrawn (can include a discount for early withdraw). |
