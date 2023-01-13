# Auditor

The **Auditor** is the risk management layer of the protocol; it determines how much collateral a user is required to maintain, and whether (and by how much) a user can be liquidated. Each time a user borrows from a [Market](market/), the **Auditor** validates his account’s liquidity to determine his health factor.

## Public State Variables

### ASSETS\_THRESHOLD

```solidity
function ASSETS_THRESHOLD() external view returns (uint256)
```

Maximum value the liquidator can send and still have granular control of max assets. Above this threshold, they should send `type(uint256).max`.

### BASE\_FEED

```solidity
function BASE_FEED() external view returns (address)
```

Address that a market should have as price feed to consider as base price and avoid external price call.

### TARGET\_HEALTH

```solidity
function TARGET_HEALTH() external view returns (uint256)
```

Target health factor that the account should have after it's liquidated to prevent cascade liquidations.

### accountMarkets

```solidity
function accountMarkets(address) external view returns (uint256)
```

Tracks the markets' indexes that an account has entered as collateral.

### liquidationIncentive

```solidity
function liquidationIncentive() external view returns (uint128 liquidator, uint128 lenders)
```

Liquidation incentive factors for the liquidator and the lenders of the market where the debt is repaid.

### marketList

```solidity
function marketList(uint256) external view returns (contract Market)
```

Array of all enabled markets.

### markets

```solidity
function markets(contract Market) external view returns (uint128 adjustFactor, uint8 decimals, uint8 index, bool isListed, contract IPriceFeed priceFeed)
```

Stores market parameters per each enabled market.

### priceDecimals

```solidity
function priceDecimals() external view returns (uint256)
```

Decimals that the answer of all price feeds should have.

## View Methods

### accountLiquidity

```solidity
function accountLiquidity(address account, contract Market marketToSimulate, uint256 withdrawAmount) external view returns (uint256 sumCollateral, uint256 sumDebtPlusEffects)
```

Returns account's liquidity calculation.

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

### allMarkets

```solidity
function allMarkets() external view returns (contract Market[])
```

Retrieves all markets.

**Returns**

| Type               | Description              |
| ------------------ | ------------------------ |
| contract Market\[] | List of enabled markets. |

### assetPrice

```solidity
function assetPrice(contract IPriceFeed priceFeed) external view returns (uint256)
```

Gets the asset price of a price feed.

_If Chainlink's asset price is <= 0 the call is reverted._

**Parameters**

| Name      | Type                | Description                                                                 |
| --------- | ------------------- | --------------------------------------------------------------------------- |
| priceFeed | contract IPriceFeed | address of Chainlink's Price Feed aggregator used to query the asset price. |

**Returns**

| Type    | Description                                         |
| ------- | --------------------------------------------------- |
| uint256 | The price of the asset scaled to 18-digit decimals. |

### calculateSeize

```solidity
function calculateSeize(contract Market repayMarket, contract Market seizeMarket, address borrower, uint256 actualRepayAssets) external view returns (uint256 lendersAssets, uint256 seizeAssets)
```

Calculates the amount of collateral to be seized when a position is undercollateralized.

**Parameters**

| Name              | Type            | Description                                                    |
| ----------------- | --------------- | -------------------------------------------------------------- |
| repayMarket       | contract Market | market from where the debt will be repaid.                     |
| seizeMarket       | contract Market | market from where the assets will be seized by the liquidator. |
| borrower          | address         | account in which assets are being seized.                      |
| actualRepayAssets | uint256         | amount being repaid.                                           |

**Returns**

| Name          | Type    | Description                                                                  |
| ------------- | ------- | ---------------------------------------------------------------------------- |
| lendersAssets | uint256 | amount to be added for other lenders as a compensation of bad debt clearing. |
| seizeAssets   | uint256 | amount that can be seized by the liquidator.                                 |

### checkLiquidation

```solidity
function checkLiquidation(contract Market repayMarket, contract Market seizeMarket, address borrower, uint256 maxLiquidatorAssets) external view returns (uint256 maxRepayAssets)
```

