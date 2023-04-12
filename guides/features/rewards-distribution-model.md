# Rewards Distribution Model

The **RewardsController** contract distributes rewards to accounts that interact with the protocol.

Reward programs typically rely on a fixed emission schedule over time or a predetermined distribution function rule. However, these approaches could be more effective in attracting a diverse user base, often leading to a concentration of rewards among a few select wallets.

To optimize the use of our available token supply, we are introducing a new distribution rule that adjusts the distribution of rewards based on the current achieved percentage of the target loan volume of the protocol. Additionally, we are adding a feature that adjusts the rewards given to borrowers and liquidity providers based on the utilization rate of each pool. This will help ensure that our token distribution is aligned with the usage and demand for the protocol's utilization and helps optimize the system's overall performance.

We built a new adaptive model at Exactly to address this issue: a recursive token distribution in continuous time. The model implements an incentive function based on how closely or far the program's objectives are being met during the rewards period.

The proposed model has two distinct modules:

* The distribution module determines the number of tokens allocated during each time frame (determined by two consecutive protocol transactions).
* The allocation module determines the proportion of tokens allocated to each protocol user class in each specific period.

Since its conception, this contract supports a multiple array of different tokens.

### Audits

As a DeFi protocol, our top priority is to provide a reliable and secure system for our users. We are committed to continuously improving our platform to ensure the safety and integrity of all transactions. For that reason we have enlisted the expertise of Coinspect, a highly respected security audit firm, to perform an extensive review of our [Rewards Controller contract](../protocol/rewardscontroller/).

Coinspect has conducted three separate audits on the Rewards Controller, which have taken place in January, February, and March of 2023. Each audit was performed to identify any potential vulnerabilities, validate the overall design, and ensure that our Rewards Controller meets the highest security standards.

You can access the detailed reports of these three audits by clicking on the following links:

1. [Coinspect Rewards Controller 1st audit (Jan-23)](https://github.com/exactly/audits/blob/main/Coinspect%20RewardsController%201st%20audit%20\(Jan-23\).pdf)
2. [Coinspect Rewards Controller 2nd audit (Feb-23)](https://github.com/exactly/audits/blob/main/Coinspect%20RewardsController%202nd%20audit%20\(Feb-23\).pdf)
3. [Coinspect Rewards Controller 3rd audit (Mar-23)](https://github.com/exactly/audits/blob/main/Coinspect%20RewardsController%203rd%20audit%20\(Mar-23\).pdf)

Each audit report contains a comprehensive analysis of the Rewards Controller's security, addressing any potential issues and outlining the steps taken to resolve them.

### Math model

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fn6wwJ0pvrhjXGxxDpmNa%2Fuploads%2FjefVkjTKHNJSgk7ueIjH%2FExactly%20Rewards%20Controller%20Math%20Model.pdf?alt=media&token=7fe72436-b79a-4220-862b-340e71f069cb" %}
