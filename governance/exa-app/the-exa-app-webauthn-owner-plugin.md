---
icon: robot
---

# The Exa App Webauthn Owner Plugin

In collaboration with [Circle](https://www.circle.com/) and [Alchemy](https://www.alchemy.com/) and audited by [Quantstamp](https://quantstamp.com/), the Exactly Protocol team has developed the [Webauthn Owner Plugin](https://github.com/exactly/webauthn-owner-plugin) to integrate passkeys into the Exa App and create a seamless self-custodial experience for Exa App users.

* [Exa App Webauthn Owner Plugin audit](https://certificate.quantstamp.com/full/exactly-web-authn-owner-plugin/195741fd-c62b-4a88-87b8-020dd454bd59/index.html)

The plugin supports multiple verification methods, including:

* ECDSA signature verification: This is the standard used by most externally owned accounts (EOA) on Ethereum.
* ERC-1271 signature verification: This allows smart contracts to verify signatures, providing flexibility for different types of accounts.
* WebAuthn and P256 passkey signature verification: Thanks to the RIP-7212 standard, P256 passkey signatures are now fast, secure, and cheaper to verify.

Thanks to the recent Fjord network upgrade on Optimism, the RIP-7212 standard was introduced. This enables P256 signature verification to be 20 times cheaper than before, significantly reducing gas fees for passkey-based authentication. With RIP-7212, passkeys become not only more secure but also cost-effective for daily use in decentralized applications like the Exa App.

The Webauthn Owner Plugin uses the RIP-7212 precompiled contract to verify P256 passkey signatures at a significantly reduced gas cost.

This plugin also implements EIP-712, a standard for structured data signing in smart contracts, which makes transaction approval more user-friendly and secure. Together, these features ensure that users can interact with DeFi without needing to validate every single on-chain action manually—improving the overall user experience.

### ERC-6900: Modular Accounts for a better Web3 experience

The Webauthn Owner Plugin is built on the ERC-6900 standard, which introduces a modular approach to [smart contract accounts](https://www.alchemy.com/account-contracts). This standard allows for greater flexibility in handling authentication and account management. With ERC-6900, developers can create reusable plugins that integrate advanced features like passkey verification or multi-owner accounts.

For users, this means a more adaptable wallet system where they can benefit from features traditionally associated with externally owned accounts (EOA) or smart contracts without needing to manage complex private keys.&#x20;

This modular approach allows developers to implement account features, like biometric authentication or multi-owner access, without needing to manage or expose sensitive private keys, making DeFi more secure and user-friendly.

### What does this mean for the Exa App users?

The Exa App is designed to remove the complexities of DeFi and provide a smooth, user-friendly experience. Here’s how the app benefits users:

* Self-custodial wallet: Users maintain full ownership of their funds at all times. There is no reliance on third parties, and the private key is never exposed, providing the highest level of security.
* Simplicity: Instead of managing a 12-word recovery phrase, users can easily access their wallet through biometric authentication, removing the technical barriers typically associated with Web3.
* Cost-effective transactions: With RIP-7212, P256 passkey signature verification is 20 times cheaper, significantly reducing gas fees for authentication on supported Layer 2 networks like Optimism.
