# 🔮 Price Feeds

Exactly Protocol uses [Chainlink's data feeds](https://docs.chain.link/docs/using-chainlink-reference-contracts/) to get reliable, up-to-date, and secure asset prices to calculate users' lending power and health factor.

## Chainlink

Chainlink is the most used data provider in the industry. It provides secure pricing feeds and it is the DeFi standard oracle network with [over six trillion in transaction value enabled](https://chain.link/).
No liveness checks are performed while retrieving oracle data. We believe Chainlink offers
robust and historically stable price feeds, even more on Mainnet for high liquid assets
such as WBTC, ETH and DAI. Also, by avoiding this check, we can lower the gas consumption of the involved transactions.
The following contracts depend directly on Chainlink's price feed: [Auditor](../guides/protocol/auditor.md), [Price Feed Wrapper](../guides/protocol/pricefeedwrapper.md) and [Price Feed Double](../guides/protocol/pricefeeddouble.md).

## Uniswap TWAPs

Other sources, such as [Uniswap's TWAPs](https://docs.uniswap.org/protocol/concepts/V3-overview/oracle), have also been considered but finally discarded.

After Ethereum's upgrade from proof of work to proof of stake, block proposers are chosen deterministically before they validate blocks. This feature creates new challenges for decentralized price oracles, like those provided by Uniswap V3, because they open up more significant potential for inter-block price manipulation, as detailed below.

Block proposers are alerted ahead of time when they are selected to propose a block. This gives them a unique opportunity to carry out oracle manipulation attacks. If they are chosen to propose block _n_, they can attempt to manipulate the spot price on block _n-1_, knowing that they will be free to arbitrage their own price manipulation on the next block (and censor any other attempts at arbitrage).

It is hard to estimate how many block proposers will view oracle manipulation attacks as a legitimate way to increase their income. It seems likely many will not take the risk of carrying out these kinds of attacks. However, as long as the number is non-zero, it is clear there is some reduced cost for carrying out these possible attacks.

## Health Factor

The health factor is calculated from the user's collateral balance (in `ETH`) multiplied by each asset's adjust factor divided by the user's debt which is also divided by this adjust factor.

Given an `ETH` adjusted factor of `0.8`, a deposit of `112.5 ETH` and borrow of `60 ETH`:

The adjusted collateral will equal `90` (`112.5 * 0.8`).

The adjusted debt will equal `75` (`60 / 0.8`).

The health factor will then equal `1.2`.

Below a health factor of `1`, the user will be considered with shortfall and open to [liquidation](../getting-started/math-paper.md#6.-liquidations).
