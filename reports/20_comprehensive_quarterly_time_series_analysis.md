# Ethereum Quarterly Time-Series Analysis (2015--2026)

## 1. Executive summary

This report consolidates the quarterly time-series analysis of Ethereum
transaction activity from Q4 2015 through Q3 2026. It covers quarterly
aggregation, descriptive and seasonal analysis, stationarity testing,
ARIMA and trend-based forecasting, structural-break testing,
rolling-origin validation, and empirical relationships between
transaction activity and network/market indicators.

The dataset contains **44 complete quarterly observations**. The main
findings are:

1.  Quarterly transaction activity expanded substantially over the
    sample, but growth was uneven and included several contractionary
    periods.
2.  Calendar-quarter averages suggest some seasonal differences, but the
    Kruskal--Wallis test does not find statistically significant
    differences between quarters.
3.  The transaction-count series in levels is non-stationary under the
    ADF test. Its first difference is stationary. The log-transformed
    level series, however, is stationary under the reported ADF
    specification.
4.  Among the forecasting models tested, a simple **naïve forecast**
    performed best in one-quarter-ahead rolling-origin validation. The
    piecewise linear trend model was the strongest of the more
    structured alternatives, but it did not beat the naïve benchmark.
5.  In the empirical analysis, quarterly changes in active addresses
    have the strongest association with quarterly changes in transaction
    activity. This relationship remains statistically significant with
    HC3 robust standard errors.
6.  The level regression has a very high (R\^2), but its residual
    autocorrelation and non-stationarity make a long-run interpretation
    unsafe. The growth-rate model is more defensible, although it
    remains an observational association rather than evidence of
    causation.

The analysis is based on a relatively short quarterly sample. Forecasts
and model comparisons should therefore be interpreted as evidence about
this dataset and evaluation period, not as guaranteed predictions of
future Ethereum activity.

## 2. Data and construction

### 2.1 Transaction data

The starting monthly dataset is:

`data/processed/ethereum_monthly_txcount_2015_2026.csv`

It contains 134 monthly observations from September 2015 through October
2026, with the fields `month` and `tx_count`.

Quarterly transaction totals were constructed by resampling the monthly
series at quarter starts (`QS`) and summing the monthly transaction
counts. The analysis retained only quarters containing all three months.
The resulting quarterly dataset is:

`data/processed/ethereum_quarterly_txcount_2015_2026.csv`

The quarterly dataset contains **44 observations**, from **Q4 2015 to Q3
2026**. The continuity check found:

-   Expected quarters: 44
-   Observed quarters: 44
-   Missing quarters: 0
-   Duplicate quarters: 0
-   Months represented in each retained quarter: 3

The main quarterly variables are:

-   `quarter_tx_count`: total transactions in the quarter
-   `monthly_average`: average monthly transaction count within the
    quarter
-   `months_observed`: number of monthly observations contributing to
    the quarter
-   `qoq_growth_pct`: quarter-over-quarter percentage change
-   `yoy_growth_pct`: year-over-year percentage change

The first and last quarterly transaction totals are:

  Quarter     Transactions
  --------- --------------
  Q4 2015          786,870
  Q3 2026      199,319,870

### 2.2 Additional explanatory variables

The empirical analysis combined the quarterly transaction dataset with
three additional variables:

-   `active_addresses`: quarterly active-address count
-   `avg_gas_price_gwei`: quarterly average gas price in Gwei
-   `median_gas_price_gwei`: quarterly median gas price in Gwei
-   `avg_eth_usd`: quarterly average ETH/USD price

Daily ETH price observations were retrieved from the DefiLlama price API
and aggregated to quarterly averages. The daily price series contained
4,015 observations from 1 October 2015 through 30 September 2026, with
no missing prices. The resulting quarterly price series contained 44
observations, aligned to Q4 2015--Q3 2026.

The combined dataset initially contained 44 rows and 10 columns. The
first lagged growth values were missing by construction: one missing QoQ
growth value and four missing YoY growth values. These are expected
because the first observation has no preceding quarter and the first
four observations cannot have a four-quarter comparison.

For the empirical regression, four variables were selected:

1.  Quarterly transaction count
2.  Active addresses
3.  Average gas price
4.  Average ETH/USD price

All four are strictly positive in the supplied dataset, allowing
natural-log transformations.

## 3. Descriptive statistics and growth

### 3.1 Quarterly transaction counts

  Statistic              Quarterly transactions
  -------------------- ------------------------
  Observations                               44
  Mean                               85,611,480
  Standard deviation                 51,270,690
  Minimum                               786,870
  25th percentile                    55,533,480
  Median                             96,410,910
  75th percentile                   110,338,800
  Maximum                           203,852,887

The distribution spans a wide range, reflecting the expansion of
Ethereum activity over the sample. The mean is below the median,
indicating that the relatively low early observations pull the overall
mean downward.

### 3.2 Quarter-over-quarter growth

QoQ growth is calculated as the percentage change from the preceding
quarter.

  Statistic                           QoQ growth
  -------------------- -------------------------
  Observations                                43
  Mean                                    18.74%
  Standard deviation     41.06 percentage points
  Minimum                                -18.43%
  Median                                   1.70%
  75th percentile                         26.58%
  Maximum                                167.44%

