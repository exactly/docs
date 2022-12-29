# 🤖 Exactly's Bot

At Exactly, we developed our own open-source team-driven liquidation bot. You can check out the code here: [https://github.com/exactly/liquidation-bot](https://github.com/exactly/liquidation-bot).

We strongly advise and invite anyone interested in participating in Exactly's liquidations to fork from it and build on top.

### How does it work?

The liquidation bot analyzes the users' positions using the events emitted by the protocol with the minimum number of calls to the contracts, thus making the bot more efficient in recreating such states.

After connecting to the RPC provider through a WebSocket, the bot subscribes to receive the events stream.

Each one of those events is parsed and transcribed into the user's data.

Whenever there's an idle moment on receiving new events, the bot does a check for liquidations.

If a position is in a state to be [liquidated](../../getting-started/faq.md#what-is-a-liquidation) (with a [Health Factor](../../getting-started/faq.md#what-is-the-health-factor) less than 1), the liquidation function in the [flash loan contract](exactlys-bot.md#flash-loan-contract) is called.

**Important**: to avoid reentrancy issues on Uniswap's contract, the bot must pick a pair of assets that is **not** the same pair of debt/collateral.

After this call, the bot liquidates the user's debt seizing the collateral with the highest value.

After the liquidations, the bot waits for more events and recreates the user's positions.

#### Flash loan contract

The flash loan contract calls the liquidation function in the [protocol](../protocol/).

It checks its amount on the specific debt asset available on the contract's balance to repay the user's debt. In case it has less than the amount needed to liquidate the user, it does as follow:

1. Borrow on Uniswap V3 the difference between what it has and the user's debt
2. Waits for a callback notifying it that the amount was received
3. Repays the debt
4. Receives the collateral
5. Swaps it to the same as the user's debt
6. Repays Uniswap V3

### Structure

The project is structured as follows:

* `main.rs`
  * Setups the bot to connect correctly to RPC Provider.
  * Starts the service and handles most of the errors.
* `service.rs`: It's where most of the tasks are executed.
  * A subscription to the event's stream is made on the main thread.
  * Each event received is parsed.
  * Positions are created
  * A debounce for idleness is made in another thread.
  * When the bot is idle, this thread checks for liquidations
* `borrower.rs`: This structure is used to store users' data.
* `exactly_oracle.rs`: Helper to access price protocol used by the .protocol.
* `fixed_lender.rs`: Stores updated information created by the protocol's emitted events about all the markets.
* `exactly_events.rs`: Redirect the events to suitable structures.
* `config.rs`: Handle environment variables such as RPC provider link access, wallet's private key, etc.
