# MarketETHRouter

To be used by Exactly’s web-app, so accounts can operate with `ETH` on **MarketWETH**.\
\
Wraps `ETH` or unwraps `WETH` before and after interacting with **MarketWETH**. It saves one step for the user.

## Public State Variables

### market

```solidity
function market() external view returns (contract Market)
```

### weth

```solidity
function weth() external view returns (contract WETH
```

## Write Methods

### borrow

```solidity
function borrow(uint256 assets) external nonpayable returns (uint256 borrowShares)
```

Unwraps WETH from the floating pool and borrows to caller.

**Parameters**

| Name   | Type    | Description                 |
| ------ | ------- | --------------------------- |
| assets | uint256 | amount of assets to borrow. |

**Returns**

| Name         | Type    | Description                |
| ------------ | ------- | -------------------------- |
| borrowShares | uint256 | number of borrowed shares. |

### borrowAtMaturity

```solidity
function borrowAtMaturity(uint256 maturity, uint256 assets, uint256 maxAssetsAllowed) external nonpayable returns (uint256 assetsOwed)
```

Unwraps WETH from a maturity and borrows to caller.

**Parameters**

| Name             | Type    | Description                                                  |
| ---------------- | ------- | ------------------------------------------------------------ |
| maturity         | uint256 | maturity date for repayment.                                 |
| assets           | uint256 | amount to be sent to caller.                                 |
| maxAssetsAllowed | uint256 | maximum amount of debt that the caller is willing to accept. |

**Returns**

| Name       | Type    | Description                                                        |
| ---------- | ------- | ------------------------------------------------------------------ |
| assetsOwed | uint256 | total amount of assets (principal + fee) to be repaid at maturity. |

### deposit

```solidity
function deposit() external payable returns (uint256 shares)
```

Wraps ETH and deposits WETH into the floating pool's market.

**Returns**

| Name   | Type    | Description              |
| ------ | ------- | ------------------------ |
| shares | uint256 | number of minted shares. |

### depositAtMaturity

```solidity
function depositAtMaturity(uint256 maturity, uint256 minAssetsRequired) external payable returns (uint256 maturityAssets)
```

Wraps ETH and deposits to a maturity.

**Parameters**

| Name              | Type    | Description                                                                         |
| ----------------- | ------- | ----------------------------------------------------------------------------------- |
| maturity          | uint256 | maturity date where the assets will be deposited.                                   |
| minAssetsRequired | uint256 | minimum amount of assets required by the caller for the transaction to be accepted. |

**Returns**

| Name           | Type    | Description                                                           |
| -------------- | ------- | --------------------------------------------------------------------- |
| maturityAssets | uint256 | total amount of assets (principal + fee) to be withdrawn at maturity. |

### redeem

```solidity
function redeem(uint256 shares) external nonpayable returns (uint256 assets)
```

Unwraps WETH from the floating pool and withdraws to caller.

**Parameters**

| Name   | Type    | Description                                          |
| ------ | ------- | ---------------------------------------------------- |
| shares | uint256 | amount of shares to be burned in exchange of assets. |

**Returns**

| Name   | Type    | Description                 |
| ------ | ------- | --------------------------- |
| assets | uint256 | amount of assets withdrawn. |

### refund

```solidity
function refund(uint256 borrowShares) external payable returns (uint256 repaidAssets, uint256 actualShares)
```

Wraps ETH and repays to the floating pool.

**Parameters**

| Name         | Type    | Description                                     |
| ------------ | ------- | ----------------------------------------------- |
| borrowShares | uint256 | shares to be subtracted from the caller's debt. |

**Returns**

| Name         | Type    | Description                                                                            |
| ------------ | ------- | -------------------------------------------------------------------------------------- |
| repaidAssets | uint256 | number of repaid assets.                                                               |
| actualShares | uint256 | number of borrowed shares subtracted from the debt (can be lower than `borrowShares`). |

### repay

```solidity
function repay(uint256 assets) external payable returns (uint256 repaidAssets, uint256 borrowShares)
```

Wraps ETH and repays to the floating pool.

**Parameters**

| Name   | Type    | Description                |
| ------ | ------- | -------------------------- |
| assets | uint256 | amount of assets to repay. |

**Returns**

| Name         | Type    | Description                                           |
| ------------ | ------- | ----------------------------------------------------- |
| repaidAssets | uint256 | number of repaid assets (can be lower than `assets`). |
| borrowShares | uint256 | number of borrowed shares subtracted from the debt.   |

### repayAtMaturity

```solidity
function repayAtMaturity(uint256 maturity, uint256 assets) external payable returns (uint256 repaidAssets)
```

Wraps ETH and repays to a maturity.

**Parameters**

| Name     | Type    | Description                                    |
| -------- | ------- | ---------------------------------------------- |
| maturity | uint256 | maturity date where the assets will be repaid. |
| assets   | uint256 | amount to be paid for the caller's debt.       |

**Returns**

| Name         | Type    | Description                                             |
| ------------ | ------- | ------------------------------------------------------- |
| repaidAssets | uint256 | the actual amount that was transferred into the Market. |

### withdraw

```solidity
function withdraw(uint256 assets) external nonpayable returns (uint256 shares)
```

Unwraps WETH from the floating pool and withdraws to caller.

**Parameters**

| Name   | Type    | Description                   |
| ------ | ------- | ----------------------------- |
| assets | uint256 | amount of assets to withdraw. |

**Returns**

| Name   | Type    | Description              |
| ------ | ------- | ------------------------ |
| shares | uint256 | number of burned shares. |

### withdrawAtMaturity

```solidity
function withdrawAtMaturity(uint256 maturity, uint256 assets, uint256 minAssetsRequired) external nonpayable returns (uint256 actualAssets)
```

Unwraps WETH from a maturity and withdraws to caller.

**Parameters**

| Name              | Type    | Description                                                                        |
| ----------------- | ------- | ---------------------------------------------------------------------------------- |
| maturity          | uint256 | maturity date where the assets will be withdrawn.                                  |
| assets            | uint256 | position size to be reduced.                                                       |
| minAssetsRequired | uint256 | minimum amount required by the caller (if discount included for early withdrawal). |

**Returns**

| Name         | Type    | Description                                                             |
| ------------ | ------- | ----------------------------------------------------------------------- |
| actualAssets | uint256 | amount of assets withdrawn (can include a discount for early withdraw). |
