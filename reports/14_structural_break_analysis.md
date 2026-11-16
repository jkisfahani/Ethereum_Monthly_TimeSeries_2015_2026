# Structural Break Analysis of Quarterly Ethereum Transaction Activity (2015–2026)

## 1. Objective

A structural break analysis was conducted to investigate whether the underlying trend in Ethereum quarterly transaction activity changed during the observation period. October 2024 (Q4 2024) was selected as a candidate break point, separating the historical period from the more recent phase of transaction growth.

## 2. Methodology

The analysis used 44 quarterly observations covering Q4 2015 to Q3 2026. The natural logarithm of quarterly transaction counts was used to examine changes in the underlying trend.

A Chow structural break test was performed at the candidate break date of Q4 2024. The test compared a single linear time-trend regression fitted to the complete series against separate regressions fitted before and after the proposed break point.

The sample was divided into 36 observations before the candidate break and eight observations from Q4 2024 onward.

The null hypothesis was that the regression coefficients remained unchanged across the two periods.

## 3. Results

The Chow test produced an F-statistic of 3.0116 and a p-value of 0.060486.

At the conventional 5% significance level, the null hypothesis could not be rejected. Therefore, the analysis did not provide sufficient statistical evidence of a structural break at Q4 2024. However, the result was relatively close to the 5% threshold and would be significant at the 10% level.

Separate regression models were also examined to compare the estimated trends across the two periods.

| Regression statistic | Before Q4 2024 | From Q4 2024 |
|---|---:|---:|
| Observations | 36 | 8 |
| Intercept | 15.8366 | 14.7871 |
| Trend coefficient | 0.1012 | 0.1023 |
| R-squared | 0.6212 | 0.9061 |

The estimated trend coefficients were very similar (0.1012 and 0.1023), suggesting comparable average linear trends in log transaction counts across the two periods. The higher post-break R-squared (0.9061) indicates a closer fit of the linear trend to the eight observations in the later period, although this estimate should be interpreted cautiously given the small sample size.

## 4. Discussion

The findings do not establish a statistically significant structural break at Q4 2024 at the 5% level. Furthermore, the similarity between the estimated trend coefficients provides little indication of a substantial change in the average linear trend at this particular break point.

Nevertheless, the test result should not be interpreted as evidence that Ethereum transaction activity remained structurally unchanged throughout the study period. The candidate break date was specified in advance, and the post-break period contained only eight quarterly observations. In addition, the Chow test was based on a linear time-trend specification, which may not adequately capture nonlinear growth, changing volatility, or multiple changes in the underlying data-generating process.

## 5. Conclusion

The Chow structural break test did not identify sufficient evidence of a structural break in the log-transformed quarterly Ethereum transaction series at Q4 2024 (F = 3.0116, p = 0.060486). Although the result approached conventional statistical significance, the estimated linear trend coefficients were nearly identical across the two periods.

Consequently, the evidence for a structural change at the selected date remains inconclusive. Further investigation using alternative break-date methods, longer post-break observations, and models allowing for nonlinear trends may provide additional insight into the recent evolution of Ethereum transaction activity.