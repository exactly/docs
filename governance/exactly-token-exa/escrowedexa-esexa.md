# ⚪ EscrowedEXA (esEXA)

**EscrowedEXA Smart Contract Address (OP Mainnet):** [`0xbea586A167853ADddEF12818f264f1F9823fBc18`](https://optimistic.etherscan.io/address/0xbea586A167853ADddEF12818f264f1F9823fBc18)

The [EscrowedEXA](https://www.youtube.com/watch?v=RGE4U6os4sw) contract is an ERC-20 token that allows anyone to mint esEXA tokens in exchange for EXA tokens. The esEXA tokens are only transferable for accounts with a TRANSFERER\_ROLE, reserved for the protocol contracts to integrate smoothly.

The idea behind esEXA is to provide rewards equivalent to EXA but with a linear vesting period, gradually releasing EXA tokens, ensuring that the Exactly protocol remains sustainable and rewarding for long-term community members.

## Vesting

EscrowedEXA (esEXA) tokens can be converted into EXA tokens through vesting in a 1:1 ratio.

Elements for the esEXA to EXA conversion process:

* Burning Mechanism: Holders will burn their esEXA tokens to initiate the vesting period and convert them into EXA tokens through [Sablier](https://sablier.com/).
* EXA Reserve: Holders must provide a “reserve” in EXA proportionally to the esEXA they want to vest. This proportion is represented in the EscrowedEXA contract as \`reserveRatio\`, the “reserve” EXA tokens to start the vesting process. The reserve percentage is 35% and can be changed through governance.

### Steps

* Step 1: Claim your esEXA Rewards&#x20;
* Step 2: Initiate the vesting of your esEXA. You must deposit 35% of the total esEXA you want to vest as an EXA reserve.&#x20;
* You can get EXA [here](https://app.exact.ly/get-exa) if you don’t have the required reserve amount.

Users can cancel the vesting at their discretion, triggering the withdrawal of all vested EXA tokens from Sablier and the associated reserve. Any unvested tokens are returned to the EscrowedEXA contract, where the corresponding esEXA tokens are minted and returned to the user.

This mechanism guarantees that the exclusive path for the users to retrieve reserved EXA tokens from the EscrowedEXA contract is by adhering to the stipulated vesting schedule.

### **Withdraw Reserved EXA**&#x20;

When you withdraw the reserved EXA, the associated vesting streams will be canceled automatically. You’ll get back the earned EXA and all remaining esEXA.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2024-07-15 a las 17.23.14.png" alt=""><figcaption><p><a href="https://app.exact.ly/vesting">Active Vesting Streams</a></p></figcaption></figure>

**GitHub URL:**

[https://github.com/exactly/protocol/blob/main/contracts/periphery/EscrowedEXA.sol](https://github.com/exactly/protocol/blob/main/contracts/periphery/EscrowedEXA.sol)

**EXAIP-01 Transitioning to EscrowedEXA rewards (esEXA):** [https://gov.exact.ly/#/proposal/0x889d08cbe0ed7be4fd437ca374ef2845b4dbd641a6d2c57e76cd2c54d47fcadc](https://gov.exact.ly/#/proposal/0x889d08cbe0ed7be4fd437ca374ef2845b4dbd641a6d2c57e76cd2c54d47fcadc)

**EXAIP-06 Increasing the esEXA Reserve:** [https://gov.exact.ly/#/proposal/0xaa0fa1ea69371b6ba475962b488970778761be8b36427a08af5194264a573178](https://gov.exact.ly/#/proposal/0xaa0fa1ea69371b6ba475962b488970778761be8b36427a08af5194264a573178)

&#x20;
