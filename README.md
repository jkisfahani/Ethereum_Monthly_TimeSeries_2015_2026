# Ethereum Blockchain Analytics: Monthly and Quarterly Time-Series Analysis (2015--2026)

## 1. Project Overview

This project examines the evolution of Ethereum blockchain activity from
2015 to 2026, focusing primarily on monthly and quarterly
transaction-count time series.

It combines empirical research with practical data-science methods to
investigate long-term trends, growth dynamics, seasonal patterns,
stationarity, structural changes, network-demand indicators, and
forecasting behaviour.

The project has two complementary objectives:

-   **Academic research:** Investigate the temporal dynamics of Ethereum
    network activity and identify patterns in blockchain transaction
    behaviour.
-   **Data-science portfolio:** Demonstrate an analytical workflow
    involving blockchain data collection, SQL querying, data
    preparation, statistical analysis, time-series modelling,
    forecasting, and visualization.

## 2. Research Objectives

The main objectives are to:

1.  Examine the historical evolution of Ethereum transaction activity.
2.  Identify long-term trends and changes in transaction growth.
3.  Investigate monthly and quarterly seasonal patterns.
4.  Assess stationarity and temporal dependence in transaction-count
    time series.
5.  Develop and evaluate statistical time-series models.
6.  Compare forecasting approaches and their predictive performance.
7.  Investigate structural changes in network activity.
8.  Explore relationships between transaction activity and other
    indicators of network demand.

## 3. Data Source and Study Period

The project uses Ethereum blockchain data obtained through Google
BigQuery public datasets.

The monthly source series contains 136 observations spanning August 2015
to November 2026. After excluding incomplete boundary observations, the
principal monthly analysis uses 134 complete observations:

-   **Start:** September 2015
-   **End:** October 2026
-   **Frequency:** Monthly

The data are also aggregated into quarterly transaction-count
observations for the complementary quarterly analysis.

Original source files are retained locally and are not included in the
GitHub repository. Processed analytical datasets are stored in
`data/processed/`.

## 4. Monthly Time-Series Analysis

**Notebook:**
[`01_Ethereum_Monthly_TimeSeries_Analysis.ipynb`](notebooks/01_Ethereum_Monthly_TimeSeries_Analysis.ipynb)

The monthly notebook covers data preparation, exploratory analysis,
statistical investigation, and preliminary ARIMA/SARIMA modelling.

### 4.1 Data collection and preparation

-   Load monthly Ethereum transaction data obtained through BigQuery.
-   Import and validate the dataset using Python.
-   Check data types, missing observations, and duplicate months.
-   Select complete monthly observations.
-   Prepare and save the cleaned analytical dataset.

### 4.2 Descriptive and trend analysis

The analysis includes descriptive statistics and visualizations of
historical monthly transaction activity, including a 12-month moving
average.

For the 134-month analytical period:

-   **Total transactions:** 3,822,444,185
-   **Mean monthly transactions:** approximately 28.53 million
-   **Median monthly transactions:** approximately 32.03 million
-   **Maximum monthly transactions:** 72,827,602 (April 2026)

### 4.3 Growth and seasonality

-   Calculate and examine year-over-year (YoY) transaction growth.
-   Analyze positive and negative growth periods.
-   Calculate average transaction counts by calendar month.
-   Calculate monthly seasonal indices and compare activity with the
    overall average.

### 4.4 Decomposition and stationarity

The monthly series is examined using classical additive decomposition
with a 12-month seasonal period. Stationarity is investigated using:

-   Augmented Dickey--Fuller (ADF) test.
-   Kwiatkowski--Phillips--Schmidt--Shin (KPSS) test.

The analysis considers ordinary differencing, seasonal differencing,
combined differencing, and logarithmic transformation.

### 4.5 Autocorrelation and modelling

Autocorrelation and partial autocorrelation are examined using ACF and
PACF analyses, including calculations up to 36 lags.

A baseline ARIMA(1,1,1) model and three candidate specifications are
compared:

-   ARIMA(0,1,1)
-   ARIMA(1,1,0)
-   ARIMA(1,1,1)

Model comparison uses Akaike Information Criterion (AIC), Bayesian
Information Criterion (BIC), and log-likelihood. ARIMA(0,1,1) achieved
the lowest AIC and BIC among these three candidate models.

### 4.6 Diagnostics and preliminary SARIMA analysis

Residual diagnostics include residual plots, distribution analysis,
Ljung--Box autocorrelation testing, Jarque--Bera normality diagnostics,
and heteroskedasticity assessment.

Seasonal modelling was also investigated using
**SARIMA(0,1,1)(0,1,1)\[12\]**. These modelling results are exploratory.
Residual non-normality and other diagnostic limitations should be
considered before treating the models as validated forecasting tools.

