# 👀 Previewer

The **Previewer** is a smart contract designed exclusively to be consumed by Exactly's web app.

It retrieves comprehensive on-chain information about Exactly's markets and accounts while minimizing the number of requests made, enhancing performance, and abstracting the front-end interface from protocol intricacies.

This contract can be found in the [periphery folder of Exactly's repo](https://github.com/exactly/protocol/blob/main/contracts/periphery/Previewer.sol).

### Public State Variables

#### auditor

```solidity
function auditor() external view returns (contract Auditor)
```

**Returns**

| Name    | Type             | Description |
| ------- | ---------------- | ----------- |
| auditor | contract Auditor | Auditor     |

#### basePriceFeed

```solidity
function basePriceFeed() external view returns (contract IPriceFeed)
```

**Returns**

| Name          | Type                | Description   |
| ------------- | ------------------- | ------------- |
| basePriceFeed | contract IPriceFeed | BasePriceFeed |

### View Methods

#### exactly

```solidity
function exactly(address account) external view returns (struct Previewer.MarketAccount[] data)
```

Function to get a certain account extended data.

**Parameters**

| Name    | Type    | Description                                         |
| ------- | ------- | --------------------------------------------------- |
| account | address | address which the extended data will be calculated. |

**Returns**

| Name | Type                       | Description                                             |
| ---- | -------------------------- | ------------------------------------------------------- |
| data | Previewer.MarketAccount\[] | extended accountability of all markets for the account. |

#### previewBorrowAtAllMaturities

```solidity
function previewBorrowAtAllMaturities(contract Market market, uint256 assets) external view returns (struct Previewer.FixedPreview[] previews)
```

Gets the assets plus fees offered by all VALID maturities when borrowing a certain amount.

**Parameters**

| Name   | Type            | Description                             |
| ------ | --------------- | --------------------------------------- |
| market | contract Market | address of the market.                  |
| assets | uint256         | amount of assets that will be borrowed. |

**Returns**

| Name     | Type                      | Description                                                                       |
| -------- | ------------------------- | --------------------------------------------------------------------------------- |
| previews | Previewer.FixedPreview\[] | array containing amount plus yield that account will receive after each maturity. |

#### previewBorrowAtMaturity

```solidity
function previewBorrowAtMaturity(contract Market market, uint256 maturity, uint256 assets) external view returns (struct Previewer.FixedPreview)
```

Gets the amount plus fees to be repaid at maturity when borrowing certain amount of assets.

**Parameters**

| Name     | Type            | Description                                           |
| -------- | --------------- | ----------------------------------------------------- |
| market   | contract Market | address of the market.                                |
| maturity | uint256         | maturity date/pool where the assets will be borrowed. |
| assets   | uint256         | amount of assets that will be borrowed.               |

**Returns**

| Name           | Type                   | Description                                                                |
| -------------- | ---------------------- | -------------------------------------------------------------------------- |
| positionAssets | Previewer.FixedPreview | positionAssets amount plus fees that the depositor will repay at maturity. |

#### previewDepositAtAllMaturities

```solidity
function previewDepositAtAllMaturities(contract Market market, uint256 assets) external view returns (struct Previewer.FixedPreview[] previews)
```

Gets the assets plus yield offered by all VALID maturities when depositing a certain amount.

**Parameters**

| Name   | Type            | Description                              |
| ------ | --------------- | ---------------------------------------- |
| market | contract Market | address of the market.                   |
| assets | uint256         | amount of assets that will be deposited. |

**Returns**

| Name     | Type                      | Description                                                                       |
| -------- | ------------------------- | --------------------------------------------------------------------------------- |
| previews | Previewer.FixedPreview\[] | array containing amount plus yield that account will receive after each maturity. |

#### previewDepositAtMaturity

```solidity
function previewDepositAtMaturity(contract Market market, uint256 maturity, uint256 assets) external view returns (struct Previewer.FixedPreview)
```

Gets the assets plus yield offered by a maturity when depositing a certain amount.

**Parameters**

| Name     | Type            | Description                                            |
| -------- | --------------- | ------------------------------------------------------ |
| market   | contract Market | address of the market.                                 |
| maturity | uint256         | maturity date/pool where the assets will be deposited. |
| assets   | uint256         | amount of assets that will be deposited.               |

**Returns**

| Name   | Type                   | Description                                                       |
| ------ | ---------------------- | ----------------------------------------------------------------- |
| assets | Previewer.FixedPreview | amount plus yield that the depositor will receive after maturity. |

#### previewRepayAtMaturity

```solidity
function previewRepayAtMaturity(contract Market market, uint256 maturity, uint256 positionAssets, address borrower) external view returns (struct Previewer.FixedPreview)
```

Gets the assets that will be repaid when repaying a certain amount at the current maturity.

**Parameters**

| Name           | Type            | Description                                                 |
| -------------- | --------------- | ----------------------------------------------------------- |
| market         | contract Market | address of the market.                                      |
| maturity       | uint256         | maturity date/pool where the assets will be repaid.         |
| positionAssets | uint256         | amount of assets that will be subtracted from the position. |
| borrower       | address         | address of the borrower.                                    |

**Returns**

| Name        | Type                   | Description                                       |
| ----------- | ---------------------- | ------------------------------------------------- |
| repayAssets | Previewer.FixedPreview | repayAssets amount of assets that will be repaid. |

#### previewWithdrawAtMaturity

```solidity
function previewWithdrawAtMaturity(contract Market market, uint256 maturity, uint256 positionAssets, address owner) external view returns (struct Previewer.FixedPreview)
```

Gets the amount to be withdrawn for a certain positionAmount of assets at maturity.

**Parameters**

| Name           | Type            | Description                                            |
| -------------- | --------------- | ------------------------------------------------------ |
| market         | contract Market | address of the market.                                 |
| maturity       | uint256         | maturity date/pool where the assets will be withdrawn. |
| positionAssets | uint256         | amount of assets that will be tried to withdraw.       |
| owner          | address         | owner of the fixed deposit position                    |

**Returns**

| Name           | Type                   | Description                                   |
| -------------- | ---------------------- | --------------------------------------------- |
| withdrawAssets | Previewer.FixedPreview | withdrawAssets amount that will be withdrawn. |
