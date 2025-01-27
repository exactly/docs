---
description: >-
  This paper describes the v2 version of the model, which is currently deployed
  on the OP Mainnet.
---

# 🔣 Math Paper v2

## Exactly Interest Rate Model Upgrade, IRM-V2

Authors: [Francisco Lepone](https://github.com/FranciscoLepone)&#x20;

[math@exact.ly](mailto:math@exact.ly)

December 2024

## Abstract

One year after launching theExactly protocol on the Optimism Mainnet, we introduce an enhanced version of its interest rate model. This iteration, IRM-V2, is designed to increase capital efficiency, produce a smoother term structure of interest rates, stimulate demand for credit across extended maturities, and lay the groundwork for the rise of more end-user-oriented credit products currently unavailable in the DeFi ecosystem.

Consistent with its predecessor, IRM-V2 allows users to borrow and deposit at either floating or fixed interest rates. The determination of floating rates has been refined and now depends on the utilization level of the Variable Rate Pool in conjunction with the overall utilization of the Protocol. Meanwhile, fixed rates are determined through a spread term, which depends on the relative utilization across various maturities, and it is also linked to the áoating rate value at any given moment.

The protocol maintains all underlying logic related to calculating supply rates, whether floating or fixed, ensuring a cohesive and robust framework for interest rate management.

## 1. Introduction

The emergence of decentralized Finance (DeFi) has the potential to radically transform traditional financial systems, introducing unprecedented levels of accessibility, transparency, and efficiency. Within this innovative landscape, theExactly Protocol has made a significant contribution, especially in the domain of fixed interest rates. Currently, Exactly is the only platform in the Optimism ecosystem that enables users to deposit and borrow crypto assets at both variable and fixed interest rates. Unlike previous fixed-rate models that relied on creating and pricing numerous maturity tokens that required a special type of Automated Market Makers (AMMs) and faced challenges such as liquidity shortages,Exactly Protocol introduced a novel fixed interest rate discovery process. It resembles the methodology used by successful and well-proven variable-rate protocols \[refs] but extends it to the much more complex fixed-rate field. It never hurts to highlight that developing a robust fixed-rate market is essential for DeFi to satisfy real-world financial needs and achieve massiveness.

The original Interest Rate Model (IRM-V1)\[1] adopts a continuous and differentiable rational function of the utilization for setting lending rates instead of the linear model Compound Protocol introduced in February 2019\[2]. The function was designed to diverge asymptotically for a certain boundary value of utilization to act as a natural barrier for credit demand as the utilization level depletes the protocol liquidity capability. The model implemented one of such functions calibrated explicitly for each pool maturity offered to borrowers/depositors. The borrowing rate for a given maturity is thus determined by the degree of utilization in this specific pool.\


This paper introduces IRM-V2, an evolution of the Exactly Protocol's approach to interest rate determination. Building upon the experience gained from over 100,000 transactions on the Protocol since launching, IRM-V2 incorporates a refined methodology that better aligns with market dynamics and user behaviors.

Specifically, it introduces a global utilization metric into the rate function to optimize liquidity usage and improve capital efficiency across the platform. It also establishes a term structure for fixed rates. This adjustment is not just a technical update but a thoughtful response to the practical challenges and opportunities observed within the DeFi ecosystem over the past year.

On the Short-Rate side, improvements are intended to increase capital efficiency. The variable rate will depend on the variable pool utilization level (as in the previous model) as well as on the protocol global utilization (variable pool plus all fixed-rate pools combined). The new interest rate function now becomes a bivariate function, and its behavior allows a reduction in the reserve requirement.

On the Fixed Term Side, the model introduces a guideline for the term structure of interest rates. Fixed rates will now be linked to the short rate status and incorporate a spread term dependent on the relative utilization for each specific maturity.

It is worth noting that changes were designed so that users donít notice the transition from IRM-V1 to IRM-V2, at least for low to medium global utilization values. Only when global utilization levels are high do the new features gain all their e§ectiveness, preventing liquidity issues.

## 2. Revisiting Pool Utilization Definitions

We introduce some changes in the definitions of utilization with respect to the ones applied in IRM-V1. In this new version, the common numeraire for any utilization is the total amount of deposits at variable rates. Since we have not modified the Protocol logic, we still have a single utilization for the variable rate pool (which measures the demand for instant-rate loans) and specific utilizations for each maturity pool, which measures the individual demand for each fixed-rate loan term. In addition, we will define a global utilization that characterizes total Protocol credit demand.

Consider the following time-dependent quantities: $$TD_{VR}$$ is the total amount of deposits at variable rate (i.e., the protocol liquidity source), $$TB_{VR}$$ is the total amount of loans at variable rate, $$TD_{FR}^T$$  and $$TB_{FR}^T$$ are the total amount of deposits and loans at maturity $$T$$, respectively.

We define the variable pool utilization as,

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.31.00.png" alt=""><figcaption></figcaption></figure>

and the utilization of a fixed pool with maturity $$T$$ as

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.31.47.png" alt=""><figcaption></figcaption></figure>

Note that with this definition, only loans backed by deposits in the floating pool add to the utilization.

Finally, the Protocol global utilization ($$U_{Liq}$$) is simply the summation of all the other ones and denotes the degree of liquidity usage.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.32.47.png" alt=""><figcaption></figcaption></figure>

## 3. Modelling the Short Rate (Variable Rate)

It is well known that liquidity preservation and capital efficiency are key points to consider when designing an autonomous interest rate system. Liquidity is a big concern, but only when global utilization (variable plus fixed pools together) is high. Otherwise, it should have little influence on interest rate determination. Concentrating liquidity resources helps to improve capital efficiency by avoiding segmentation but imposes new management challenges when there are multiple objectives to be satisfied (as is the case of multiple maturities in a term structure of interest rates). Therefore, Pools should care about global liquidity as well as their individual utilization level. By introducing a double dependency on variable-pool and global utilization levels, together with a liquidity triggering mechanism (as in the previous model), it is possible to have better control of rate adjustments. The immediate consequence is the chance to reduce liquidity reserve requirements, freeing up additional capital to be lent.

As already mentioned, the goal of an improved short-rate model is to make it also dependent on the protocolís global utilization. This dependency on global utilization should be ideally relevant only when available liquidity is scarce. For low to medium utilization levels, this e§ect should be almost negligible, and the old model would work perfectly fine.

A way to incorporate these features is to add a modulation factor to the old specification of IRM-V1. This factor takes the form of a rational function of $$U_{Liq}$$ with a switching mechanism that turns this dependency on/off according to the value of $$U_{Liq}$$ . It is essential for this transition to be continuous and as smooth as possible. This is achieved by incorporating a sigmoid function as a switching mechanism.

The functional form for the short rate is thus given by:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.36.34.png" alt=""><figcaption></figcaption></figure>

where

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.37.22.png" alt=""><figcaption></figcaption></figure>

and

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.38.09.png" alt=""><figcaption></figcaption></figure>

In the above expressions $$A$$, $$B$$, $$U_{max}$$, $$\alpha$$, $$U_{Liq0}$$ and $$k_{sig}$$ are constant parameters serving different purposes. {$$A$$.$$B$$.$$U_{max}$$} are intended for calibrating the behavior of the surface rate in the low to medium global utilization range; {$$\alpha$$} controls the steepness for rate increase in the high global utilization range; {$$U_{Liq0}$$.$$k_{sig}$$ } determine the utilization level where the transition occurs and its shift speed, respectively.

## 4. Modelling Fixed-Term Rates

Concerning fixed-rate determination as a function of loan maturity, IRM-V2 replaces the scheme of multiple curve functions (each one ruled by the individual pool utilization) with a spread term regulated by the relative usage of each maturity with respect to a predefined natural allocation level, see Fig.4. This approach makes also possible to incorporate other market characteristics such as intertemporal preferences.

One advantage of this novel approach is that rates will show more parsimonious behavior across terms while still reflecting user preferences. In some sense, it is a way to incorporate the benefits that monetary-policy guidelines bring to traditional finance, but in an autonomous way.

At every moment, Protocol users can freely choose between taking loans with an unspecific time horizon (floating rate) or with specific repayment dates (fixed rates).

To formalize these ideas, we introduce a parameter $$\nu$$ that conceptually affects the natural ratio between floating and fixed loans outstanding volume expected (conversely, 1 - $$\nu$$  is the natural proportion for fixed loans). As an example, a value .$$\nu$$ = 0:4 would mean that 40% are expected to be ideally allocated to variable-rate loans and 60% to fixed-rate loans. Having a proportion of floating debt above/below $$\nu$$ indicates that the former are over/under-demanded, respectively.

We define the indifference utilization point for any maturity pool as the average utilization value of a single fixed-rate pool:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.39.23.png" alt=""><figcaption></figcaption></figure>

Where $$n_{T}$$  is the number of existing maturities. From eq.(2.2) we know that:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.40.10.png" alt=""><figcaption></figcaption></figure>

Call $$\phi$$ the fraction of natural utilization being used in a specific maturity.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.41.15.png" alt=""><figcaption></figcaption></figure>

We model the spread term as:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 12.42.29.png" alt=""><figcaption></figcaption></figure>



where $$T$$  is time to maturity, $$T_{max}$$  is the time to the longest maturity pool, $$\eta$$ ,$$a_1$$ and $$a_0$$, are constant, and $$Z$$ ($$\phi$$ ) is a monotonic function (-1 ≤ $$Z$$ ($$\phi$$ ) ≤ 1) given by

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.07.20.png" alt=""><figcaption></figcaption></figure>

The expression for the interest rate term structure is then given by:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.08.49.png" alt=""><figcaption></figcaption></figure>

In the current version, factor $$a_0$$ commands the spread wideness. In addition, parameter $$a_1$$ allows the introduction of a time liquidity-preference premium. In the future, these features may evolve to be endogenous or dynamically calibrated based on actual user behavior.

Under this approach and assuming $$a_1$$ = 0, when $$U_{FR}^T$$ $$U_{FR}^T<⟨U_{FT}^T⟩$$ at a given maturity, the pool is under-demanded -  $$Z (U_{FR}^T<⟨U_{FT}^T⟩)<0$$ $$a_1$$ - so the spread is negative and the rate trades at discount over the floating rate. Conversely, $$U_{FR}^T<⟨U_{FT}^T⟩$$ the pool is over-demanded - $$Z (U_{FR}^T<⟨U_{FT}^T⟩)>0$$ - the and the spread is positive and the rate trades at a premium over the floating reference.

## 5. Results

This section presents the performance and behavior of IRM-V2 under different utilization levels and parameter calibrations. Key results include the smooth transition of short rates, the impact of parameters on rate growth beyond utilization thresholds, and the efficiency of the sigmoid switching mechanism in regulating liquidity constraints.

Figure(5.1), illustrates the short rateís sensitivity to global utilization beyond $$U_{Liq0}$$  as governed by the $$\alpha$$  parameter. The results demonstrate that higher values steepen the rate growth curve, limiting borrowing at high utilization levels without impacting low-to-moderate utilization scenarios.

For illustrative purposes, we took on particular case $$U_{Liq0}$$ = 0:75; $$K_{sig}$$ = 2:5. By adjusting, it is very easy to make loans so costly to repay that no practical transactions will occur beyond the desired utilization limit. This can happen without affecting the normal use of the protocol at lower utilization levels.\


<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 17.49.19.png" alt=""><figcaption></figcaption></figure>

Figure(5.2) shows the switching mechanism behavior for chosen combinations of { $${U_{Liq0},k_{sig}}$$ } These range from a low utilization-low speed transition case { $${U_{Liq0}=0.5,k_{sig}}=2$$ }  to a high utilization-fast speed transition case { $${U_{Liq0}=0.8,k_{sig}}=20$$ }.&#x20;

Figure(5.3) shows the general aspect of the short rate surface. This surface smoothly increases as the variable pool utilization gets higher; and shows a bigger steepness in the direction of global utilization. In fact, the calibration is set so that for most of the utilization space, rates are well contained and only increase sharply beyond the transition point.



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 17.54.39.png" alt=""><figcaption></figcaption></figure>

Figure(5.4) exhibits the variable rate behavior as a function of global utilization for different variable rate pool utilization levels.

Figure(5.5) exhibits the variable rate behavior as a function of the variable rate pool utilization for different levels of global utilization. It is evident that curves are notoriously smoother along this dimension.

The fixed-rate determination depends on the combination of floating rate levels and spread terms. Fig(5.6) shows the range they can adopt as a function of time to maturity depending on the relative utilization of each fixed rate pool. The black line shows the indifference point where fixed pool utilizations align according to the expected natural distribution. Below this line, pools are under-demanded, so interest rates are lower, encouraging new loans. Above the line, pools are over-demanded, rates are higher, and users will tend to get cheaper debt from other maturities.

As Fig(5.7) shows, the model is very flexible and can adopt different config durations depending on the parameterization. In its initial version (as previously mentioned), parameters will be exogenous but could be adapted to change dynamically, reflecting agents' preferences.

## 6. Applications

The ultimate goal of the Exactly Protocol is to bridge the gap between the current status of DeFi and the development of practical solutions that directly benefit real-world end users. This new interest rate framework allows for the exploration of innovative applications in personal and commercial finance, such as structuring installment loans with longer terms and predetermined fixed financial costs, enabling users to plan their finances with greater certainty. Another potential application the model facilitates is the creation of a credit card instrument that empowers users to defer payments into a self-determined number of installments at competitive financial costs, enhancing áexibility and control over personal spending. Furthermore, it supports the design of loans with payment schedules tailored to the user's cash flow patterns. For instance, in activities characterized by strong seasonality, repayment structures can concentrate payments during periods of higher income, thereby aligning financial obligations with revenue generation cycles. These practical implementations are just a few examples of the model's adaptability and potential to address diverse needs effectively.

<figure><img src="../.gitbook/assets/Screenshot 2025-01-23 at 7.36.44 PM.png" alt=""><figcaption></figcaption></figure>

From a conceptual point of view, any application would be the result of solving some version of the problem:

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 18.00.37 (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.25.25.png" alt=""><figcaption></figcaption></figure>

where $$ps$$ and $$ls$$ are the payment and loan stream at every maturity, IRM is the protocol interest rate model, $$x$$ is a vector describing the status of the protocol. and $$H(),g().l()$$ are functions that shape the particular application of interest. In general, a recursive algorithm is needed in order to solve problem (6.1).

## 6.1 Case 1. Periodic Installment Fixed Rate Loans - Bul- let Bonds

This example demonstrates how a multiple-installment fixed-rate loan can be structured. To simplify, suppose there is a total supply of 10 million USDC in the market, with an initial global utilization rate of 0:5. The floating rate utilization stands at 0:2, while the utilizations of the first six available pools are {0:070; 0:078; 0:01; 0:078; 0:054; 0:01}, respectively. In this scenario, a loan of 2 million USDC is requested, which will be repaid in six equal installments.

The structure involves distributing the total loan amount into partial loans across the available maturities, ensuring the sum reaches the desired 2 million USDC.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 18.04.41.png" alt=""><figcaption></figcaption></figure>

Figure (6.1) illustrates the size of partial loans and the equal repayment stream. Given the system status, this amounts to 338;200 USDC per installment, providing a transparent and predictable financial plan for the borrower.

Figure (6.2) shows the rates at which each partial loan was issued, offering insights into the cost dynamics over time. Additionally, the implied yield for the total loan is displayed, reáecting the global funding cost.

Finally, Figure (6.3) highlights the evolution of utilization levels across maturities, showing both the individual pool utilizations and the aggregated global utilization before and after the loan issuance.&#x20;

This structured approach ensures transparency and alignment between borrower needs and market conditions, showcasing the practical applicability of the Exactly Protocol's interest rate model in real-world lending scenarios. By customizing loans to existing pool conditions, borrowers and lenders can achieve an optimal balance between cost efficiency and resource allocation.&#x20;

Furthermore, these ideas could be expanded to include the issuance of debt instruments such as bullet bonds, which could have significant potential for funding projects initiated by DAOs within the ecosystem. Such debt instruments could be actively traded in secondary markets or decentralized exchanges, with the Exactly Protocol serving as a consistent market maker to ensure liquidity. While substantial development is still required, one can envision a future phase where these debts are securitized, benefiting from risk diversification and offering additional value to investors and the ecosystem as a whole.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 18.07.15.png" alt=""><figcaption></figcaption></figure>

### 6.2 Case 2. Credit Card Issuance with a Flexible Payment Schedule

As the first concrete end-user application of the Exactly Protocol borrow and lending framework, we have created the first self-custodian credit card system that allows users to manage their purchases with unmatched flexibility and control\[3]. By integrating the protocol with a smart wallet, users can defer payments for any purchase into a self-determined number of installments at competitive fixed rates and predefined terms. This setup eliminates the uncertainty associated with áuctuating financial costs, enabling users to plan their expenses confidently and efficiently. In this framework, each purchase is converted into a loan structured directly through theExactly Protocol. Users can select the number of installments that best fit their financial situation, benefiting from a transparent and predictable repayment schedule. The smart wallet acts as a seamless interface, automating the interaction with the protocol and managing repayments without requiring intermediaries, ensuring complete control remains with the user. This approach not only enhances user autonomy but also unlocks broader accessibility to decentralized financial services. It provides a scalable solution for integrating DeFi principles into everyday financial tools, helping to bridge the gap between traditional credit systems and blockchain technology. By leveraging the Exactly Protocolís robust fixed-rate lending infrastructure, this application represents a significant step forward in reimagining consumer finance in the digital age.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.30.18.png" alt=""><figcaption></figcaption></figure>

### 6.3 Case 3. Tailor-Made Loans, Deferred and Seasonal Payments

Another example of the Protocolís potential is its application in providing tailored financial solutions for producers or entrepreneurs needing funding for their productive activities. Such users often face the challenge of financing projects that involve a significant time lag before generating returns. In these cases, the ability to design a loan with áexible terms that align with the user's cash flow becomes a critical enabler for business growth and operational efficiency.

The Protocol's unique borrowing and lending logic facilitates the structuring of loans with payment schedules that are not only delayed to accommodate the gestation period of the investment but also aligned with seasonal income patterns. For instance, an agricultural producer investing in crop cultivation might need funding at the start of the planting season but would only begin to realize income after the harvest. Similarly, a tourism-focused entrepreneur might generate the bulk of their revenues during peak seasons, necessitating a repayment structure concentrated in those periods.

By leveraging the Exactly Protocol, borrowers can obtain fixed-rate loans that defer initial payments to match their projected cash ináows and subsequently adjust repayment schedules according to their income cycles. This approach not only reduces financial strain during low-income periods but also minimizes the risk of default, ensuring the sustainability of both the borrowerís business and the lending ecosystem.

As a numerical example, let's reproduce the figures of Case 1, now with a time horizon of 24 monthly maturities. This time, the user wants to start repaying the loan one year after receiving the funds, matching payments with high-income periods (months 12 to 15 and 18 to 21), totaling eight installments. Figure(6.4) illustrates the proportion of partial loans and the equal repayment stream.

<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.31.59.png" alt=""><figcaption></figcaption></figure>

Figure(6.5) shows the rates at which each partial loan was issued and the implied yield for the total loan. Finally, Figure(6.6) highlights the evolution of utilization levels across maturities, showing both the individual pool utilizations and the aggregated global utilization before and after the loan issuance.

The integration of smart contracts within the protocol enables the automation of such tailored repayment structures, eliminating the need for manual renegotiations or interventions. Furthermore, the transparency and predictability of fixed-rate lending provide entrepreneurs with a clear understanding of their financial obligations, enhancing their ability to effectively plan and execute their investment strategies.

This application demonstrates the versatility of the Exactly Protocol in addressing the diverse needs of users across different industries. By aligning financial products with the realities of income variability and investment timelines, the protocol empowers users to unlock new opportunities, fostering innovation and economic growth within decentralized ecosystems.

## 7. Final Remarks

This paper introduced IRM-V2, an upgraded version of the Exactly Protocolís interest rate model, designed to enhance capital efficiency, liquidity management, and the usability of fixed-term borrowing with the objective of bringing the end user to decentralized finance. By incorporating a global utilization metric alongside individual pool utilization, the model offers a more comprehensive approach to rate determination, ensuring that liquidity constraints are effectively managed only at high utilization levels. By reducing reserve requirements, the protocol frees additional liquidity for lending, improving overall capital productivity. The model introduces a smooth and continuous transition



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.32.59.png" alt=""><figcaption></figcaption></figure>

mechanism, ensuring predictable rate increases as global utilization approaches critical levels. The novel spread term, dependent on relative utilization and intertemporal preferences, creates a more parsimonious and responsive term structure for fixed rates.

The results demonstrate that IRM-V2 can optimize liquidity allocation while maintaining stability across both variable and fixed-rate products. These enhancements pave the way for innovative financial applications, including tailored credit instruments such as installment loans, áexible credit card systems, and deferred payment solutions for seasonal businesses. We believe this represents a significant step toward addressing real-world financial needs within the DeFi ecosystem.

Looking forward, the agenda includes exploring the implementation of a dynamic calibration of parameters based on user behavior and market conditions, improving the protocolís adaptability. Additionally, it sets the basis for the introduction of new derivative debt-based instruments and securitization mechanisms, fostering broader adoption and integration of decentralized finance solutions.

## References

\[1] Francisco Lepone, Gabriel Gruber, Exactly Protocol: A Model to Complete the Credit Market on the Ethereum Blockchain, [https://docs.exact.ly/resources/math-paper](https://docs.exact.ly/resources/math-paper)

\[2] Compound (2019), [https://compound.Önance/docs](https://docs.compound.finance/)



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.34.07.png" alt=""><figcaption></figcaption></figure>

## Disclaimer

This paper is only for general information purposes. It does not constitute investment advice or a recommendation or solicitation to buy or sell any investment instrument and should not be used in the evaluation of the merits of making any investment decision. It should not be relied upon for accounting, legal or tax advice, or investment recommendations. This paper reflects the current personal opinions of the authors and is subject to change without being updated.



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.34.54.png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.35.33.png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/Captura de pantalla 2025-01-17 a las 13.36.10.png" alt=""><figcaption></figcaption></figure>

## Download the Math Paper v2

{% embed url="https://github.com/exactly/papers/blob/main/ExactlyMathPaperV2.pdf" %}

* [\[EXAIP-08\] Interest Rate Model Upgrade (IRM v2)](https://medium.com/@exactly_protocol/exaip-08-interest-rate-model-upgrade-irm-v2-16759de270e6)