### 4.11 Comprehensive Monthly Report

The full report is available at [`reports/10_comprehensive_monthly_time_series_analysis.md`](reports/10_comprehensive_monthly_time_series_analysis.md). It documents the monthly dataset, descriptive statistics, growth analysis, calendar-month comparisons, decomposition, stationarity tests, ARIMA comparison, preliminary SARIMA results, diagnostics, and study limitations.

## 5. Quarterly Time-Series Analysis

**Notebook:**
[`02_Ethereum_Quarterly_TimeSeries_Analysis.ipynb`](notebooks/02_Ethereum_Quarterly_TimeSeries_Analysis.ipynb)

The quarterly analysis provides a complementary view of Ethereum network
activity by examining broader temporal patterns and forecasting
behaviour.

Topics include:

-   Quarterly transaction counts.
-   Quarter-over-quarter (QoQ) and year-over-year (YoY) growth.
-   Quarterly seasonal indices.
-   Time-series decomposition and residual analysis.
-   First-order differencing and logarithmic transformation.
-   ACF/PACF analysis and quarterly growth rates.
-   Detrended quarterly activity.
-   Forecast comparison and model evaluation.
-   Quadratic and piecewise linear trend forecasting.
-   Rolling-origin forecast evaluation.
-   Comparison of transaction growth with active-address growth.

Quarterly visualizations are stored in `visualizations/quarterly/`.

## 6. Additional Empirical Investigations

Two additional investigations extend the project beyond the principal
monthly and quarterly time-series analyses:

-   **Structural break analysis:** examines changes in the behaviour of
    Ethereum network activity over time.
-   **Empirical network-demand analysis:** investigates network-demand
    behaviour using transaction activity and related indicators.

The corresponding reports are listed in Section 7.

## 7. Research Reports

The `reports/` directory contains four detailed reports:

1.  [Monthly Time-Series
    Analysis](reports/10_comprehensive_monthly_time_series_analysis.md)
2.  [Structural Break Analysis](reports/14_structural_break_analysis.md)
3.  [Empirical Network-Demand
    Analysis](reports/19_empirical_network_demand_analysis.md)
4.  [Quarterly Time-Series
    Analysis](reports/20_comprehensive_quarterly_time_series_analysis.md)

## 8. Repository Structure

``` text
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
├── visualizations/
│   ├── monthly/              # Monthly analytical figures
│   └── quarterly/            # Quarterly analytical figures
│
├── reports/
│   ├── 10_comprehensive_monthly_time_series_analysis.md
│   ├── 14_structural_break_analysis.md
│   ├── 19_empirical_network_demand_analysis.md
│   └── 20_comprehensive_quarterly_time_series_analysis.md
│
├── .gitignore
└── README.md
```

## 9. Tools and Technologies

  Category                 Technologies
  ------------------------ ----------------------
  Programming              Python
  Data manipulation        Pandas, NumPy
  Data visualization       Matplotlib
  Statistical analysis     Statsmodels
  Time-series modelling    ARIMA, SARIMA
  Data querying            SQL, Google BigQuery
  Analytical environment   Jupyter Notebook
  Version control          Git, GitHub

## 10. Visualizations

The project includes two separate collections of figures, organized by analytical frequency:

### Monthly visualizations

The `visualizations/monthly/` folder contains figures generated during the monthly Ethereum time-series analysis (2015–2026). These figures support the investigation of long-term transaction activity, trends, growth patterns, seasonality, time-series decomposition, statistical properties, and forecasting results.

### Quarterly visualizations

The `visualizations/quarterly/` folder contains figures generated during the quarterly Ethereum time-series analysis. These include transaction activity, quarter-over-quarter and year-over-year growth, seasonal indices, detrended activity, time-series decomposition, residual diagnostics, autocorrelation analysis, transformations, and forecast comparisons.

Both collections are referenced in their corresponding research reports.

## 11. Scope and Limitations

This project primarily examines transaction-count dynamics. Transaction
counts are an indicator of blockchain activity, but they do not
independently measure economic value, unique users, or overall network
adoption.

Statistical forecasts are conditional projections based on historical
observations. They should not be interpreted as predictions of Ethereum
market prices or investment returns.

Model-selection and diagnostic results should be interpreted in light of
the assumptions and limitations of each time-series specification.
Exploratory results are distinguished from validated forecasting
performance.

## 12. Project Status

The project includes data collection and preparation, exploratory
analysis, monthly and quarterly time-series investigations, statistical
modelling, forecasting experiments, visualizations, and empirical
reports.

The repository is being organized as a reproducible research record and
a professional portfolio demonstrating blockchain data analytics and
applied statistical modelling.

## 13. Author

Independent research and data-science project focused on Ethereum
blockchain analytics, empirical time-series analysis, and statistical
forecasting.
