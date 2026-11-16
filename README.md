# Ethereum Blockchain Analytics: Monthly and Quarterly Time-Series Analysis (2015–2026)

## 1. Project Overview

This project investigates the evolution of Ethereum blockchain activity from 2015 to 2026 through monthly and quarterly transaction-count time-series analysis.

It combines empirical research with practical data science methods to examine long-term trends, growth dynamics, seasonal patterns, stationarity, structural changes, and forecasting behaviour in Ethereum network activity.

The project has two complementary objectives:

- **Academic research:** To investigate the temporal dynamics of Ethereum network activity and identify meaningful patterns in blockchain transaction behaviour.
- **Data science portfolio:** To demonstrate an end-to-end analytical workflow involving blockchain data collection, SQL querying, data preparation, statistical analysis, time-series modelling, forecasting, and visualization.

## 2. Research Objectives

1. Examine the historical evolution of Ethereum transaction activity.
2. Identify long-term trends and changes in transaction growth.
3. Investigate monthly and quarterly seasonal patterns.
4. Assess stationarity and temporal dependence in transaction-count time series.
5. Develop and evaluate statistical time-series models.
6. Compare forecasting approaches and their predictive performance.
7. Investigate relationships between transaction activity and other indicators of network demand.

## 3. Data Source and Study Period

The project uses Ethereum blockchain data obtained through Google BigQuery public datasets.

The original monthly dataset contains 136 observations spanning August 2015 to November 2026.

Following data validation and the exclusion of incomplete boundary months, the principal monthly analysis uses 134 complete observations:

- Start: September 2015
- End: October 2026
- Frequency: Monthly

The data were aggregated into monthly and quarterly transaction-count time series.

Raw source files are retained locally and excluded from the GitHub repository. Processed analytical datasets are included in `data/processed/`.

## 4. Monthly Time-Series Analysis

**Notebook:** `01_Ethereum_Monthly_TimeSeries_Analysis.ipynb`

This notebook covers data collection, preparation, exploratory analysis, statistical investigation, and preliminary ARIMA/SARIMA modelling.

### 4.1 Data Collection and Preparation

- Loading monthly Ethereum transaction data obtained through BigQuery.
- Importing and validating the dataset using Python.
- Checking data types, missing observations, and duplicate months.
- Selecting complete monthly observations.
- Preparing and saving the cleaned analytical dataset.

### 4.2 Descriptive and Trend Analysis

- Descriptive statistics of monthly transaction counts.
- Total, mean, median, minimum, and maximum transaction counts.
- Visualization of historical monthly transaction activity.
- Calculation and visualization of a 12-month moving average.

For the 134-month analytical period:

- Total transactions: 3,822,444,185
- Mean monthly transactions: approximately 28.53 million
- Median monthly transactions: approximately 32.03 million
- Maximum monthly transactions: 72,827,602 (April 2026)

### 4.3 Transaction Growth Analysis

- Calculation of year-over-year (YoY) transaction growth.
- Descriptive analysis of monthly growth rates.
- Visualization of positive and negative growth periods.

### 4.4 Monthly Seasonality

- Calculation of average transaction counts by calendar month.
- Calculation of monthly seasonal indices.
- Comparison of monthly activity against the overall average.
- Visualization of seasonal patterns.

### 4.5 Time-Series Decomposition

- Preparation of a monthly frequency time series.
- Classical additive decomposition.
- Examination of trend, seasonal, and residual components using a 12-month seasonal period.

### 4.6 Stationarity and Differencing

Stationarity was investigated using two complementary statistical tests:

- Augmented Dickey–Fuller (ADF).
- Kwiatkowski–Phillips–Schmidt–Shin (KPSS).

The analysis included:

- Stationarity testing of the original series.
- First-order differencing.
- Stationarity testing after differencing.
- Seasonal differencing with a 12-month period.
- Combined first-order and seasonal differencing.
- Natural logarithmic transformation and differencing.

### 4.7 Autocorrelation Analysis

- Autocorrelation function (ACF).
- Partial autocorrelation function (PACF).
- Numerical ACF/PACF calculations up to 36 lags.
- Investigation of autocorrelation after ordinary and combined differencing.

### 4.8 ARIMA Modelling and Model Comparison

A baseline ARIMA(1,1,1) model was estimated.

Three candidate ARIMA specifications were subsequently compared:

- ARIMA(0,1,1)
- ARIMA(1,1,0)
- ARIMA(1,1,1)

Model comparison used:

- Akaike Information Criterion (AIC).
- Bayesian Information Criterion (BIC).
- Log-likelihood.

