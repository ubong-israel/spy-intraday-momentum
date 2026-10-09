# spy-intraday-momentum

## Overview

This repository contains my WUTIS assignment on an intraday momentum strategy for the SPY ETF. I adapted the authors’ implementation from *Beat the Market: An Effective Intraday Momentum Strategy for the S&P 500 ETF (SPY)* and tested an additional entry filter.

## Baseline Strategy

The baseline uses a noise area based on historical intraday price movements and volume-weighted average price (VWAP) to determine long, short, or flat exposure. Trading decisions are evaluated every 30 minutes, positions are sized using historical volatility, and positions are closed at the end of the trading day.

## My Variation

I added a filter that requires stronger momentum before opening a position:

- **Long:** The current 30-minute return must be positive and greater than the maximum of the previous three interval returns.
- **Short:** The current 30-minute return must be negative and lower than the minimum of the previous three interval returns.
- When only one or two previous intervals are available that day, the filter uses those available intervals.
- The first trading decision of each day follows the baseline entry rule.

The filter applies to new entries, including reversals. Existing positions retain the baseline exit rules.

## Backtesting and Evaluation

The notebook compares the baseline, my variation, and SPY buy-and-hold price returns using:

- A chronological train/test split at January 1, 2026
- Commission and slippage assumptions
- Portfolio-value charts
- Sharpe ratios, annualized returns, and annualized volatility
- Rolling performance charts

The train/test split separates the reporting periods. It does not involve fitting a machine-learning model.

## Running the Notebook

The notebook includes saved tables and plots, so results can be viewed without rerunning it.

To reproduce the analysis, open the notebook in Jupyter, install the imported dependencies, provide your own Polygon API key when prompted, and run the cells in order. Access to the required historical market data is necessary.

## Limitations

Results depend on the sample period, data quality, execution assumptions, and transaction costs. The SPY benchmark uses price returns rather than dividend-reinvested total returns. Historical backtest performance does not guarantee future performance.

## Attribution

The baseline methodology and starting implementation come from the paper and accompanying code by the original authors. My contribution is the additional entry-confirmation filter and its comparison with the baseline.
- [Research paper — Carlo Zarattini, Andrew Aziz, and Andrea Barbon](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4824172)
- [Original Python implementation and explanation — Concretum Group](https://concretumgroup.com/python-backtesting-beat-the-market-an-effective-intraday-momentum-strategy-for-the-sp500-etf-spy/)
