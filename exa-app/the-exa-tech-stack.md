---
icon: network-wired
---

# The Exa Tech stack

There are three different layers that constitute the Exa Tech Stack that allows us to provide a Web2 UX while getting the cutting edge of the Web3 in our users hands.

<figure><img src="../.gitbook/assets/Screenshot 2025-06-11 at 7.57.04 PM.png" alt=""><figcaption></figcaption></figure>

Lets go layer by layer:

* **Exactly Protocol** is the core, it's a decentralized protocol that allows users to deposit assets to earn yield while acting as collateral and borrow at fixed interest rates that can be repaid in installments.
* **Alchemy Account Kit, The Webauthn Plugin and the Exa Plugin** are in the middle, and they the tools that make the app easy to use.&#x20;
  * Alchemy account kit: A modular smart account that permits abstraction of several processes and reduces the friction with the users. Besides, sets the basis for the plugins, which are modules of the modular accounts.
  * [Webauthn Plugin](https://docs.exact.ly/exa-app/the-exa-app-webauthn-owner-plugin): the main function is to create a more adaptable wallet system where the users can benefit from features traditionally associated with externally owned accounts (EOA) or smart contracts without needing to manage complex private keys. This allows the Exa App users to sign with passkeys or/and with other Ethereum wallets.&#x20;
  * [Exa Plugin](https://docs.exact.ly/exa-app/exa-plugin): enables users to borrow, lend, and manage credit and debit transactions through a self-custodial smart account, in other words, it ensures a smooth DeFi experience, automating key financial operations while maintaining security and efficiency.
* **Exa App:** This is the application the user sees and interacts with. It is designed with a familiar and intuitive interface, similar to today's popular fintech and banking apps. This provides a user-friendly experience, masking the powerful and complex technology that runs underneath. The required integrations to go through the KYC (Persona), issue the Exa Cards (Rain) and communicating with customer support (Intercom) are done in the Exa App's backend.

In summary, the Exa App architecture leverages advanced, secure technology on the back end to deliver a simple, safe, and powerful financial experience to its users.

