# Rewards Distribution Model

The **RewardsController** contract distributes rewards to accounts that interact with the protocol.

Reward programs typically rely on a fixed emission schedule over time or a predetermined distribution function rule. However, these approaches could be more effective in attracting a diverse user base, often leading to a concentration of rewards among a few select wallets.

To optimize the use of our available token supply, we are introducing a new distribution rule that adjusts the distribution of rewards based on the current achieved percentage of the target loan volume of the protocol. Additionally, we are adding a feature that adjusts the rewards given to borrowers and liquidity providers based on the utilization rate of each pool. This will help ensure that our token distribution is aligned with the usage and demand for the protocol's utilization and helps optimize the system's overall performance.

We built a new adaptive model at Exactly to address this issue: a recursive token distribution in continuous time. The model implements an incentive function based on how closely or far the program's objectives are being met during the rewards period.

The proposed model has two distinct modules:

* The distribution module determines the number of tokens allocated during each time frame (determined by two consecutive protocol transactions).
* The allocation module determines the proportion of tokens allocated to each protocol user class in each specific period.

Since its conception, this contract supports a multiple array of different tokens.