The mean is substantially higher than the median because of very large
increases during the early expansion period. The largest QoQ increases
were:

  Quarter     QoQ growth
  --------- ------------
  Q2 2017        167.44%
  Q1 2016        134.10%
  Q3 2017        106.97%
  Q2 2016        102.03%
  Q4 2017         88.12%

The largest QoQ decreases were:

  Quarter     QoQ growth
  --------- ------------
  Q3 2018        -18.43%
  Q3 2021        -10.31%
  Q2 2018         -9.93%
  Q1 2022         -9.11%
  Q4 2018         -8.88%

### 3.3 Year-over-year growth

YoY growth compares each quarter with the corresponding quarter one year
earlier.

  Statistic                            YoY growth
  -------------------- --------------------------
  Observations                                 40
  Mean                                    128.83%
  Standard deviation     305.23 percentage points
  Minimum                                 -36.82%
  Median                                   15.23%
  75th percentile                          75.00%
  Maximum                               1,348.64%

There were 31 positive-growth quarters and 9 negative-growth quarters,
with no zero-growth quarters. The negative YoY observations occurred in:

-   Q4 2018: -6.86%
-   Q1 2019: -36.82%
-   Q1 2022: -5.99%
-   Q2 2022: -19.00%
-   Q3 2022: -4.09%
-   Q4 2022: -16.06%
-   Q1 2023: -10.25%
-   Q2 2023: -4.34%
-   Q3 2023: -11.24%

The largest YoY increases occurred during the early growth phase,
particularly Q1 2018 (1,348.64%) and Q4 2017 (1,283.80%). These
unusually large rates partly reflect the low base at the beginning of
the series.

### 3.4 Recent transaction activity

The final 16 quarters show a period of comparatively moderate activity
followed by a substantial increase in 2026:

  Quarter     Transactions   QoQ growth
  --------- -------------- ------------
  Q4 2022       97,508,049       -7.85%
  Q1 2023       94,763,301       -2.81%
  Q2 2023       95,313,768        0.58%
  Q3 2023       93,919,245       -1.46%
  Q4 2023       99,104,337        5.52%
  Q1 2024      107,452,234        8.42%
  Q2 2024      106,883,044       -0.53%
  Q3 2024      103,315,009       -3.34%
  Q4 2024      112,095,697        8.50%
  Q1 2025      110,364,282       -1.54%
  Q2 2025      121,082,115        9.71%
  Q3 2025      147,337,159       21.68%
  Q4 2025      145,154,396       -1.48%
  Q1 2026      200,442,726       38.09%
  Q2 2026      203,852,887        1.70%
  Q3 2026      199,319,870       -2.22%

The series reached its sample maximum in Q2 2026, followed by a modest
decline in Q3 2026.

### 3.5 Descriptive visualizations

The notebook saved:

-   `visualizations/01_quarterly_transaction_count.png`
-   `visualizations/02_quarterly_qoq_growth.png`
-   `visualizations/03_quarterly_yoy_growth.png`

## 4. Seasonal analysis and decomposition

### 4.1 Calendar-quarter averages

The mean transaction count was grouped by calendar quarter:

  -----------------------------------------------------------------------
  Calendar      Observations           Mean         Median       Standard
  quarter                      transactions   transactions      deviation
  ----------- -------------- -------------- -------------- --------------
  Q1                      11     83,788,512     94,763,301     55,758,116

  Q2                      11     89,499,189     95,313,768     54,439,074

  Q3                      11     92,531,674    103,315,009     53,911,963

  Q4                      11     76,626,562     97,508,049     46,502,672
  -----------------------------------------------------------------------

The preliminary seasonal indices, calculated relative to the overall
mean, were:

  Calendar quarter     Seasonal index
  ------------------ ----------------
  Q1                            97.87
  Q2                           104.54
  Q3                           108.08
  Q4                            89.51

These preliminary indices suggest relatively higher average activity in
Q2 and Q3 and lower average activity in Q4. However, the indices are
calculated from a series with a strong long-term trend. They should not
be interpreted as definitive seasonal effects without accounting for
that trend.

### 4.2 Detrended seasonal comparison

A centred four-quarter moving average was used to estimate the trend:

`trend_4q = rolling mean of quarterly transaction counts over four quarters`

The detrended index was calculated as the observed quarterly count
divided by the estimated trend and multiplied by 100.

  -----------------------------------------------------------------------
  Calendar      Observations Mean detrended         Median       Standard
  quarter                             index                     deviation
  ----------- -------------- -------------- -------------- --------------
  Q1                      10         100.13          98.31          15.73

  Q2                      11         108.49         107.38          12.96

  Q3                      10         105.68         105.63          11.02

  Q4                      10         100.54          99.11          10.00
  -----------------------------------------------------------------------

The Kruskal--Wallis test gave:

-   H statistic: 4.4049
-   p-value: 0.2209

At the 5% significance level, the null hypothesis of equal distributions
across calendar quarters is not rejected. Thus, the descriptive
differences in seasonal indices are not supported by statistically
significant evidence in this sample.

The analysis saved:

-   `visualizations/04_quarterly_seasonal_indices.png`
-   `visualizations/05_detrended_quarterly_activity.png`
-   `data/processed/ethereum_detrended_quarterly_summary.csv`

