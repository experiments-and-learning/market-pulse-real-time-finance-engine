# MarketPulse — Phase 4: Stock Analysis

[![Phase](https://img.shields.io/badge/Phase-4%20of%204-blue)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)
[![Stack](https://img.shields.io/badge/Stack-Python%20%7C%20Pandas%20%7C%20NumPy%20%7C%20SQLAlchemy-informational)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/stock_analysis)

Reads `ibm_daily_stock` from MySQL via SQLAlchemy, computes financial indicators, and writes an enriched analysis table (`ibm_analyzed_data`) back to MySQL.

**Part of:** [MarketPulse: Real-Time Finance Engine](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)

---

## What This Phase Does

1. Connects to MySQL via SQLAlchemy using a `mysql+mysqlconnector` URL built from `.env`
2. Pulls all rows from `ibm_daily_stock` into a Pandas DataFrame, ordered by date ascending
3. Computes a 5-day simple moving average (`ma_5`) of the closing price using `.rolling(window=5).mean()`
4. Computes the daily percentage return (`daily_return`) using `.pct_change() * 100`
5. Generates a `signal` column: `BUY` where `close_price > ma_5`, `SELL` otherwise
6. Finds the day with the maximum daily return and prints its surrounding 2-day context window
7. Writes the enriched DataFrame (with `ma_5`, `daily_return`, `signal`) to a new MySQL table `ibm_analyzed_data`

---

## Directory Structure

    stock_analysis/
    ├── real_time_market_pulse_dashboard_stock_analyzer.py   ← main script
    ├── requirements.txt
    ├── LICENSE
    └── README.md

---

## Database Schema

Reads from `ibm_daily_stock` (created in Phase 3). Writes to `ibm_analyzed_data`:

    | Field        | Type   |
    |--------------|--------|
    | trade_date   | date   |
    | open_price   | double |
    | high_price   | double |
    | low_price    | double |
    | close_price  | double |
    | volume       | bigint |
    | ma_5         | double |
    | daily_return | double |
    | signal       | text   |

The table is replaced on each run (`if_exists='replace'`).

---

## Signal Logic

    ma_5    = average of close_price over the last 5 trading days

    signal:
      close_price > ma_5  →  BUY
      close_price ≤ ma_5  →  SELL

Note: the first 4 rows will have `NaN` for `ma_5` and `daily_return` because there is insufficient preceding data for a 5-day window. This is expected behaviour.

---

## Prerequisites

- Python 3.8+
- MySQL 8.0+ with data loaded from Phase 3

---

## Setup and Run

    # 1. Clone and switch to this branch
    git clone https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine.git
    cd market-pulse-real-time-finance-engine
    git checkout stock_analysis

    # 2. Install dependencies
    pip install -r requirements.txt

    # 3. Ensure Phase 3 has been run and ibm_daily_stock is populated

    # 4. Create a .env file with:
    #   DB_HOST=localhost
    #   DB_USER=root
    #   DB_PASSWORD=your_mysql_password
    #   DB_NAME=real_time_market_pulse_dashboard

    # 5. Run
    python real_time_market_pulse_dashboard_stock_analyzer.py

Output: Summary statistics, signal table, and max-return context window printed to console. `ibm_analyzed_data` table written to MySQL.

---

## Previous Phase

[Phase 3: MySQL Integration](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/mysql-integration)

---

## License

MIT License
