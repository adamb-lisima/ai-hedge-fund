---
inclusion: manual
---

# Financial Datasets API Reference

The project uses [financialdatasets.ai](https://financialdatasets.ai/) as its sole financial data provider. The API key is set via `FINANCIAL_DATASETS_API_KEY` in `.env`. Data for AAPL, GOOGL, MSFT, NVDA, and TSLA is available without a key.

## Endpoints Used

The API client lives in `src/tools/api.py`. All requests go through `_make_api_request()` with retry logic (max 3 retries).

### Prices
- `GET /prices` — OHLCV price data
- Returns: `{ ticker, open, close, high, low, volume, time }`
- Used by: technicals agent, backtesting engine

### Financial Metrics
- `GET /financial-metrics` — Pre-computed ratios and metrics
- Returns fields like: `market_cap`, `pe_ratio`, `price_to_book`, `gross_margin`, `operating_margin`, `net_margin`, `roe`, `roa`, `roic`, `debt_to_equity`, `current_ratio`, `revenue_growth`, `earnings_growth`, `free_cash_flow_yield`, `peg_ratio`, etc.
- Used by: fundamentals agent, valuation agent

### Line Items (Search)
- `POST /financials/search/line-items` — Flexible search across financial statements
- Returns dynamic fields based on search query (income statement, balance sheet, cash flow items)
- Used by: various analyst agents for deep-dive analysis

### Income Statements (example response shape)
```json
{
  "income_statements": [{
    "ticker": "AAPL",
    "calendar_date": "2023-12-31",
    "report_period": "2023-09-30",
    "period": "annual",
    "revenue": 383285000000,
    "cost_of_revenue": 214137000000,
    "gross_profit": 169148000000,
    "operating_expense": 54847000000,
    "selling_general_and_administrative_expenses": 24932000000,
    "research_and_development": 29915000000,
    "operating_income": 114301000000,
    "interest_expense": 3933000000,
    "ebit": 117669000000,
    "income_tax_expense": 16741000000,
    "net_income": 96995000000,
    "net_income_common_stock": 96995000000,
    "earnings_per_share": 6.16,
    "earnings_per_share_diluted": 6.13,
    "dividends_per_common_share": 0.94,
    "weighted_average_shares": 15744231000,
    "weighted_average_shares_diluted": 15812547000
  }]
}
```

### Insider Trades
- `GET /insider-trades` — Insider buying/selling activity
- Returns: `{ ticker, issuer, name, title, is_board_director, transaction_date, transaction_shares, transaction_price_per_share, transaction_value, shares_owned_before/after, filing_date }`
- Used by: sentiment agent

### Company News
- `GET /news` — Recent news articles with optional sentiment
- Returns: `{ ticker, title, author, source, date, url, sentiment }`
- Used by: sentiment agent, news sentiment agent

### Market Cap
- `GET /financial-metrics` (filtered) — Current market capitalization
- Used by: risk manager for position sizing

## Data Models

All response models are defined in `src/data/models.py` as Pydantic BaseModels. Key models:
- `Price` — OHLCV candle
- `FinancialMetrics` — ~40 pre-computed financial ratios
- `LineItem` — Dynamic financial statement line item (uses `extra="allow"`)
- `InsiderTrade` — Insider transaction record
- `CompanyNews` — News article with sentiment
- `CompanyFacts` — Company metadata (industry, sector, employees, etc.)

## Conventions
- All monetary values are in raw numbers (not thousands/millions) — e.g. revenue of 383285000000 = $383.285B
- Dates use `YYYY-MM-DD` format
- Period types: `"annual"`, `"quarterly"`, `"ttm"`
- `report_period` is the fiscal period end date, `calendar_date` is the calendar year-end
- Nullable fields use `float | None` pattern
