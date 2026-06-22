# Stock Market Analysis (India vs US) - Python

> Continues with SQL analysis here: [sql-projects/stock_market_analysis](https://github.com/tharunsajeev/Sql-projects/tree/main/stock_market_analysis)

## What this is

I wanted a project where I sourced the data myself instead of using an already-cleaned Kaggle CSV, so this one starts from a live API. I pulled 100 days of daily price data for 6 Indian stocks (RELIANCE, TCS, INFY, HDFCBANK, WIPRO, ICICIBANK) and 2 US stocks (AAPL, MSFT), cleaned it with pandas, and explored returns, volatility, and trend direction.

## How it works

I used the Alpha Vantage API's `TIME_SERIES_DAILY` endpoint to pull OHLCV (open, high, low, close, volume) data for each stock, with `requests` handling the API calls and `time.sleep()` managing the free tier's 5 calls per-minute limit. Each response gets parsed into a pandas DataFrame, and all 8 stocks are combined into a single 800-row dataset.

From there the notebook covers:
- period returns for each stock (first close vs last close)
- volatility, measured as average daily high-low range
- a 20-day moving average plotted against the daily close price
- individual price charts per stock, since plotting all 8 on one shared scale made the US stocks look flat next to the higher-priced Indian ones

## What I found

AAPL was the only stock that gained value over the period, up about 19%. Every other stock I tracked lost value, and the Indian IT stocks dropped the hardest - TCS down roughly 30%, INFY down roughly 31%. That tracks with what's been happening more broadly with Indian IT this year: concerns about US clients cutting spending, and some nervousness around AI eating into the services these companies sell. TCS and INFY were also the most volatile stocks in the dataset, which makes sense given how sharp the decline was.

## Tools

Python, `requests`, `pandas`, `matplotlib`, Google Colab

## A few things that didn't work on the first try

- Alpha Vantage's rate limit kept cutting off my fetch loop partway through, so I added retry logic for whichever ticker failed
- Indian stocks need an exchange suffix like `.BSE` I had a typo in one ticker's symbol that caused a confusing "Invalid API call" error with no useful detail, and the only way I found the issue was printing the actual request URL and comparing it against one I knew worked
- My first price chart plotted all 8 stocks on the same axis, which made AAPL look almost flat since it trades in a completely different price range than the Indian stocks — switching to one subplot per stock fixed that


## How to Run

1. Get a free API key from [alphavantage.co](https://www.alphavantage.co/support/#api-key)
2. Open `stock_analysis.ipynb` in Google Colab
3. Add your key to Colab's secret manager as `ALPHA_VANTAGE_KEY` and load it with `userdata.get('ALPHA_VANTAGE_KEY')`
4. Run all cells. The data fetch takes a few minutes because of the rate limit.

## Author

[Tharun Sajeev](https://github.com/tharunsajeev)
