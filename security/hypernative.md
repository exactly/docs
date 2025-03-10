# 🛡️ Hypernative



## Real-Time Monitoring and Automated Threat Response by Hypernative

Exactly Protocol has officially partnered with [Hypernative](https://www.hypernative.io/) to integrate real-time monitoring and automated response systems to strengthen security infrastructure. This collaboration ensures the proactive detection of threats and allows swift action to protect user assets. The integration was executed based on the directives outlined in the proposals [EXAIP-7](https://gov.exact.ly/#/proposal/0x1d9916b71607aa8ff50394318ee6892665ffb96dc7ad3e7e4df7c36e04123001) and [EXAIP-12](https://gov.exact.ly/#/proposal/0xa0dade37527b9f37340b89a2ebe5929cceba22fbff1d627301322315a22e5a7f).

#### About Hypernative

Hypernative is a real-time monitoring, risk detection, and automated response solution that accurately identifies threats and gives customers precious minutes to respond before exploits can do damage. Over 130 Web3 projects rely on Hypernative’s enterprise-grade platform that monitors over $37 billion of digital assets across more than 40 chains. The list includes Balancer, Blockdaemon, Chainalysis, Chainlink, Circle, Consensys, Ethena, Etherfi, Galaxy, Linea, Quantstamp, Solana, Starknet, and Uniswap.

### **Active Monitoring and Automated Response System**

Hypernative continuously monitors Exactly Protocol’s smart contracts and all incoming transactions. In the event of suspicious activity—such as a malicious contract interacting with Exactly Protocol—Hypernative is equipped to trigger an immediate response automatically. This response involves pausing the protocol’s critical contracts, preventing further damage, and securing user funds before the threat escalates.

We implemented a custom Pauser Contract, which enables the protocol to be paused automatically in response to any threat detected by Hypernative. This contract is a core component of the security framework and is managed by the protocol’s governance and multisig structure. Hypernative, through the Emergency Admin Role, is authorized to activate the pausing function, but it cannot resume operations, which the protocol owner strictly handles via multisig.

**The Pauser Contract:**

The Pauser Contract halts operations across Exactly Protocol’s ecosystem during an emergency. Key technical details:

* Contract Address: 0x8cC05394eD714073758E9bEf8073a83d79F6F2A3
* Functionality: The contract can pause specific protocol contracts or the entire protocol if needed. This includes halting all market activities, such as withdrawals, deposits, and transfers, ensuring total containment of the incident.

Role Structure:

* Emergency Admin Role:\
  This role, assigned to Hypernative, allows them to trigger the pausing of contracts in the event of a security threat. Hypernative has no authority to unpause, ensuring that only the multisig can restore normal operations.
* Multisig Control:\
  The multisig retains full control over the unpausing mechanism, ensuring that a deliberate review is conducted before restoring the system to normal operations.

**Granular Control via the "IsFrozen" State**

In addition to the Paused State, which stops all protocol operations, Exactly Protocol introduced a more nuanced feature called the IsFrozen State. This state allows the protocol to limit new borrows and deposits while still permitting existing users to withdraw and transfer funds. This feature provides more flexibility during periods of heightened security while maintaining a degree of user engagement and liquidity access.

The IsFrozen State ensures that the protocol can remain functional for users while limiting new exposure to risk during an ongoing incident.

For detailed technical documentation and the source code of the Pauser Contract, please visit our[ GitHub Repository](https://github.com/exactly/protocol/blob/main/contracts/periphery/Pauser.sol).

