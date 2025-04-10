---
description: Powering onchain credit, debit and lending in the Exa App
icon: robot
---

# Exa Plugin

## Overview

The Exa Plugin is a modular smart contract designed exclusively for the Exa App, providing seamless interaction with the Exactly Protocol. This integration enables users to borrow, lend, and manage credit and debit transactions through a self-custodial smart account. By leveraging ERC-6900 modular accounts, the Exa Plugin ensures a smooth DeFi experience, automating key financial operations while maintaining security and efficiency.&#x20;

[Quantstamp](https://quantstamp.com/) audited the Exa Plugin to verify its adherence to high-security standards. By integrating this plugin, Exa App users can:\


* Deposit collateral and take loans within Exactly Protocol.
* Execute credit or debit transactions without constant manual approvals.
* Automate key lending and borrowing processes using a structured role system.

### Key Characteristics: 

* Automated lending and borrowing: The plugin facilitates collateral management and repayments through Exactly Protocol without requiring user intervention for each action.
* WebAuthn integration: Users authenticate transactions using biometrics, eliminating the need for seed phrases.
* Keeper role automation: The keeper bot assists in executing operations like enabling collateral, processing loans, managing repayments, and handling proposals.

### Roles and Responsibilities

The Exa Plugin defines specific roles to enforce structured execution logic, ensuring secure and automated transactions.

**Admin Role (DEFAULT\_ADMIN\_ROLE)**

The admin role is designed to manage critical plugin settings and role assignments.

Key functions:

`setIssuer()` – Assigns the issuer responsible for transaction approvals.

`setOperationExpiry()` – Configures the expiry time for transaction authorization.

`setPrevIssuerWindow()` – Defines the time window for recognizing previous issuers.

grantRole(role), setFlashLoaner, setCollector, setProposalManager,setSwapper, allowPlugin

**Keeper Role (KEEPER\_ROLE)**&#x20;

The KEEPER\_ROLE is assigned to an automated entity responsible for executing transactions that require protocol enforcement. This role interacts with Exactly Protocol’s lending pools and ensures credit-related actions comply with liquidity constraints and proposal validations.

Functions restricted to KEEPER\_ROLE:

`collectCredit()` – Executes on-chain borrowing for credit transactions.

`collectCollateral()` – Moves collateral from external markets into Exactly Protocol.

`collectInstallments()` – Processes installment-based credit repayments.

`collectDebit()` – Handles direct debit operations to repay borrowed amounts.

`poke() / pokeETH()` – Marks assets as collateral and updates liquidity status.

`repay() / crossRepay()` – Facilitates repayment and refinancing of loans.

`executeProposal()` – Executes time-locked proposals submitted by the user.

These functions enforce risk management mechanisms, prevent unauthorized withdrawals, and ensure the proper execution of credit-related workflows.

**Issuer Role (IssuerChecker)**

The issuer is responsible for validating and authorizing transactions within the Exa Plugin. This role ensures that credit transactions and refunds are approved before execution, enforcing additional security measures.

The issuer operates through the IssuerChecker contract, which verifies and signs operations related to credit payments and refunds.

The issuer is responsible for:

`checkIssuer()` – Verifies that a credit or refund transaction is properly signed and authorized.

### Core Smart Contracts

The Exa Plugin consists of multiple smart contracts, each playing a crucial role in enabling lending, credit payments, and refunds.

**1. Exa Account Factory (ExaAccountFactory.sol)**

This contract creates and initializes modular accounts for users, integrating both the WebAuthn owner plugin and the Exa Plugin.

Deploys accounts with pre-configured plugins for WebAuthn authentication and on-chain lending.

Uses `donateStake()` to add stake in the EntryPoint contract.

Handles the initialization of accounts, ensuring all required plugins are installed.

**2. Exa Account Interface (IExaAccount.sol)**

Defines the core lending, repayment, and proposal functions used by the Exa Plugin.

Supports borrowing, collateral management, token swaps, and repayments.

Implements liquidity and risk management constraints.

Introduces a proposal system to delay execution and validate intent.

**3. Installments Previewer (InstallmentsPreviewer.sol)**

A read-only contract that calculates expected loan installments based on market conditions.

Fetches floating and fixed-rate borrowing data from Exactly Protocol.

Evaluates utilization rates for risk assessment.

Helps users preview their installment plans before borrowing.

**4. Issuer Checker (IssuerChecker.sol)**

Handles issuer validation and transaction approval.\
Uses ECDSA signature verification to authenticate transactions.

Maintains an operation expiry window to prevent replay attacks.

Ensures only authorized issuers can approve transactions.

**5. Refunder (Refunder.sol)**

Processes approved refunds for users.

Interacts with Exactly Protocol to deposit refunded assets.

Uses IssuerChecker to validate refund requests.

Implements role-based access control to restrict refund execution.

Security Considerations\



* Restricted function execution: Only approved functions can interact with the Exactly Protocol.\

* Proposal-based withdrawals: Ensures sufficient collateral remains locked before allowing withdrawals.\

* Execution hooks: Enforces execution logic through runtime validations and prevents unauthorized calls.\

* Plugin allowlist: Only approved plugins can be installed or swapped, reducing attack vectors.\

* Flash loan risk protection: Credit repayment with flash loans is validated atomically to avoid misuse.

[Ex**a Plugin GitHub repository**](https://github.com/exactly/mobile/blob/368df252a3db7b2e370f1ed0af8db0939b45138e/contracts/src/ExaPlugin.sol)

[**Exa Plugin Audit**](https://github.com/exactly/audits/blob/main/Quantstamp%20Exa%20App%20Plugin%20\(Mar-25\).pdf)

