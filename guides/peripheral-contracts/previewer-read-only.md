# 👀 Previewer (read-only)

The Previewer is a read-only smart contract specifically engineered for Exactly's web application. Its role is to gather comprehensive on-chain data pertaining to Exactly's markets, and user accounts while minimizing the amount of necessary requests, thus enhancing performance. Moreover, it abstracts the complexities of the protocol from the front-end interface by directly returning oracle prices, counting the amount of deposits and borrows per market, and providing the current snapshot of any account being queried.

This contract can be found in the [periphery folder of Exactly's repo](https://github.com/exactly/protocol/blob/main/contracts/periphery/Previewer.sol).

Please be advised that the `Previewer` contract has not undergone a formal security audit. As such, we strongly discourage integrations involving this contract since it is exclusively intended for read-only purposes by the [Exactly Web App](https://app.exact.ly/). Using unaudited contracts for other purposes may expose users to potential security risks or unexpected behavior.

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
