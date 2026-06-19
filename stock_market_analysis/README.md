# Stock Market Analysis — Data Collection & EDA (Python)

> 🔗 This project continues with SQL analysis in the [sql_projects repo](https://github.com/tharunsajeev/sql_projects/tree/main/stock_market_analysis) — see that repo for the full set of analytical queries run on this same dataset.

## Project Overview

Collects live daily stock market data for 6 Indian stocks (RELIANCE, TCS, INFY, HDFCBANK, WIPRO, ICICIBANK) and 2 US stocks (AAPL, MSFT) via the Alpha Vantage API, cleans and structures it with pandas, then explores returns, volatility, and trend direction through visualization.

## Tools Used

- `requests` — API calls to Alpha Vantage
- `pandas` — data cleaning, aggregation, window calculations
- `matplotlib` — price charts and moving average overlays

## Data Source

Daily OHLCV (Open, High, Low, Close, Volume) data from the [Alpha Vantage](https://www.alphavantage.co/) `TIME_SERIES_DAILY` endpoint — 100 trading days per ticker, Jan 16 – Jun 11, 2026.

## What's in This Notebook

1. **API setup & data collection** — fetch function with error handling for rate limits and invalid responses
2. **Data cleaning** — type conversion, null checks, combining 8 tickers into one DataFrame
3. **Exploratory analysis** — period returns, daily volatility (high-low range), monthly trend
4. **Visualization** — individual price charts per stock, 20-day moving average overlay

## Key Findings

- **AAPL was the only stock with a positive return (+19.3%)** over the 100-day window; every other tracked stock declined.
- **TCS (-30.4%) and INFY (-31.5%)** were the worst performers, consistent with broader pressure on Indian IT stocks tied to US client spending and AI disruption concerns.
- **TCS and INFY also showed the highest day-to-day volatility**, despite (or because of) their steep declines.
- The 20-day moving average confirmed a sustained downtrend across Indian stocks and a sustained uptrend in AAPL, visible clearly when each stock is plotted on its own price scale.

## Challenges & How They Were Solved

- **API rate limiting** — Alpha Vantage's free tier allows 5 calls/minute. Added `time.sleep()` between requests and a retry step for any ticker that failed mid-loop.
- **Symbol formatting** — Indian tickers needed exchange suffixes (`.BSE`); a typo here produced a silent "Invalid API call" error, traced by comparing `response.url` against a known working request.
- **Scale mismatch in charts** — plotting all 8 stocks on one shared axis made AAPL's price movement look flat, since Indian stock prices are 5-10x higher. Solved by giving each stock its own subplot.

## How to Run

1. Get a free API key from [alphavantage.co](https://www.alphavantage.co/support/#api-key)
2. Open `stock_analysis.ipynb` in Google Colab
3. Add your key to Colab's secret manager as `ALPHA_VANTAGE_KEY` and load it with `userdata.get('ALPHA_VANTAGE_KEY')`
4. Run all cells top to bottom — the fetch loop takes ~3 minutes due to rate limiting

## Author

[Tharun Sajeev](https://github.com/tharunsajeev)