### 4.3 Classical decomposition

The quarterly series was decomposed using classical additive
decomposition with a seasonal period of four quarters. The decomposition
visualization was saved as:

-   `visualizations/06_quarterly_time_series_decomposition.png`

The decomposition residuals had 40 usable observations, a mean of
approximately -173,108, and a standard deviation of approximately
7,003,547. There were 19 positive and 21 negative residuals.

The Ljung--Box results for the decomposition residuals were:

  Lag     Q statistic   p-value
  ----- ------------- ---------
  4            5.6190    0.2295
  8            9.5938    0.2947

These tests do not reject the null of no residual autocorrelation at the
tested lags. This result applies to the decomposition residuals and
should not be confused with the separate autocorrelation diagnostics for
the empirical regression.

The residual visualization was saved as
`visualizations/07_quarterly_decomposition_residuals.png`.

## 5. Stationarity and transformations

### 5.1 ADF test on the original series

The Augmented Dickey--Fuller (ADF) test on the quarterly
transaction-count level series returned:

-   ADF statistic: -0.1376
-   p-value: 0.9456
-   1% critical value: -3.5925
-   5% critical value: -2.9315
-   10% critical value: -2.6041

The unit-root null hypothesis is not rejected. The original quarterly
transaction-count series is therefore treated as non-stationary under
this test specification.

### 5.2 First-differenced series

The first difference contains 43 observations. The ADF test returned:

-   ADF statistic: -5.8323
-   p-value: less than 0.001
-   1% critical value: -3.5966
-   5% critical value: -2.9333
-   10% critical value: -2.6050

The unit-root null is rejected. The first-differenced series is
stationary under the reported test specification.

The first-difference and ACF/PACF visualizations were saved as:

-   `visualizations/08_quarterly_first_difference.png`
-   `visualizations/09_quarterly_acf_pacf.png`

### 5.3 Log-transformed series

The natural logarithm of quarterly transaction counts was also examined.
The ADF test on the log-level series returned:

-   ADF statistic: -5.5844
-   p-value: less than 0.001
-   1% critical value: -3.5925
-   5% critical value: -2.9315
-   10% critical value: -2.6041

Under the reported ADF specification, the unit-root null is rejected for
the log-transformed series. This differs from the result for the
untransformed level series. The log transformation changes the scale and
behaviour of the series; it does not by itself establish that a
particular regression or forecasting model is correctly specified.

The log-series and its ACF/PACF visualization were saved as:

-   `visualizations/11_quarterly_log_transformation.png`
-   `visualizations/12_quarterly_log_acf_pacf.png`

## 6. ARIMA and ARMA modelling

### 6.1 ARIMA candidate models

Three ARIMA models were fitted to the quarterly transaction-count level
series:

  Model                 AIC        BIC
  -------------- ---------- ----------
  ARIMA(0,1,0)     1,536.46   1,538.23
  ARIMA(1,1,0)     1,536.94   1,540.46
  ARIMA(0,1,1)     1,537.45   1,540.97

ARIMA(0,1,0) had the lowest AIC and BIC among these candidates, although
the differences were small.

For ARIMA(0,1,0), the reported model summary included:

-   44 observations
-   Log likelihood: -767.232
-   AIC: 1,536.464
-   BIC: 1,538.226
-   Residual Ljung--Box p-value at lag 1: 0.61
-   Jarque--Bera p-value: approximately 0.00
-   Heteroskedasticity test p-value: 0.24

The residual normality test indicates non-normal residuals. The reported
lag-1 Ljung--Box test does not indicate significant autocorrelation at
that lag.

### 6.2 Fixed-origin holdout evaluation

The first 36 quarters (Q4 2015--Q3 2024) were used for training. The
final eight quarters (Q4 2024--Q3 2026) were held out for evaluation.

  Model                     MAE         RMSE     MAPE
  ---------------- ------------ ------------ --------
  Seasonal Naïve     50,767,486   62,953,100   28.71%
  ARIMA(0,1,0)       51,641,133   64,118,580   29.19%
  ARIMA(0,1,1)       52,587,259   64,883,014   29.84%
  ARIMA(1,1,0)       53,238,676   65,476,061   30.27%

The seasonal naïve model performed best among these initial candidates,
although its errors were close to those of ARIMA(0,1,0).

The comparison was saved as
`data/processed/ethereum_quarterly_forecast_comparison.csv`, and the
chart as `visualizations/10_quarterly_forecast_comparison.png`.

### 6.3 ARIMA(0,1,0) with drift

An ARIMA(0,1,0) model with a linear drift term was then estimated on the
training sample. The estimated drift was approximately **2,929,375
transactions per quarter**.

Its fixed-origin forecasts were:

  Quarter     Forecast transactions
  --------- -----------------------
  Q4 2024               106,244,384
  Q1 2025               109,173,760
  Q2 2025               112,103,135
  Q3 2025               115,032,511
  Q4 2025               117,961,886
  Q1 2026               120,891,261
  Q2 2026               123,820,637
  Q3 2026               126,750,012

Its holdout accuracy was:

-   MAE: 38,458,943
-   RMSE: 49,873,942
-   MAPE: 21.22%

