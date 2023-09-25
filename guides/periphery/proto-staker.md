# 🥩 Proto-Staker

This smart contract is integrated into Exactly Protocol’s web app to enable users to provide EXA and WETH to a [Velodrome](https://velodrome.finance/) liquidity pool in several ways:

* Providing ETH, where half is wrapped into WETH, and the remaining half is used to purchase EXA before being added to the EXA/WETH Liquidity Pool (LP).
* Providing both ETH and EXA to be added to the EXA/WETH Liquidity Pool.
* Claiming EXA rewards and sending ETH, which are both added to the Liquidity Pool.

This contract also uses '[permits](https://help.1inch.io/en/articles/5435386-permit-712-signed-token-approvals-and-how-they-work-on-1inch)' to perform these transactions.

**GitHub URL:** [https://github.com/exactly/protocol/blob/history/proto-staking-0/contracts/periphery/ProtoStaker.sol ](https://github.com/exactly/protocol/blob/history/proto-staking-0/contracts/periphery/ProtoStaker.sol)

### Potential Exposure to Impermanent Loss

By engaging with the Proto-Staker contract within Exactly Protocol, users should be aware that they may be exposed to a phenomenon known as **impermanent loss**. This financial risk is prevalent in dApps, where liquidity is provided to automated market makers (AMMs). Impermanent loss can occur when the price ratio of the provided assets changes, resulting in a temporary discrepancy between the current value of the held assets within the liquidity pool and the value if held outside the pool. In the context of the EXA/WETH Liquidity Pool, price changes in either EXA or WETH can trigger this loss, impacting the overall value of the contributed assets. Users should carefully consider this risk and monitor the price movements of the involved assets when interacting with the Proto-Staker.

Users can find out how to calculate impermanent loss by reading [this article](https://medium.com/auditless/how-to-calculate-impermanent-loss-full-derivation-803e8b2497b7).

\
\
