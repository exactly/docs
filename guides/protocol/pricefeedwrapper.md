# PriceFeedWrapper

Returns the price of an asset that doesn’t have a direct feed from [Chainlink](https://docs.chain.link/docs/data-feeds/price-feeds/addresses/).

Written as a generic implementation it can be used, for example, to retrieve the price of `wstETH`. Queries `stETH / ETH` feed and multiplies it by the exchange rate between `stETH / wstETH` before returning the price that it’s then used by the [Auditor](auditor.md).

### Public State Variables

#### baseUnit

```solidity
function baseUnit() external view returns (uint256)
```

Base units that are sent to the conversion function to get the asset rate.

#### conversionSelector

```solidity
function conversionSelector() external view returns (bytes4)
```

Function selector of the wrapper contract where the asset rate is fetched from.

#### decimals

```solidity
function decimals() external view returns (uint8)
```

Number of decimals that the answer of this price feed has.

#### mainPriceFeed

```solidity
function mainPriceFeed() external view returns (contract IPriceFeed)
```

Main price feed where the price is fetched from.

#### wrapper

```solidity
function wrapper() external view returns (address)
```

Address of the wrapper contract where the asset rate is fetched from.

### View Methods

#### latestAnswer

```solidity
function latestAnswer() external view returns (int256)
```

Returns the price feed's latest value considering the wrapped asset's rate.

**Returns**

| Type   | Description         |
| ------ | ------------------- |
| int256 | latest asset price. |