This improved substantially on the no-drift ARIMA candidates and the
seasonal naïve model in this fixed-origin comparison. However, later
rolling-origin validation showed that a simple naïve forecast was
stronger when the model was repeatedly updated.

## 7. Trend-model comparisons

Several deterministic trend specifications were estimated. The main
fixed-origin evaluation used the same 36-quarter training sample and
eight-quarter test sample.

### 7.1 Linear and quadratic trends

Full-sample log-trend model comparison:

  Model                   R²   Adjusted R²        AIC        BIC
  ----------------- -------- ------------- ---------- ----------
  Linear trend        0.6236        0.6146   108.9064   112.4748
  Quadratic trend     0.8353        0.8273    74.5255    79.8781

The nested F-test comparing the quadratic model with the linear model
returned:

-   F statistic: 52.7293
-   p-value: 7.0614 × 10⁻⁹

The quadratic term materially improves in-sample fit. This does not
guarantee better out-of-sample forecasts.

Fixed-origin holdout performance:

  Model                          MAE             RMSE      MAPE
  ----------------- ---------------- ---------------- ---------
  Linear trend        266.71 million   273.98 million   173.86%
  Quadratic trend     126.31 million   136.13 million    78.24%

Both models extrapolated poorly over the final eight quarters. The
linear trend substantially overpredicted, while the quadratic
specification produced declining forecasts that were far below actual
observations.

The quadratic comparison chart was saved as
`visualizations/15_quadratic_trend_forecast_comparison.png`.

### 7.2 Piecewise linear trend

A piecewise linear trend was estimated using a candidate breakpoint at
Q4 2022. The training sample contained 36 quarters, with 28 observations
before the breakpoint and 8 observations from the breakpoint onward.

The training-sample comparison was:

  Model                          R²   Adjusted R²       AIC       BIC
  ------------------------ -------- ------------- --------- ---------
  Linear trend               0.6212        0.6101   91.9395   95.1066
  Piecewise linear trend     0.7555        0.7326   80.1756   86.5097

The piecewise model improved in-sample fit and information criteria. In
the fitted model, the post-break indicator was statistically significant
(p = 0.029), while the change in slope after the breakpoint was not
statistically significant (p = 0.229).

The Durbin--Watson statistic was 0.245, indicating substantial positive
residual autocorrelation in the training regression. This weakens the
reliability of conventional OLS inference for that specification.

Fixed-origin forecasts were:

  Quarter     Forecast transactions
  --------- -----------------------
  Q4 2024               107,531,337
  Q1 2025               109,364,649
  Q2 2025               111,229,218
  Q3 2025               113,125,577
  Q4 2025               115,054,266
  Q1 2026               117,015,838
  Q2 2026               119,010,853
  Q3 2026               121,039,881

Fixed-origin accuracy:

-   MAE: 40.78 million
-   RMSE: 53.01 million
-   MAPE: 22.45%

This was considerably better than the linear and quadratic trend models
in the fixed-origin comparison.

The piecewise model visualization was saved as
`visualizations/16_piecewise_linear_trend.png`.

### 7.3 Logarithmic time trend

The logarithmic time model regressed log transaction counts on log time.
Its training-sample results included:

-   R²: 0.891
-   Adjusted R²: 0.888
-   Log-time slope: 1.4754 (p \< 0.001)
-   Intercept: 13.6843
-   Durbin--Watson: 0.280

The low Durbin--Watson statistic indicates residual serial correlation.

Its fixed-origin forecasts ranged from approximately 180.60 million in
Q4 2024 to 233.21 million in Q3 2026. Holdout accuracy was:

-   MAE: 51,637,948
-   RMSE: 56,323,164
-   MAPE: 38.88%

### 7.4 Exponential trend interpretation

The model fitted by regressing log transaction counts on a linear time
index is an exponential-growth specification when expressed on the
original transaction-count scale. It is not a distinct independent model
from the linear trend in log space. The report therefore treats it as an
alternative interpretation of that log-linear time trend rather than
counting it as a separate model family.

### 7.5 Local linear trend model

A local linear trend model was fitted to the log-transformed training
series using an unobserved-components specification.

Reported information criteria:

-   Log likelihood: -0.683
-   AIC: 7.366
-   BIC: 11.945
-   HQIC: 8.928

The estimated trend variance had a p-value of 0.056. The forecast
accuracy was:

-   MAE: 66,010,944
-   RMSE: 79,649,662
-   MAPE: 37.95%

The model did not outperform the piecewise linear model or the naïve
benchmark in the reported evaluations.

### 7.6 Fixed-origin trend-model comparison

  Model                                 MAE             RMSE      MAPE
  ------------------------ ---------------- ---------------- ---------
  Piecewise linear trend      40.78 million    53.01 million    22.45%
  Logarithmic time trend      51.64 million    56.32 million    38.88%
  Local linear trend          66.01 million    79.65 million    37.95%
  Quadratic trend            126.31 million   136.13 million    78.24%
  Linear trend               266.71 million   273.98 million   173.86%

The piecewise linear trend was the strongest of these trend
specifications in the fixed-origin holdout evaluation.

## 8. Rolling-origin forecast validation

A one-quarter-ahead rolling-origin evaluation was conducted over the
final eight quarters. At each forecast origin, the training sample was
expanded and the model was re-estimated. This differs from the
fixed-origin exercise, where all eight test forecasts are generated from
the same initial training sample.

