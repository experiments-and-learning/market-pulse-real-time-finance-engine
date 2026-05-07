# MarketPulse: Real-Time Finance Engine

[![Type](https://img.shields.io/badge/Type-Personal_Project-blue)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)
[![Status](https://img.shields.io/badge/Status-Active_Development-green)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)

An end-to-end data engineering project that builds a financial data pipeline from API ingestion to SQL-backed analysis. The project uses IBM stock data from the Alpha Vantage API and is developed incrementally across four branches, each representing a distinct phase of the pipeline.

---

## Project Phases

| Branch | Phase | What It Does |
|---|---|---|
| [`api-to-json`](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/api-to-json) | Phase 1 | Fetches IBM daily stock data from Alpha Vantage and caches it as `data.json` |
| [`data-pipeline`](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/data-pipeline) | Phase 2 | Parses `data.json` to extract and print OHLCV metrics per trading day |
| [`mysql-integration`](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/mysql-integration) | Phase 3 | Writes parsed OHLCV records to a MySQL table with upsert logic |
| [`stock_analysis`](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/stock_analysis) | Phase 4 | Computes 5-day moving averages, daily returns, and BUY/SELL signals; writes enriched data back to MySQL |

---

## Full Pipeline Overview

    Alpha Vantage API (IBM TIME_SERIES_DAILY)
          │
          ▼
    [Phase 1] real_time_market_pulse_dashboard_api_to_json.py
              └── writes ──► data.json  (local cache)
          │
          ▼
    [Phase 2] real_time_market_pulse_dashboard_data_pipeline.py
              └── reads data.json ──► prints OHLCV per date to console
          │
          ▼
    [Phase 3] real_time_market_pulse_dashboard_mysql_integration.py
              └── inserts OHLCV rows ──► MySQL: ibm_daily_stock
                  (ON DUPLICATE KEY UPDATE — safe to re-run)
          │
          ▼
    [Phase 4] real_time_market_pulse_dashboard_stock_analyzer.py
              ├── computes ma_5 (5-day SMA), daily_return, BUY/SELL signal
              └── writes enriched data ──► MySQL: ibm_analyzed_data

---

## Tech Stack

| Phase | Stack |
|---|---|
| API ingestion | Python, Alpha Vantage API, `requests`, `python-dotenv`, JSON |
| Parsing | Python, `json` |
| Database | MySQL, `mysql-connector-python`, `python-dotenv` |
| Analysis | `pandas`, `numpy`, `SQLAlchemy`, `mysql-connector-python` |

---

## Prerequisites

- Python 3.8+
- MySQL 8.0+
- A free [Alpha Vantage API key](https://www.alphavantage.co/support/#api-key)

---

## Repository Contents

    market-pulse-real-time-finance-engine/  (main branch)
    ├── .gitignore
    ├── LICENSE
    └── README.md

All source code lives in the phase branches above.

---

## Setup

Clone the repository and check out the branch for the phase you want to run:

    git clone https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine.git
    cd market-pulse-real-time-finance-engine
    git checkout api-to-json   # or: data-pipeline | mysql-integration | stock_analysis

Each branch has its own README with specific setup and run instructions.

---

## License

MIT License
