# Crypto Momentum vs Mean Reversion

A research and backtesting project that compares two systematic crypto trading approaches on hourly Binance spot data:

- **Time-series trend-following**
- **Cross-sectional mean-reversion**

The project studies whether momentum signals outperform mean-reversion once realistic trading frictions, turnover, and portfolio constraints are taken into account.

## Overview

This repository contains the data pipeline, strategy logic, portfolio construction, transaction cost model, and performance evaluation used to compare momentum and mean-reversion in a liquid Binance USDT universe. The analysis focuses on six major crypto assets:

- BTCUSDT
- ETHUSDT
- BNBUSDT
- XRPUSDT
- ADAUSDT
- DOGEUSDT

The sample is hourly and spans **2022-01-01 to 2026-03-15**. Strategies are implemented under a shared cost-aware portfolio construction framework so that the comparison reflects differences in the underlying signals rather than inconsistent sizing rules. :contentReference[oaicite:1]{index=1}

## Research Question

**Do momentum signals beat mean-reversion in crypto spot markets after slippage and turnover costs?**

This repo is built to answer that question using:

- hourly OHLCV market data
- excess return construction
- signal generation
- quadratic portfolio optimization
- slippage estimation
- gross and net PnL evaluation

## Main Findings

Under the report’s fixed **$100k gross exposure cap** framework, the results favor **trend-following** over **mean-reversion**.

- Trend-following remains profitable after estimated slippage
- Mean-reversion is unprofitable even before realistic costs are fully considered
- Turnover is the main weakness of the reversal strategy
- Financing assumptions matter a lot: the fixed-cap convention can make results look stronger than what a self-financing live account could realistically sustain :contentReference[oaicite:2]{index=2}

## Strategy Summary

### 1. Trend-Following
A medium-horizon time-series momentum strategy based on lagged return continuation. Signals are volatility-scaled and bounded to avoid extreme concentration.

### 2. Mean-Reversion
A short-horizon cross-sectional reversal strategy that buys recent relative losers and sells recent relative winners. The expected-return vector is demeaned to keep the portfolio approximately market-neutral.

### Shared Portfolio Construction
Both strategies use the same optimizer:

- rolling covariance estimation
- risk-aversion penalty
- turnover penalty
- gross exposure limit

This makes the comparison cleaner and more defensible. :contentReference[oaicite:3]{index=3}

## Transaction Costs

The project includes a one-way slippage model compatible with portfolio turnover. Asset-level slippage is estimated primarily using the **Roll** model, with **Corwin-Schultz** used as a fallback when needed. A turnover-weighted scalar cost is then used for portfolio-level net PnL attribution. :contentReference[oaicite:4]{index=4}