The rolling-origin results were:

  Model                       MAE         RMSE     MAPE
  ------------------ ------------ ------------ --------
  Naïve                14,112,406   22,299,212    8.64%
  Piecewise linear     23,113,140   30,373,075   13.10%
  Seasonal naïve       41,501,907   51,328,629   23.78%
  Logarithmic time     38,233,798   46,944,075   30.66%

The naïve model, which uses the most recently observed quarter as the
next-quarter forecast, performed best. It had the lowest MAE, RMSE, and
MAPE.

The piecewise linear model was the best of the two structured models in
this rolling-origin comparison, but it did not outperform the naïve
forecast. Its rolling-origin MAPE of 13.10% is also different from its
fixed-origin MAPE of 22.45%, because the forecast design and information
available at each origin differ.

Selected quarter-by-quarter absolute percentage errors:

  Quarter     Naïve APE   Piecewise linear APE
  --------- ----------- ----------------------
  Q4 2024         7.83%                  4.07%
  Q1 2025         1.57%                  0.94%
  Q2 2025         8.85%                  6.52%
  Q3 2025        17.82%                 19.75%
  Q4 2025         1.50%                 10.37%
  Q1 2026        27.58%                 30.78%
  Q2 2026         1.67%                 21.75%
  Q3 2026         2.27%                 10.65%

The naïve forecast was not best in every individual quarter, but its
aggregate rolling-origin accuracy was strongest.

The rolling-origin visualization was saved as
`visualizations/17_rolling_origin_forecast_comparison.png`.

**Forecasting conclusion:** The preferred benchmark for this sample and
one-quarter-ahead task is the naïve model. The piecewise linear trend is
a useful structured alternative, but the additional complexity did not
produce better rolling-origin accuracy. Future forecasting work should
continue to compare candidate models against simple benchmarks using
rolling-origin evaluation.

## 9. Structural-break analysis

### 9.1 Chow test at Q4 2024

A Chow test was conducted on the log-transformed quarterly series with a
linear time trend, using Q4 2024 as the candidate break date. This date
also marks the start of the eight-quarter holdout period.

The sample contained 44 observations: 36 before the candidate break and
8 from the candidate break onward.

Results:

-   F statistic: 3.0116
-   p-value: 0.060486

At the conventional 5% significance level, the null hypothesis of
parameter stability is not rejected. The result is borderline at the 10%
level, but it is not sufficient to establish a structural break. The
post-break segment contains only eight observations, so the test has
limited information about the later regime.

Separate log-linear trend fits gave:

  Segment            Intercept   Trend coefficient       R²
  ---------------- ----------- ------------------- --------
  Before Q4 2024       15.8366              0.1012   0.6212
  Q4 2024 onward       14.7871              0.1023   0.9061

The post-break R² is based on only eight observations and should not be
compared as if the two segments had equal sample sizes.

### 9.2 CUSUM stability test for the growth model

A CUSUM parameter-stability test was also applied to the parsimonious
quarterly growth regression (transaction growth explained by
active-address growth).

Results:

-   CUSUM statistic: 0.6488
-   p-value: 0.793903
-   10% critical value: 1.2200
-   5% critical value: 1.3600
-   1% critical value: 1.6300

The null hypothesis of parameter stability is not rejected. There is no
statistically significant evidence of parameter instability in this
growth model according to this test. This result does not prove that the
relationship is invariant across all periods; it indicates that this
test does not detect instability in the available sample.

## 10. Empirical analysis: transaction activity and network indicators

### 10.1 Variables and transformations

The empirical model used four logged variables:

-   Quarterly transaction count
-   Active addresses
-   Average gas price in Gwei
-   Average ETH/USD price

The analysis examined both correlations between log levels and
correlations between quarterly log changes. Quarterly log changes are
first differences of the logged series and can be interpreted
approximately as proportional growth rates.

### 10.2 Correlation between log levels

The Pearson correlation matrix for the log-transformed variables was:

  --------------------------------------------------------------------------
  Variable                  Log     Log active    Log average  Log ETH price
                   transactions      addresses            gas
  -------------- -------------- -------------- -------------- --------------
  Log                     1.000          0.994         -0.277          0.938
  transactions

  Log active              0.994          1.000         -0.258          0.942
  addresses

  Log average            -0.277         -0.258          1.000         -0.226
  gas

  Log ETH price           0.938          0.942         -0.226          1.000
  --------------------------------------------------------------------------

The level correlations are very high for transactions and active
addresses, and for transactions and ETH price. However, these variables
may share strong time trends. High correlations between non-stationary
or trending levels can be misleading and should not be interpreted as
evidence of a stable long-run relationship without appropriate
time-series tests.

### 10.3 Correlation between quarterly log changes

The correlation matrix for quarterly log changes was:

  ------------------------------------------------------------------------------
  Variable            Transaction   Active-address      Gas-price      ETH-price
                           change           change         change         change
  ---------------- -------------- ---------------- -------------- --------------
  Transaction               1.000            0.930          0.055          0.786
  change

  Active-address            0.930            1.000         -0.033          0.835
  change

  Gas-price change          0.055           -0.033          1.000          0.215

  ETH-price change          0.786            0.835          0.215          1.000
  ------------------------------------------------------------------------------

