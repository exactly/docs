# InterestRateModel

Given supply and demand values, the **InterestRateModel** is queried to calculate and return both fixed and variable rates. Contains parameters as state variables that are used to get the different points in the utilization curve for an asset.\
There's one **InterestRateModel** contract per enabled asset.

### Public State Variables

#### fixedCurveA

```solidity
function fixedCurveA() external view returns (uint256)
```

Scale factor of the fixed curve.

#### fixedCurveB

```solidity
function fixedCurveB() external view returns (int256)
```

Origin intercept of the fixed curve.

#### fixedMaxUtilization

```solidity
function fixedMaxUtilization() external view returns (uint256)
```

Asymptote of the fixed curve.

#### floatingCurveA

```solidity
function floatingCurveA() external view returns (uint256)
```

Scale factor of the floating curve.

#### floatingCurveB

```solidity
function floatingCurveB() external view returns (int256)
```

Origin intercept of the floating curve.

#### floatingMaxUtilization

```solidity
function floatingMaxUtilization() external view returns (uint256)
```

Asymptote of the floating curve.

### View Methods

#### fixedBorrowRate

```solidity
function fixedBorrowRate(uint256 maturity, uint256 amount, uint256 borrowed, uint256 supplied, uint256 backupAssets) external view returns (uint256)
```

Gets the rate to borrow a certain amount at a certain maturity with supply/demand values in the fixed rate pool and assets from the backup supplier.

**Parameters**

| Name         | Type    | Description                                          |
| ------------ | ------- | ---------------------------------------------------- |
| maturity     | uint256 | maturity date for calculating days left to maturity. |
| amount       | uint256 | the current borrow's amount.                         |
| borrowed     | uint256 | ex-ante amount borrowed from this fixed rate pool.   |
| supplied     | uint256 | deposits in the fixed rate pool.                     |
| backupAssets | uint256 | backup supplier assets.                              |

**Returns**

| Type    | Description                                                                        |
| ------- | ---------------------------------------------------------------------------------- |
| uint256 | rate of the fee that the borrower will have to pay (represented with 18 decimals). |

#### floatingBorrowRate

```solidity
function floatingBorrowRate(uint256 utilizationBefore, uint256 utilizationAfter) external view returns (uint256)
```

Returns the interest rate integral from utilizationBefore to utilizationAfter.

_Minimum and maximum checks to avoid negative rate._

**Parameters**

| Name              | Type    | Description                                           |
| ----------------- | ------- | ----------------------------------------------------- |
| utilizationBefore | uint256 | ex-ante utilization rate, with 18 decimals precision. |
| utilizationAfter  | uint256 | ex-post utilization rate, with 18 decimals precision. |

**Returns**

| Type    | Description                                    |
| ------- | ---------------------------------------------- |
| uint256 | the interest rate, with 18 decimals precision. |
