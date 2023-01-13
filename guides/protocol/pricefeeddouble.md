# PriceFeedDouble

Returns the price of an asset considering two different [Chainlink](https://docs.chain.link/docs/data-feeds/price-feeds/addresses/) price feeds.

Written as a generic implementation it can be used, for example, to retrieve the price of `WBTC`. Queries `BTC/ ETH` feed and multiplies it by the exchange rate between `WBTC / BTC` before returning the price that it’s then used by the [Auditor](auditor.md).

## Public State Variables

### baseUnit

```solidity
function baseUnit() external view returns (uint256)
```

The base units are used to normalize the answer when multiplied by the second price feed rate.

### decimals

```solidity
function decimals() external view returns (uint8)
```

Number of decimals that the answer of this price feed has.

### priceFeedOne

```solidity
function priceFeedOne() external view returns (contract IPriceFeed)
```

Main price feed where the price is fetched from.

### priceFeedTwo

```solidity
function priceFeedTwo() external view returns (contract IPriceFeed)
```

Second price feed where the asset's rate is fetched from.

## View Methods

### latestAnswer

```solidity
function latestAnswer() external view returns (int256)
```

Returns the price feed's latest value considering the other price feed's rate.