Active-address changes have the strongest correlation with transaction
changes (r = 0.930). ETH-price changes also have a strong positive
correlation (r = 0.786), while gas-price changes have almost no linear
correlation with transaction changes (r = 0.055).

This supports examining active-address growth as the primary explanatory
variable in the growth regression.

### 10.4 Level regression: active addresses alone

The first OLS model regressed log transaction count on log active
addresses:

\[ `\log`{=tex}(`\text{transactions}`{=tex}\_t) = 5.4396 +
0.7828`\log`{=tex}(`\text{active addresses}`{=tex}\_t) +
`\varepsilon`{=tex}\_t \]

Key results:

-   Observations: 44
-   R²: 0.987
-   Adjusted R²: 0.987
-   Active-address coefficient: 0.7828
-   Standard error: 0.014
-   t statistic: 56.908
-   p-value: less than 0.001

The coefficient is an elasticity in the log-log specification: a 1%
higher active-address count is associated with approximately 0.783%
higher transaction count in this fitted sample.

However, the model has a Durbin--Watson statistic of 0.722, suggesting
positive residual autocorrelation. The Breusch--Godfrey test with four
lags confirmed residual autocorrelation:

-   LM statistic: 13.2852
-   LM p-value: 0.009963
-   F statistic: 4.1091
-   F p-value: 0.007285

An ADF test on the OLS residuals returned:

-   ADF statistic: -1.5354
-   p-value: 0.515949

The residual unit-root null is not rejected under the reported ordinary
ADF test. This is a warning against interpreting the level regression as
a valid long-run relationship. A formal cointegration analysis would
require an appropriate residual-based cointegration test and its
corresponding critical values; the ordinary residual ADF output alone is
not a complete cointegration test.

### 10.5 Newey--West standard errors for the level regression

The active-address-only level regression was recalculated with
HAC/Newey--West standard errors using four lags.

  Term                     Coefficient   HAC standard error   t statistic   p-value
  ---------------------- ------------- -------------------- ------------- ---------
  Constant                      5.4396                0.393        13.840   \<0.001
  Log active addresses          0.7828                0.024        32.580   \<0.001

The active-address coefficient remains statistically significant with
HAC standard errors. However, robust standard errors do not correct a
potentially spurious levels relationship, nor do they remove residual
autocorrelation from the model. The level regression should therefore be
treated as descriptive rather than as a confirmed long-run model.

### 10.6 Adding ETH price and gas price to the level model

Three nested specifications were estimated:

  ------------------------------------------------------------------------
  Model            Explanatory                         R² Key coefficient
                   variables                              results
  ---------------- ---------------- --------------------- ----------------
  Model 1          Active addresses                 0.987 Active
                                                          addresses:
                                                          0.7828, p \<
                                                          0.001

  Model 2          Active                           0.987 Active
                   addresses, ETH                         addresses:
                   price                                  0.7681, p \<
                                                          0.001; ETH
                                                          price: 0.0126, p
                                                          = 0.707

  Model 3          Active                           0.988 Active
                   addresses, ETH                         addresses:
                   price, gas price                       0.7611, p \<
                                                          0.001; ETH
                                                          price: 0.0148, p
                                                          = 0.659; gas
                                                          price: -0.0227,
                                                          p = 0.227
  ------------------------------------------------------------------------

Adding ETH price and gas price barely changes the R², and neither
additional variable is statistically significant in these level
specifications. Active addresses remain statistically significant in all
three.

The high fit in levels should still be interpreted in light of the
autocorrelation and residual-stationarity concerns described above.

### 10.7 Quarterly log-change regression

The more defensible empirical specification models quarterly log
changes:

\[ `\Delta`{=tex}`\log`{=tex}(`\text{transactions}`{=tex}\_t) =
`\beta`{=tex}\_0 +
`\beta`{=tex}\_1`\Delta`{=tex}`\log`{=tex}(`\text{active addresses}`{=tex}\_t) +
`\beta`{=tex}\_2`\Delta`{=tex}`\log`{=tex}(`\text{ETH price}`{=tex}\_t) +
`\beta`{=tex}\_3`\Delta`{=tex}`\log`{=tex}(`\text{average gas price}`{=tex}\_t) +
`\varepsilon`{=tex}\_t \]

The model used 43 quarterly changes.

OLS results:

  Variable                     Coefficient   Standard error   t statistic   p-value
  -------------------------- ------------- ---------------- ------------- ---------
  Constant                          0.0315            0.017         1.846     0.072
  Active-address change             0.6272            0.073         8.572   \<0.001
  ETH-price change                 -0.0215            0.056        -0.382     0.705
  Average gas-price change          0.0494            0.033         1.507     0.140

Overall model statistics:

-   Observations: 43
-   R²: 0.872
-   Adjusted R²: 0.862
-   F statistic: 88.70
-   Model p-value: 1.81 × 10⁻¹⁷
-   Durbin--Watson: 1.929

Active-address growth is the only explanatory variable with a
statistically significant coefficient in this multiple regression. Its
coefficient of 0.6272 indicates that a 1% higher quarterly growth rate
in active addresses is associated with approximately 0.627% higher
quarterly transaction growth, conditional on the other included
variables, under this log-difference specification.

