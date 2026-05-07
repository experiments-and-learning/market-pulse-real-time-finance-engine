# MarketPulse — Phase 1: API to JSON

[![Phase](https://img.shields.io/badge/Phase-1%20of%204-blue)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)
[![Stack](https://img.shields.io/badge/Stack-Python%20%7C%20Alpha%20Vantage%20%7C%20JSON-informational)](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/api-to-json)

Fetches IBM daily stock data from the Alpha Vantage API and caches it as a local JSON file (`data.json`). The cache avoids redundant API calls during downstream development and testing.

**Part of:** [MarketPulse: Real-Time Finance Engine](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine)

---

## What This Phase Does

1. Reads the Alpha Vantage API key from a `.env` file using `os.getenv()`
2. Calls the `TIME_SERIES_DAILY` endpoint for IBM stock
3. Dumps the raw JSON response to `data.json` with `indent=4` for human readability
4. Reads `data.json` back and prints it to verify the write succeeded

---

## Directory Structure

    api-to-json/
    ├── real_time_market_pulse_dashboard_api_to_json.py   ← main script
    ├── requirements.txt
    ├── LICENSE
    └── README.md

`data.json` is generated at runtime and is not committed — it is listed in `.gitignore`.

---

## Prerequisites

- Python 3.8+
- A free [Alpha Vantage API key](https://www.alphavantage.co/support/#api-key)

---

## Setup and Run

    # 1. Clone and switch to this branch
    git clone https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine.git
    cd market-pulse-real-time-finance-engine
    git checkout api-to-json

    # 2. Install dependencies
    pip install -r requirements.txt

    # 3. Create a .env file in the project root
    # Add the following line:
    #   Alpha_Vantage_Key=your_api_key_here

    # 4. Run
    python real_time_market_pulse_dashboard_api_to_json.py

Output: `data.json` is written to the project root and its contents are printed to console.

---

## Key Design Decisions

- `indent=4` is passed to `json.dump()` to make the file human-readable and prevent the entire response being written as one compressed line.
- The file is opened with `'w'` (write) to create or overwrite, then re-opened with `'r'` (read-only) to verify the write.
- Local caching means Phases 2–4 can be developed and tested without consuming the free tier's 25 daily API calls.

---

## Next Phase

[Phase 2: Data Pipeline](https://github.com/experiments-and-learning/market-pulse-real-time-finance-engine/tree/data-pipeline) — parses `data.json` to extract OHLCV fields.

---

## License

MIT License
