# Rate Manipulation Prevention

As the [Math Paper](../../resources/math-paper.md#4.1.3-time-averaged-variable-rate-pool-supply) explains, when calculating the utilization rate for fixed borrows, an average of the variable pool deposits (`floatingAssetsAverage`) is passed to the [InterestRateModel](../protocol/interestratemodel.md). The reason behind this is to prevent manipulation of a fixed borrow rate: a user could deposit a significant amount in the variable pool to lower the utilization, ask for a considerably cheap fixed borrow and then withdraw the initially deposited amount.

The `floatingAssetsAverage` is updated with the use of damp speeds to smoothly increase or decrease the value that the actual variable pool deposits (`floatingAssets`) have through a short period of time. With this approach, for example, if a user deposits to the variable pool and wants to ask for a fixed borrow in the same transaction, the average will still account for the outdated value.