The ETH-price and gas-price growth coefficients are not statistically
significant at the 5% level.

### 10.8 Diagnostics for the growth regression

The Breusch--Godfrey test for four lags returned:

-   LM statistic: 1.8874
-   p-value: 0.756453

There is no evidence of residual autocorrelation at the tested lags.

The Breusch--Pagan test returned:

-   LM statistic: 7.9769
-   p-value: 0.046491

This indicates evidence of heteroskedasticity at the 5% level.
Accordingly, HC3 heteroskedasticity-robust standard errors were
calculated.

### 10.9 HC3 robust inference

  Variable                     Coefficient   HC3 standard error   t statistic   p-value
  -------------------------- ------------- -------------------- ------------- ---------
  Constant                          0.0315                0.016         1.982     0.055
  Active-address change             0.6272                0.103         6.074   \<0.001
  ETH-price change                 -0.0215                0.053        -0.406     0.687
  Average gas-price change          0.0494                0.031         1.618     0.114

The active-address coefficient remains strongly statistically
significant after applying HC3 robust standard errors. The ETH-price and
gas-price coefficients remain statistically insignificant.

### 10.10 Multicollinearity

Variance inflation factors for the explanatory variables in the growth
model were:

  Variable                       VIF
  -------------------------- -------
  Active-address change        3.906
  ETH-price change             4.091
  Average gas-price change     1.243

These values do not indicate extreme multicollinearity under common
rules of thumb, although the active-address and ETH-price changes share
some explanatory information. This is consistent with their correlation
of 0.835.

### 10.11 Parsimonious growth model

A simpler model containing only active-address growth was estimated:

\[ `\Delta`{=tex}`\log`{=tex}(`\text{transactions}`{=tex}\_t) = 0.0271 +
0.6013`\Delta`{=tex}`\log`{=tex}(`\text{active addresses}`{=tex}\_t) +
`\varepsilon`{=tex}\_t \]

With HC3 robust standard errors:

-   Observations: 43
-   R²: 0.864
-   Adjusted R²: 0.861
-   Active-address coefficient: 0.6013
-   HC3 standard error: 0.075
-   t statistic: 8.047
-   p-value: less than 0.001

The active-address coefficient remains highly significant. The model
explains 86.4% of the in-sample variation in quarterly transaction
growth, despite having only one explanatory variable.

The three-variable growth model has a slightly higher R² (0.872), but
the added ETH-price and gas-price terms are not individually
significant. The parsimonious model is therefore attractive for
interpretation and simplicity.

### 10.12 CUSUM stability test

The CUSUM test on the parsimonious growth model returned:

-   CUSUM statistic: 0.6488
-   p-value: 0.793903

The test does not reject parameter stability. There is no detected
evidence of parameter instability in this model over the available
sample, although the result does not prove that the coefficient will
remain unchanged in future periods.

The relationship visualization was saved as:

`visualizations/18_transaction_vs_active_address_growth.png`

It plots quarterly active-address log changes against quarterly
transaction-count log changes, with an OLS fitted line.

## 11. Overall interpretation

### 11.1 Transaction activity and network participation

The strongest empirical result is the close relationship between changes
in active addresses and changes in transaction activity. This
relationship appears in the correlation matrix and remains statistically
significant in both the multiple and parsimonious quarterly growth
regressions.

The result is consistent with the interpretation that changes in network
participation are closely associated with changes in transaction
activity. It does not establish that growth in active addresses causes
transaction growth. Both variables may respond to other factors not
included in the model.

### 11.2 ETH price and gas price

ETH price has a strong contemporaneous correlation with transaction
growth in the bivariate quarterly-change matrix. However, its
coefficient is not statistically significant once active-address growth
and gas-price growth are included in the multiple regression.

Gas-price growth has almost no simple correlation with transaction
growth and is also statistically insignificant in the multiple growth
model. These results do not demonstrate that gas prices or ETH prices
are irrelevant to network activity; they indicate that their independent
contributions are not established by these particular models and this
sample.

### 11.3 Forecasting

Forecast rankings depend on the evaluation design:

-   In the fixed-origin eight-quarter comparison, ARIMA(0,1,0) with
    drift and the piecewise linear trend were among the stronger models.
-   In the rolling-origin one-quarter-ahead evaluation, the naïve model
    performed best overall.
-   The piecewise linear model was the strongest structured model in the
    rolling-origin comparison, but its MAPE (13.10%) was higher than the
    naïve model's MAPE (8.64%).

For a practical one-quarter-ahead benchmark based on the available
evaluation, the naïve model should be retained. More complex models
should be adopted only if they demonstrate consistent improvement over
this benchmark in additional out-of-sample periods.

### 11.4 Seasonality and structural stability

The seasonal indices show descriptive differences, but the
Kruskal--Wallis test does not find statistically significant differences
between calendar quarters. The Chow test at Q4 2024 also does not reject
parameter stability at the 5% level, although its p-value is close to
0.05. The CUSUM test for the parsimonious growth regression likewise
does not detect instability.

These tests address different questions and different specifications.
Their results should not be combined into a claim that the entire
Ethereum transaction process is structurally stable.

## 12. Limitations and recommendations