Allows/rejects liquidation of assets.

_This function can be called externally, but only will have effect when called from a market._

**Parameters**

| Name                | Type            | Description                                                 |
| ------------------- | --------------- | ----------------------------------------------------------- |
| repayMarket         | contract Market | market from where the debt is being repaid.                 |
| seizeMarket         | contract Market | market from where the liquidator will seize assets.         |
| borrower            | address         | address in which the assets are being liquidated.           |
| maxLiquidatorAssets | uint256         | maximum amount of debt the liquidator is willing to accept. |

**Returns**

| Name           | Type    | Description                                               |
| -------------- | ------- | --------------------------------------------------------- |
| maxRepayAssets | uint256 | capped amount of debt the liquidator is allowed to repay. |

### checkSeize

```solidity
function checkSeize(contract Market repayMarket, contract Market seizeMarket) external view
```

Allow/rejects seizing of assets.

_This function can be called externally, but only will have effect when called from a market._

**Parameters**

| Name        | Type            | Description                                |
| ----------- | --------------- | ------------------------------------------ |
| repayMarket | contract Market | market from where the debt will be repaid. |
| seizeMarket | contract Market | market where the assets will be seized.    |

### checkShortfall

```solidity
function checkShortfall(contract Market market, address account, uint256 amount) external view
```

Checks if the account has liquidity shortfall.

**Parameters**

| Name    | Type            | Description                                             |
| ------- | --------------- | ------------------------------------------------------- |
| market  | contract Market | address of the market where the operation will happen.  |
| account | address         | address of the account to check for possible shortfall. |
| amount  | uint256         | amount that the account wants to withdraw or transfer.  |

## Write Methods

### checkBorrow

```solidity
function checkBorrow(contract Market market, address borrower) external nonpayable
```

Validates that the current state of the position and system are valid.

_To be called after adding the borrowed debt to the account position._

**Parameters**

| Name     | Type            | Description                                      |
| -------- | --------------- | ------------------------------------------------ |
| market   | contract Market | address of the market where the borrow is made.  |
| borrower | address         | address of the account that will repay the debt. |

### enableMarket

```solidity
function enableMarket(contract Market market, contract IPriceFeed priceFeed, uint128 adjustFactor, uint8 decimals) external nonpayable
```

Enables a certain market.

_Enabling more than 256 markets will cause an overflow when casting market index to uint8._

**Parameters**

| Name         | Type                | Description                                                                         |
| ------------ | ------------------- | ----------------------------------------------------------------------------------- |
| market       | contract Market     | market to add to the protocol.                                                      |
| priceFeed    | contract IPriceFeed | address of Chainlink's Price Feed aggregator used to query the asset price in base. |
| adjustFactor | uint128             | market's adjust factor for the underlying asset.                                    |
| decimals     | uint8               | decimals of the market's underlying asset.                                          |

### enterMarket

```solidity
function enterMarket(contract Market market) external nonpayable
```

Allows assets of a certain market to be used as collateral for borrowing other assets.

**Parameters**

| Name   | Type            | Description                      |
| ------ | --------------- | -------------------------------- |
| market | contract Market | market to enabled as collateral. |

### exitMarket

```solidity
function exitMarket(contract Market market) external nonpayable
```

Removes market from sender's account liquidity calculation.

_Sender must not have an outstanding borrow balance in the asset, or be providing necessary collateral for an outstanding borrow._

**Parameters**

| Name   | Type            | Description                          |
| ------ | --------------- | ------------------------------------ |
| market | contract Market | market to be disabled as collateral. |

### handleBadDebt

```solidity
function handleBadDebt(address account) external nonpayable
```

Checks if account has debt with no collateral, if so then call `clearBadDebt` from each market.

_Collateral is multiplied by price and adjust factor to be accurately evaluated as positive collateral asset._

**Parameters**

| Name    | Type    | Description                             |
| ------- | ------- | --------------------------------------- |
| account | address | account in which debt is being checked. |
