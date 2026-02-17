# Global Engine Titan

**Autonomous algorithmic trading engine screening 500+ global assets for momentum and volatility anomalies.**

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Assets](https://img.shields.io/badge/Universe-500%2B%20Assets-orange)

---

## Overview

Global Engine Titan is a fully automated trading system that ranks and allocates capital across a diversified universe of 500+ global assets spanning equities, ETFs, REITs, commodities, bonds, and crypto. The engine uses a momentum-to-volatility scoring framework with weekly rebalancing, dynamic position sizing, and hard stop-loss logic.

## Performance (3-Year Backtest)

| Metric | Value |
|---|---|
| Cumulative Return | ~192% |
| Annualized Return | ~43% |
| Sharpe Ratio | ~1.41 |
| Max Drawdown | ~-18% |
| Rebalance Frequency | Weekly (Fridays) |
| Commission Model | IBKR Tiered |

> **Disclaimer:** Past performance does not guarantee future results. This is a research backtest, not investment advice.

## Architecture

```
┌─────────────────────────────────────────────┐
│              DATA LAYER                      │
│  yfinance → 500+ tickers → 3yr history      │
├─────────────────────────────────────────────┤
│              SIGNAL ENGINE                   │
│  90-day momentum ÷ 30-day volatility         │
│  → rank → filter positive momentum only      │
├─────────────────────────────────────────────┤
│              PORTFOLIO CONSTRUCTION          │
│  Top 25 assets · sector caps (35%)           │
│  Score-weighted allocation                   │
├─────────────────────────────────────────────┤
│              RISK MANAGEMENT                 │
│  Hard stop-loss (-7%) · daily monitoring     │
│  IBKR tiered commission model                │
├─────────────────────────────────────────────┤
│              EXECUTION                       │
│  Weekly rebalance (Fridays)                  │
│  Full liquidate → re-enter cycle             │
└─────────────────────────────────────────────┘
```

## Asset Universe (7 Categories)

| Category | Tickers | Examples |
|---|---|---|
| Aggressive Growth | ~95 | NVDA, TSLA, ARKK, SOXL |
| Defense / Low Vol | ~58 | JNJ, KO, SCHD, USMV |
| Commodities | ~47 | GLD, USO, DBC, COPX |
| REITs | ~48 | VNQ, SCHH, IYR, XLRE |
| Global / Thematic | ~95 | VT, EEM, BOTZ, HACK |
| Bonds / Hedges | ~77 | TLT, AGG, HYG, EMB |
| Crypto | ~31 | IBIT, FBTC, GBTC, ETHE |

## How It Works

1. **Data Download** — Fetches 3+ years of adjusted close prices for all tickers via `yfinance`
2. **Signal Scoring** — For each asset, computes `momentum(90d) / volatility(30d)` ratio
3. **Selection** — Picks the top 25 assets with positive momentum, respecting a 35% max per category
4. **Weighting** — Score-proportional allocation across selected assets
5. **Stop-Loss** — Daily check: any position down 7% from entry is liquidated immediately
6. **Rebalance** — Every Friday: full liquidation then re-entry based on fresh rankings
7. **Commission Tracking** — Realistic IBKR tiered fees applied to every trade

## Quick Start

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/global-engine-titan.git
cd global-engine-titan

# Install dependencies
pip install -r requirements.txt

# Run the backtest
python global_engine_titan.py
```

## Configuration

Key parameters in `global_engine_titan.py`:

```python
INITIAL_CAPITAL = 100_000    # Starting capital ($)
LOOKBACK_MOM    = 90         # Momentum window (days)
LOOKBACK_VOL    = 30         # Volatility window (days)
TOP_N           = 25         # Max positions
MAX_SECTOR_WEIGHT = 0.35     # Max allocation per category
STOP_LOSS_PCT   = -0.07      # Hard stop-loss threshold
REBALANCE_DAY   = "Friday"   # Weekly rebalance day
```

## Requirements

- Python 3.9+
- See `requirements.txt` for dependencies

## License

MIT License — see [LICENSE](LICENSE) for details.

---

*Built by [Maxime Perriard](https://github.com/YOUR_USERNAME)*
