# 🔢 Parameters

There is an explanation of the following parameters in [Model Parameters](../getting-started/math-paper.md#model-parameters).

## A. Reserve Factor

$$
\begin{align*} \eta = 10\% \end{align*}
$$

$$\eta$$ is the fraction of the total Variable Rate Pool supply selected as Liquidity Reserve.

## B. Treasury Fee

$$
\begin{align*} \lambda_r = 0\% \end{align*}
$$

## C. Interest Rate Curves

|                     | WETH    | DAI     | USDC    | WBTC    | wstETH  |
| ------------------- | ------- | ------- | ------- | ------- | ------- |
| Variable Rate Pools |         |         |         |         |         |
| $$A$$ =             | 0.0137  | 0.0085  | 0.0090  | 0.0438  | 0.0200  |
| $$B$$ =             | 0.0040  | 0.0066  | 0.0060  | -0.0330 | -0.0023 |
| $$U_{max}$$=        | 1.0091  | 1.0081  | 1.0060  | 1.0173  | 1.0133  |
| Fixed Rate Pools    |         |         |         |         |         |
| $$A$$=              | 0.3143  | 0.3758  | 0.3236  | 0.3468  | 0.3143  |
| $$B$$=              | -0.3008 | -0.3582 | -0.3136 | -0.3417 | -0.3008 |
| $$U_{max}$$=        | 1.0033  | 1.0072  | 1.0002  | 1.0003  | 1.0033  |

## D. Risk Factors

|                     | WETH   | DAI    | USDC   | WBTC   | wstETH |
| ------------------- | ------ | ------ | ------ | ------ | ------ |
| $$ho$$(borrow/lend) | 0.8400 | 0.9000 | 0.9100 | 0.8500 | 0.8200 |

## E. Variable Rate Pool Fee

$$
\begin{align*} \delta = 10\% \end{align*}
$$

$$\delta$$ is the fraction of term-loan interests retained by the Variable Rate Pool upon leaving the Fixed Rate Pool.

## F. Supply E.M.A. Parameters

$$
\begin{align*} \beta_{slow} = 0.0046 \end{align*}
$$

Time decay parameter is used when the supply is above average.

$$
\begin{align*} \beta_{fast} = 0.4000 \end{align*}
$$

Time decay parameter is used when the supply is below average.

## G. Target Solvency Ratio

$$
\begin{align*} \Gamma = 1.25 \end{align*}
$$

Target solvency ratio after liquidation.

## H. Liquidation Bonuses

$$
\begin{align*} \nu_{liquidator} = 5.00\% \\ \nu_{bad-debt} = 0.25\% \end{align*}
$$

## I. Extraordinary Earnings Distribution Factor

$$
\begin{align*} \xi_{extearn} = 2.00 \end{align*}
$$

## J. Penalty Rate

$$
\begin{align*} 2.00\% \end{align*}
$$

Daily penalty rate charged to late fixed repayments.
