# Liquidity Reserve

By design, the protocol sets aside a percentage of the variable rate pool deposits as liquidity reserves.

These reserves cannot be borrowed so the protocol always ensures that there's an available underlying amount for withdrawals. This can be adjusted over time given different market conditions through the [Reserve Factor](../parameters.md#a.-reserve-factor) (a parameter that can be found in each [Market](../protocol/market/) contract).
