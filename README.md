# Cryptocurrency-volatility-and-portfolio-analytics-project

## Project overview

This project investigates whether one-day-ahead volatility forecasts can improve cryptocurrency portfolio risk management. It compares an econometric model (EGARCH), a nonlinear machine-learning benchmark (SVR) and a sequential neural network (LSTM), then converts the selected forecasts into a weekly inverse-volatility allocation.

The repository is a professional restructuring of an MSc Finance and FinTech coursework project. The original analysis was technically broad but existed as a single Colab-oriented script with repeated sections. This version preserves the strongest methodology while making the workflow modular, reproducible and easier to review.

Two execution formats are included:

- **`standalone_main.py`** contains the complete project in one file and is the recommended option for Google Colab.
- **`main.py` with `config.py` and `src/`** is the modular version for local development and code review.

Both formats implement the same research workflow. You do not need to run `config.py` before the standalone file.

## One-file Google Colab instructions

Upload only `standalone_main.py` to Colab. Then run these cells:

```python
!pip install -q yfinance arch scikit-learn tensorflow matplotlib pandas numpy
```

```python
%matplotlib inline
%run standalone_main.py --mode quick
```

For a longer research run:

```python
%run standalone_main.py --mode full
```

The standalone script ignores Colab's internal kernel arguments, downloads the data automatically and creates `outputs/figures/` and `outputs/tables/`.
When it detects Jupyter or Colab, it also displays every chart inline. Do not add `--skip-plots` if you want graphs to appear.

## Why the project matters

Cryptocurrency returns exhibit volatility clustering, heavy tails and changing correlations. A static allocation can therefore produce materially different risk exposure as market conditions change. Forecasting volatility may support more disciplined position sizing, but model accuracy, transaction costs and forecast uncertainty must be evaluated honestly.

## Research question

> Can volatility forecasts from EGARCH, SVR and LSTM improve risk-adjusted cryptocurrency portfolio performance relative to static and equal-weight benchmarks?

## Skills demonstrated

- Automatic market-data collection and validation
- Leakage-aware time-series feature engineering
- EGARCH volatility modelling
- Support-vector regression with time-series cross-validation
- LSTM sequence modelling with training-only scaling
- Out-of-sample model comparison
- Dynamic portfolio allocation and transaction costs
- VaR, CVaR, drawdown and risk contribution
- Monte Carlo scenario analysis
- Modular Python, tests and reproducible reporting

## Assets and data

The project uses Yahoo Finance daily data for:

- Bitcoin (`BTC-USD`)
- Ethereum (`ETH-USD`)
- Solana (`SOL-USD`)
- Binance Coin (`BNB-USD`)

The default sample begins in January 2020 and updates automatically. An offline fallback dataset is included solely to verify that the complete pipeline runs without a network connection.

## Models and analytics

### Forecasting models

1. **EGARCH(1,1), skewed-t:** interpretable benchmark for asymmetric, fat-tailed volatility.
2. **SVR with RBF kernel:** nonlinear benchmark using lagged volatility, return, momentum and volume features.
3. **Two-layer LSTM:** sequential model using a 60-day feature history, dropout and early stopping.

### Forecast evaluation

- RMSE and MAE
- Forecast correlation
- Volatility-regime accuracy
- Stress-period RMSE and MAE
- Approximate Diebold-Mariano tests
- Training and test error comparison where available

### Portfolio analytics

- Weekly inverse-forecast-volatility allocation
- 5% minimum and 60% maximum asset weights
- 10-basis-point transaction-cost assumption
- Static and equal-weight benchmarks
- Sharpe, Sortino and Calmar ratios
- Maximum drawdown
- Historical VaR and CVaR
- Euler risk contribution
- Long-only random portfolio frontier
- Correlated 90-day Monte Carlo scenario analysis

## Installation

Python 3.10 or 3.11 is recommended for the smoothest TensorFlow installation.

```bash
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

On macOS or Linux, activate the environment with:

```bash
source .venv/bin/activate
```
## Outputs

The pipeline creates up to 12 consistently formatted figures, including:

- Market performance
- Return distributions
- Correlation heatmap
- Realised volatility
- Selected-model forecasts
- Model error comparison
- Portfolio growth and drawdowns
- Dynamic weights
- Risk contribution
- Efficient frontier
- Monte Carlo paths

It also saves structured tables for:

- Prices and returns
- Descriptive statistics
- Engineered features
- Forecasts and model performance
- Diebold-Mariano comparisons
- Portfolio returns, weights and turnover
- Risk metrics and risk contribution
- Efficient-frontier simulations
- Monte Carlo summary
- Run assumptions and metadata

## Example offline result

A verified quick offline run completed the full calculation pipeline and produced model comparisons, dynamic weights, portfolio performance and Monte Carlo risk outputs. In that generated sample, the labelled ridge fallback achieved the lowest RMSE because optional modelling packages were intentionally unavailable. This result verifies the software workflow; it is not a financial conclusion.

## Methodology and bias controls

- Train and test periods are chronological.
- Feature and target scaling use training observations only.
- Every model feature is lagged by at least one day.
- Model comparisons use common out-of-sample observations.
- Portfolio weights are shifted before return calculation.
- Turnover incurs an explicit transaction cost.
- Fallback models are clearly labelled in outputs.

Full details are provided in [`docs/methodology.md`](docs/methodology.md).

## Tests

```bash
python -m unittest discover -s tests
```

Tests cover sequence ordering, forecast metrics, drawdown calculations, portfolio weights and risk contribution.

## Limitations

- Yahoo Finance is not an institutional-grade feed.
- Cryptocurrency market relationships are unstable.
- Model selection is sample-dependent.
- Monte Carlo scenarios assume stable volatility and correlation.
- The backtest simplifies execution, liquidity and market impact.
- LSTM results can vary across hardware and software versions.
- Forecast accuracy does not guarantee profitable allocation decisions.

## Future improvements

- Add walk-forward parameter re-estimation.
- Include jump or regime-switching volatility models.
- Use intraday data for realised-volatility construction.
- Add liquidity-aware position limits.
- Compare volatility targeting with CVaR-based allocation.
- Add formal model-calibration and stability monitoring.