1.  **Short quarterly sample:** The analysis has 44 quarterly levels and
    43 quarterly changes. This restricts the number of parameters that
    can be estimated reliably and limits the power of structural-break
    tests.
2.  **Incomplete final calendar year:** The sample ends in Q3 2026. No
    Q4 2026 observation is included.
3.  **Aggregation:** Quarterly aggregation smooths monthly fluctuations
    and can conceal short-lived changes.
4.  **Seasonality:** The seasonal comparison has only 11 observations
    for each calendar quarter, and the trend can affect unadjusted
    seasonal averages.
5.  **Forecast uncertainty:** Forecast rankings can change with the
    evaluation window and forecast design. The rolling-origin test
    covers only eight quarters.
6.  **Level-regression risk:** The level regression has high explanatory
    power but also residual autocorrelation and a non-stationary
    residual series under the reported ADF test. Robust standard errors
    do not resolve a potentially spurious levels relationship.
7.  **Cointegration:** A formal cointegration analysis would be needed
    before making a long-run equilibrium claim about transactions and
    active addresses.
8.  **Heteroskedasticity:** The growth regression shows evidence of
    heteroskedasticity. HC3 robust standard errors were used for
    inference, but alternative specifications and additional validation
    would be useful.
9.  **Omitted variables:** Network activity may also be associated with
    factors not included here, such as application activity, stablecoin
    transfers, Layer 2 migration, protocol changes, market conditions,
    and changes in transaction composition.
10. **Association is not causation:** The regressions are observational
    and contemporaneous. They do not establish causal direction or
    identify the effect of an intervention.

Recommended next steps:

-   Extend the dataset as new quarterly observations become available.
-   Continue rolling-origin evaluation and compare all candidate models
    with naïve benchmarks.
-   Examine monthly data to test whether the quarterly results conceal
    shorter-term dynamics.
-   Conduct a formal cointegration analysis before interpreting any
    level relationship as long-run.
-   Consider dynamic growth models with carefully limited lag
    structures.
-   Investigate whether active-address definitions and data sources are
    consistent over the full sample.
-   Add relevant network and ecosystem indicators only when they are
    consistently available and theoretically justified.

## 13. Visualization and output inventory

The following visualizations were produced by the notebook:

  -----------------------------------------------------------------------------------
  File                                            Description
  ----------------------------------------------- -----------------------------------
  `01_quarterly_transaction_count.png`            Quarterly transaction-count series

  `02_quarterly_qoq_growth.png`                   Quarter-over-quarter growth

  `03_quarterly_yoy_growth.png`                   Year-over-year growth

  `04_quarterly_seasonal_indices.png`             Preliminary seasonal indices

  `05_detrended_quarterly_activity.png`           Detrended quarterly activity

  `06_quarterly_time_series_decomposition.png`    Classical quarterly decomposition

  `07_quarterly_decomposition_residuals.png`      Decomposition residuals

  `08_quarterly_first_difference.png`             First-differenced transaction
                                                  series

  `09_quarterly_acf_pacf.png`                     ACF/PACF of first differences

  `10_quarterly_forecast_comparison.png`          ARIMA forecast comparison; updated
                                                  with the drift model

  `11_quarterly_log_transformation.png`           Log-transformed quarterly series

  `12_quarterly_log_acf_pacf.png`                 ACF/PACF of log series

  `13_quarterly_growth_rate.png`                  Quarterly growth rate with
                                                  holdout-period marker

  `15_quadratic_trend_forecast_comparison.png`    Quadratic trend forecast comparison

  `16_piecewise_linear_trend.png`                 Piecewise linear trend

  `17_rolling_origin_forecast_comparison.png`     Rolling-origin forecast comparison

  `18_transaction_vs_active_address_growth.png`   Transaction growth versus
                                                  active-address growth
  -----------------------------------------------------------------------------------

Related processed outputs include:

-   `data/processed/ethereum_monthly_txcount_2015_2026.csv`
-   `data/processed/ethereum_quarterly_txcount_2015_2026.csv`
-   `data/processed/ethereum_detrended_quarterly_summary.csv`
-   `data/processed/ethereum_quarterly_forecast_comparison.csv`

## 14. Final conclusion

Ethereum quarterly transaction activity increased markedly between Q4
2015 and Q3 2026, with substantial variation in growth across periods.
The seasonal analysis did not find statistically significant
calendar-quarter differences after detrending. The original
transaction-count level series was non-stationary under the ADF test,
while its first difference was stationary.

Forecast evaluation showed that model complexity did not guarantee
better predictions. Although the piecewise linear trend and ARIMA(0,1,0)
with drift performed comparatively well in fixed-origin testing, the
naïve forecast achieved the best accuracy in rolling-origin
one-quarter-ahead validation.

The most robust empirical finding is the positive relationship between
quarterly active-address growth and quarterly transaction growth. The
association remains statistically significant in the multiple growth
model with HC3 robust standard errors and in the simpler one-variable
model. By contrast, ETH-price and gas-price growth do not show
statistically significant independent coefficients in the multiple
growth regression.

The evidence therefore supports active-address growth as a strong
contemporaneous indicator of Ethereum transaction growth in this sample.
It does not establish causality or a long-run equilibrium relationship.
Further observations, formal cointegration testing, and continued
out-of-sample validation are needed before making stronger claims.
