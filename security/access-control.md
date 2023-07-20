# 🔑 Access Control

## Roles & Privileges

There are a few roles to be aware of in the system. Each role has a set of privileges that are associated with it.

### Admin - Timelock Controller

The `TimelockController` smart contract acts as the `administrator` role in the protocol. It implements a governance mechanism called a timelock, which delays the execution of transactions to allow for evaluation of their potential impact before proceeding. The contract has two distinct roles: the `proposer`, who submits transactions, and the `executor`, who executes them after the timelock period expires. The `executor` is a [multisig contract](https://app.safe.global/home?safe=oeth:0xC0d6Bc5d052d1e74523AD79dD5A954276c9286D3), which means it requires multiple signatures from different signers. The number of signatures required for a transaction to be executed is defined in the multisig contract, and it's currently 3 out of 5 (2 signers are team members, and 3 are respected members of the DeFi community), providing a secure and transparent method of controlling execution. The timelock period is 24 hours, enabling quick and flexible decision-making by the protocol's administrators.&#x20;

### Pauser

The pauser role is responsible for temporarily suspending certain operations in emergency situations. The pausable operations are `deposit`, `borrow`, `repay` and `liquidate`. These operations may be paused by the same `executor` multisig to protect the system and its users in an emergency. However, the `withdraw` function will always remain active, allowing users to access and withdraw their funds at any time.

## Upgradeable Contracts

The following contracts are upgradeable and can be changed by the `TimelockController`:

* [Auditor](../guides/protocol/auditor.md)
* [Market](../guides/protocol/market/)
