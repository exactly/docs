# 🔮 Chainlink Price Feeds

Exactly Protocol uses [Chainlink's data feeds](https://docs.chain.link/docs/using-chainlink-reference-contracts/) to get reliable, up-to-date, and secure asset prices to calculate users' lending power and [health factor](https://docs.exact.ly/guides/liquidations#health-factor).

## OP Mainnet

<table><thead><tr><th>Asset</th><th>Price Feed</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td>esEXA</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedesEXA.json">PriceFeedesEXA</a></td><td></td><td></td></tr><tr><td>EXA</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedEXA.json">PriceFeedEXA</a></td><td></td><td></td></tr><tr><td>OP</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedOP.json">PriceFeedOP</a></td><td></td><td></td></tr><tr><td>USDC</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedUSDC.json">PriceFeedUSDC</a></td><td></td><td></td></tr><tr><td>WBTC</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedWBTC.json">PriceFeedWBTC</a></td><td></td><td></td></tr><tr><td>WETH</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedWETH.json">PriceFeedWETH</a></td><td></td><td></td></tr><tr><td>wstETH</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/optimism/PriceFeedwstETH.json">PriceFeedwstETH</a></td><td></td><td></td></tr></tbody></table>

## Ethereum Mainnet

<table><thead><tr><th>Asset</th><th>Price Feed</th><th data-hidden>Price Feed</th><th data-hidden></th></tr></thead><tbody><tr><td>DAI</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/ethereum/PriceFeedDAI.json">PriceFeedDAI</a></td><td>PriceFeedesEXA</td><td></td></tr><tr><td>ETH</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/ethereum/PriceFeedETH.json">PriceFeedETH</a></td><td></td><td></td></tr><tr><td>USDC</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/ethereum/PriceFeedUSDC.json">PriceFeedUSDC</a></td><td></td><td></td></tr><tr><td>WBTC</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/ethereum/PriceFeedWBTC.json">PriceFeedWBTC</a></td><td></td><td></td></tr><tr><td>wstETH</td><td><a href="https://github.com/exactly/protocol/blob/main/deployments/ethereum/PriceFeedwstETH.json">PriceFeedwstETH</a></td><td></td><td></td></tr></tbody></table>

## Chainlink

Chainlink is the most used data provider in the industry. It provides secure pricing feeds and is the DeFi standard oracle network with [over six trillion in transaction value enabled](https://chain.link/). No liveness checks are performed while retrieving oracle data. Chainlink offers robust and historically stable price feeds, even more on Mainnet for high-liquid assets such as WBTC, ETH, and DAI. Also, avoiding this check can lower the gas consumption of the involved transactions. The following contracts depend directly on Chainlink's price feed: [Auditor](protocol/auditor.md), [Price Feed Wrapper](protocol/pricefeedwrapper.md), and [Price Feed Double](protocol/pricefeeddouble.md).

## Uniswap TWAPs

Other sources, such as [Uniswap's TWAPs](https://docs.uniswap.org/protocol/concepts/V3-overview/oracle), have been considered but finally discarded.

After Ethereum's upgrade from proof of work to proof of stake, block proposers are chosen deterministically before they validate blocks. This feature creates new challenges for decentralized price oracles, like those provided by Uniswap V3, because they open up more significant potential for inter-block price manipulation, as detailed below.

Block proposers are alerted when they are selected to propose a block. This gives them a unique opportunity to carry out oracle manipulation attacks. If they are chosen to propose block _n_, they can attempt to manipulate the spot price on block _n-1_, knowing that they will be free to arbitrage their price manipulation on the next block (and censor any other attempts at arbitrage).

It is hard to estimate how many block proposers will view oracle manipulation attacks as a legitimate way to increase their income. It seems likely many will not take the risk of carrying out these kinds of attacks. However, as long as the number is non-zero, it is clear there is some reduced cost for carrying out these possible attacks.

## Price Denominations

All asset prices, including stablecoins, are accurately reflected by querying them from live, and regularly updated price feeds. This approach avoids hardcoded values, providing reliable and up-to-date pricing information for its users.

On **Mainnet**, the [Auditor](protocol/auditor.md) obtains and uses prices to calculate accounts' collateral and debt values in **ETH** denomination. In this way, an extra call (_ETH-USD_) is saved, which translates to a reduction in gas consumption for liquidity checks.

On **Optimism**, prices are currently retrieved and used in **USD** denomination due to lower availability in price feeds offered by Chainlink.

It's important to notice that this difference is only spotted at a smart contract level and does not imply any variation in the result of the health factor calculation. The web app shows prices in **USD** denominations for a better understanding from a user's perspective.

## Deprecated Chainlink interface

Exactly's smart contracts are currently fetching asset prices through a [deprecated interface provided by Chainlink](https://github.com/smartcontractkit/chainlink/blob/e1e78865d4f3e609e7977777d7fb0604913b63ed/contracts/src/v0.6/EACAggregatorProxy.sol#L41-L58).

In a [previous audit carried out by Coinspect (EXA-36)](https://github.com/exactly/audits/blob/main/Coinspect%204th%20audit%20\(Oct-22\).pdf), we already acknowledged this decision in the spirit of transparency and assure our users that we have assessed the associated risks and taken appropriate measures to mitigate them.

Our choice to continue using this deprecated interface is based on several factors. We have implemented a low minimum timelock delay (1 day) and an upgradable [Auditor](protocol/auditor.md), which enable us to respond swiftly in the event that prices are not being updated accurately.

We have confidence in the robustness and historical stability of Chainlink's price feeds, particularly for highly liquid assets, the only ones enabled as [Markets](protocol/market/).

It is worth noting that another difference with checking liveness is that transactions would revert in case of outdated `updateTimes` but as a downfall this may potentially hinder liquidations. We have carefully weighed the trade-offs and decided to assume the associated risks while focusing on **reducing gas costs** for our users.

We remain committed to the security and reliability of our protocol and will continue to monitor and evaluate any third-party integrations.