ARIMA(0,1,1) achieved the lowest AIC and BIC among the three candidate models.

### 4.9 Residual Diagnostics

Model diagnostics included:

- Residual time-series plots.
- Residual distribution analysis.
- Ljung–Box residual autocorrelation testing.
- Jarque–Bera normality diagnostics.
- Heteroskedasticity assessment.

### 4.10 Preliminary SARIMA Modelling

Seasonal ARIMA modelling was investigated using:

**SARIMA(0,1,1)(0,1,1)[12]**

The model incorporated ordinary and seasonal differencing to account for the temporal characteristics of monthly Ethereum transaction activity.

These modelling results are exploratory. Residual non-normality and other diagnostic limitations require consideration before treating the models as validated forecasting tools.

### 4.11 Comprehensive Monthly Report

The full report is available at [`reports/10_comprehensive_monthly_time_series_analysis.md`](reports/10_comprehensive_monthly_time_series_analysis.md). It documents the monthly dataset, descriptive statistics, growth analysis, calendar-month comparisons, decomposition, stationarity tests, ARIMA comparison, preliminary SARIMA results, diagnostics, and study limitations.

## 5. Quarterly Time-Series Analysis

**Notebook:** `02_Ethereum_Quarterly_TimeSeries_Analysis.ipynb`

The quarterly analysis provides a complementary perspective on Ethereum network activity, examining broader temporal patterns and forecasting behaviour.

The work includes:

- Quarterly transaction-count analysis.
- Quarter-over-quarter (QoQ) growth.
- Year-over-year (YoY) growth.
- Quarterly seasonal indices.
- Time-series decomposition and residual analysis.
- First-order differencing.
- Logarithmic transformation.
- ACF/PACF analysis.
- Quarterly growth-rate investigation.
- Detrended quarterly activity.
- Forecast comparison and model evaluation.
- Trend forecasting using quadratic and piecewise linear approaches.
- Rolling-origin forecast evaluation.

## 6. Additional Empirical Investigations

The project also includes investigations beyond the principal monthly and quarterly analyses.

These include:

- Structural break analysis.
- Empirical network-demand analysis.
- Comparison of transaction growth and active-address growth.
- Investigation of changes in Ethereum network activity over time.

Detailed reports are available in the `reports/` directory.

## 7. Repository Structure

```text
Ethereum_Monthly_TimeSeries_2015_2026/
│
├── data/
│   ├── raw/                 # Original source data (excluded from GitHub)
│   └── processed/           # Processed analytical datasets
│
├── notebooks/
│   ├── 01_Ethereum_Monthly_TimeSeries_Analysis.ipynb
│   ├── 02_Ethereum_Quarterly_TimeSeries_Analysis.ipynb
│   └── 04_BigQuery_SQL_Query_Archive.ipynb
│
├── visualizations/          # Monthly and quarterly analytical figures
│
├── reports/
│   ├── 14_structural_break_analysis.md
│   ├── 19_empirical_network_demand_analysis.md
│   └── 20_comprehensive_quarterly_time_series_analysis.md
│
├── .gitignore
└── README.md
```

## 8. Tools and Technologies

| Category | Technologies |
|---|---|
| Programming | Python |
| Data manipulation | Pandas, NumPy |
| Data visualization | Matplotlib |
| Statistical analysis | Statsmodels |
| Time-series modelling | ARIMA, SARIMA |
| Data querying | SQL, Google BigQuery |
| Analytical environment | Jupyter Notebook |
| Version control | Git, GitHub |

## 9. Research Reports and Visualizations

The `reports/` directory contains detailed analytical reports covering structural breaks, empirical network demand, and quarterly time-series analysis.

The `visualizations/` directory contains 27 figures illustrating monthly and quarterly transaction activity, growth rates, seasonal patterns, decomposition, model diagnostics, and forecasting comparisons.

## 10. Scope and Limitations

This project primarily examines transaction-count dynamics.

Transaction counts provide an indicator of blockchain activity but do not independently measure economic value, unique users, or overall network adoption.

Statistical forecasts are conditional projections based on historical observations. They should not be interpreted as predictions of Ethereum market prices or investment returns.

Model selection and diagnostic results should be interpreted in the context of the assumptions and limitations of each time-series specification.

## 11. Project Status

The project includes data collection and preparation, exploratory analysis, monthly and quarterly time-series investigations, statistical modelling, forecasting experiments, visualizations, and empirical reports.

The repository is being organized as a reproducible research record and a professional portfolio demonstrating blockchain data analytics and applied statistical modelling.

## 12. Author

Independent research and data science project focused on Ethereum blockchain analytics, empirical time-series analysis, and statistical forecasting.