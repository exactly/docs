# 💲 Fixed Rate Operations

Users can supply and borrow assets to/from various Fixed Rate Pools depending on their time horizon preferences.

Each new deposit increases liquidity for that specific Fixed Rate Pool, reducing its utilization rate. Conversely, each new borrow takes out liquidity and increases the utilization rate. When there is a new operation (deposit/borrow) in a Fixed Rate Pool, interest rates are determined based on the state of the system at this moment. Nevertheless, credit demand (borrows) and supply (deposits) rates are calculated using different principles.

You can read more [here](../../resources/math-paper.md#4.1.1.-credit-demand-interest-rate-function).
