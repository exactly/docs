# 🔮 Price Feeds

Exactly Protocol uses [Chainlink's data feeds](https://docs.chain.link/docs/using-chainlink-reference-contracts/) to get reliable, up-to-date, and secure asset prices to calculate users' lending power and [health factor](https://docs.exact.ly/guides/liquidations#health-factor).

## Chainlink

Chainlink is the most used data provider in the industry. It provides secure pricing feeds and is the DeFi standard oracle network with [over six trillion in transaction value enabled](https://chain.link/). No liveness checks are performed while retrieving oracle data. Chainlink offers robust and historically stable price feeds, even more on Mainnet for high-liquid assets such as WBTC, ETH, and DAI. Also, avoiding this check can lower the gas consumption of the involved transactions. The following contracts depend directly on Chainlink's price feed: [Auditor](protocol/auditor.md), [Price Feed Wrapper](protocol/pricefeedwrapper.md), and [Price Feed Double](protocol/pricefeeddouble.md).

## Uniswap TWAPs

Other sources, such as [Uniswap's TWAPs](https://docs.uniswap.org/protocol/concepts/V3-overview/oracle), have been considered but finally discarded.

After Ethereum's upgrade from proof of work to proof of stake, block proposers are chosen deterministically before they validate blocks. This feature creates new challenges for decentralized price oracles, like those provided by Uniswap V3, because they open up more significant potential for inter-block price manipulation, as detailed below.

Block proposers are alerted when they are selected to propose a block. This gives them a unique opportunity to carry out oracle manipulation attacks. If they are chosen to propose block _n_, they can attempt to manipulate the spot price on block _n-1_, knowing that they will be free to arbitrage their price manipulation on the next block (and censor any other attempts at arbitrage).

It is hard to estimate how many block proposers will view oracle manipulation attacks as a legitimate way to increase their income. It seems likely many will not take the risk of carrying out these kinds of attacks. However, as long as the number is non-zero, it is clear there is some reduced cost for carrying out these possible attacks.

## Price Denominations

All asset prices, including stablecoins, are accurately reflected by querying them from live, and regularly updated price feeds. This approach avoids hardcoded values, providing reliable and up-to-date pricing information for its users.

On **Mainnet**, prices are obtained and used by the Auditor to calculate accounts' collateral and debt values in **ETH** denomination. In this way, an extra call (_ETH-USD_) is saved, which translates to a reduction in gas consumption for liquidity checks.

On **Optimism**, prices are currently retrieved and used in **USD** denomination due to lower availability in price feeds offered by Chainlink.

It's important to notice that this difference is only spotted at a smart contract level and does not imply any variation in the result of the health factor calculation. The web app shows prices in **USD** denominations for a better understanding from a user's perspective.
