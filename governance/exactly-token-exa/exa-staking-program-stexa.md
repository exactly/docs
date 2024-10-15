---
icon: steak
description: >-
  The EXA Staking Program enables EXA token holders to get treasury fees and
  gain additional voting power by staking their EXA tokens.
---

# EXA Staking Program (stEXA)

{% embed url="https://www.youtube.com/watch?v=lIyyR6FFn4k" %}

### Distribution of protocol fees

The protocol's treasury fees will be partly allocated to the Staking Program. A specified fraction (providerRatio) of these fees will be assigned to the staking pool, with an initial parameter of 0.5 (50% of the fees). The rest of the fees go to [Exactly DAO Savings Account Multisig](https://optimistic.etherscan.io/address/0x8a1c05c4462b3554814a637e940b3342ffbe02f2).

[Staking Contract address](https://optimistic.etherscan.io/address/0xCEed2bFE740F02dB6094eBE89FF93b1031be752b)

### Dividend module

Users who stake their EXA tokens will receive dividends based on the staking duration. The dividend index updates regularly, ensuring that participants who stake for longer periods receive proportional rewards. This module supports multiple assets, although a single-asset approach for exaUSDC will be used to simplify the automatic dividend distribution.

### Staking Steps

* Go to  https://app.exact.ly/staking and select the amount of EXA you want to stake.&#x20;
* Click “Stake EXA” to confirm.
* Once the transaction is completed, you will see your staked EXA.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2024-09-27 a las 14.51.44.png" alt=""><figcaption></figcaption></figure>

### Staking period

The staking period will be twelve months. During this time, the stakeholder can add more EXA to their stake to earn more rewards, claim them, or partially/fully unstake their position.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2024-09-27 a las 14.55.30.png" alt=""><figcaption></figcaption></figure>

Once you claim the fees, the exaUSDC will be deposited into your account. You can view them in the Dashboard section.

Remember, the exaUSDC you earn from staking is automatically deposited into the USDC market, compounding its variable APR. It will continue to generate esEXA rewards as long as you hold it. Plus, you can swap exaUSDC for USDC anytime directly from your dashboard.

You can swap your exaUSDC for USDC by clicking “Withdraw”

<figure><img src="../../.gitbook/assets/Captura de pantalla 2024-09-27 a las 15.07.44.png" alt=""><figcaption></figcaption></figure>

### Enhanced voting power

Stakers will gain additional voting power, directly influencing protocol decisions. The voting power increases with the staking duration, incentivizing users to stake for extended periods.

### Extra rewards

Besides dividends, participants in the Staking Program could receive rewards in other tokens, providing further incentives for long-term participation.

### EXA buybacks

Discretionary EXA buybacks buy the Treasury with the remaining fees not distributed in the staking program.

### Penalty function for early withdrawals

The program will incorporate a penalty function to encourage the completion of the staking program period. Early withdrawals will incur penalties, reducing the dividends awarded.

### Penalty function for overstayers

Users will be encouraged to restart their positions after the end of the staking program. There will be a penalty function for users who stay in the program longer after the finish date.

### Initial parameters

* ProviderRatio: 0.5 (50% of treasury fees to the staking pool)&#x20;
* Minimum Time to Stake: 0 months&#x20;
* Reference Period to Stake: 12 months&#x20;
* Penalty Growth Factor: 2&#x20;
* Penalty Threshold: 0.1 These parameters guide how dividends, penalties, and rewards are calculated and distributed.\
  \
  Initial parameters, such as the provider ratio, minimum staking time, and penalty factors, can be modified through governance.

### **Estimated Staking APR**

The Staking Program "Estimated APR" is equal to 50% of last week's treasury fees from the USDC market (annualized) divided by the total EXA Staked (in $). &#x20;

You can find the actual staking APR [here](https://app.exact.ly/staking) and the weekly treasury fees [here](https://dune.com/exactly/exactly#treasury-fees).

### Math Notes and Audits

* [**Staking Program Math Notes**](https://github.com/exactly/papers/blob/main/Staking%20Model%20Math%20Notes.pdf)
* [**Chainsafe Staking Program Audit**](https://github.com/exactly/audits/blob/main/Chainsafe%20Staking%20Contract%20\(Aug-24\).pdf)
* [**Sherlock Staking Program Audit**](https://github.com/exactly/audits/blob/main/Sherlock%20Staking%20Contract%20\(Aug-24\).pdf)



