# Empirical Analysis of Ethereum Network Activity and Transaction Growth

## 1. Objective

This analysis investigates the relationship between Ethereum quarterly transaction activity and three potential explanatory variables: active addresses, average ETH price (USD), and average gas price (Gwei). The objective is to identify which variables are most strongly associated with changes in transaction activity.

The dataset covers 44 quarterly observations from Q4 2015 to Q3 2026. Growth-rate models use 43 observations because the first difference removes the initial quarter.

## 2. Regression methodology

The analysis initially estimated a log-linear regression using quarterly transaction counts, active addresses, ETH price, and gas price.

The levels regression produced an R² of 0.987. However, its Durbin–Watson statistic was 0.722, and the Breusch–Godfrey test indicated significant residual autocorrelation. A preliminary ADF test on its residuals returned a p-value of 0.516. This result does not establish a valid long-run relationship; a formal Engle–Granger cointegration test would be required for that conclusion.

Consequently, the analysis proceeded using quarterly log differences to investigate short-term growth relationships.

The three-predictor growth model was specified as:

Δlog(Transaction Count) = β₀ + β₁Δlog(Active Addresses) + β₂Δlog(ETH Price) + β₃Δlog(Gas Price) + ε

HC3 heteroskedasticity-robust standard errors were used for statistical inference.

## 3. Three-predictor regression results

The growth-rate regression explained 87.2% of the variation in quarterly transaction growth (R² = 0.872; adjusted R² = 0.862).

| Explanatory variable | Coefficient | HC3 p-value |
|---|---:|---:|
| Active-address growth | 0.6272 | <0.001 |
| ETH price growth | -0.0215 | 0.687 |
| Gas-price growth | 0.0494 | 0.114 |

Active-address growth was positively and statistically significantly associated with transaction growth. ETH price growth and gas-price growth were not statistically significant at the 5% level.

The coefficient of 0.6272 indicates that a one-percentage-point higher quarterly active-address growth rate is associated with approximately a 0.63-percentage-point higher quarterly transaction growth rate, holding the other predictors constant.

## 4. Parsimonious model and model selection

A second regression included active-address growth as the sole explanatory variable.

| Statistic | Three predictors | Active addresses only |
|---|---:|---:|
| Observations | 43 | 43 |
| R² | 0.872 | 0.864 |
| Adjusted R² | 0.862 | 0.861 |
| AIC | -70.16 | -71.62 |
| BIC | -63.11 | -68.10 |
| Active-address coefficient | 0.6272 | 0.6013 |
| HC3 p-value | <0.001 | <0.001 |

The simpler model retained almost all the explanatory power of the three-predictor specification. Its lower AIC and BIC indicate a more favourable balance between model fit and complexity.

The active-address coefficient remained positive and highly significant (β = 0.6013; HC3 p < 0.001). Therefore, the active-address-only model was selected as the preferred parsimonious specification.

## 5. Diagnostic tests and model stability

The three-predictor growth model was subjected to diagnostic testing:

- **Breusch–Godfrey test:** p = 0.7565. No significant residual autocorrelation was detected up to lag four.
- **Breusch–Pagan test:** p = 0.0465. Evidence of heteroskedasticity was detected, motivating the use of HC3 robust standard errors.
- **Variance Inflation Factors:** active addresses = 3.906; ETH price = 4.091; gas price = 1.243. These values do not indicate severe multicollinearity.
- **CUSUM stability test:** statistic = 0.6488; p = 0.7939. No significant evidence of parameter instability was detected in the preferred active-address-only model.

The HC3 adjustment did not change coefficient estimates, but provided more appropriate standard errors in the presence of heteroskedasticity.

## 6. Main findings

The empirical results identify active-address growth as the strongest explanatory variable among those investigated.

The relationship between active-address growth and transaction growth remained strong and statistically significant in both the three-predictor and parsimonious models. The addition of ETH price and gas price contributed little additional explanatory power.

These findings suggest that changes in Ethereum network participation are closely associated with changes in transaction activity.

## 7. Limitations

Several limitations should be considered:

1. The analysis is based on 43 quarterly growth observations, which limits statistical power.
2. The models identify associations rather than causal relationships.
3. Contemporaneous regressions do not establish whether changes in active addresses precede changes in transaction activity.
4. The CUSUM test does not guarantee that all forms of structural change are absent.
5. Further work is needed to examine lagged relationships, alternative specifications, and out-of-sample performance.
6. A formal cointegration analysis is required before making claims about long-run relationships between the variables.

## 8. Conclusion

Quarterly Ethereum transaction growth is strongly and positively associated with active-address growth. This relationship remains statistically significant after applying HC3 robust standard errors and is supported by the model stability test.

Among the explanatory variables examined, active addresses provide the most useful single-variable explanation of quarterly transaction growth. The active-address-only model is therefore selected as the preferred empirical specification for this stage of the research.

Further analysis should investigate lagged network effects and predictive performance before drawing conclusions about causality or forecasting capability.