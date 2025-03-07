# Entity-Relationship Model (ER.md)

## Entities & Attributes

### 1. Stock_Info
- **stock_number**: VARCHAR(6) - Pure stock number (e.g., "600519")
- **market_code**: VARCHAR(2) - Market code (e.g., "SH", "SZ")
- **stock_code**: VARCHAR(20) (Unique) - Combined code in suffix format (e.g., "600519.SH")
- **stock_name**: VARCHAR - Name of the stock
- **industry**: VARCHAR - Industry classification (e.g., "Technology")
- **listing_date**: DATE - Stock listing date
- **tracked_layer**: ENUM('stable','aggressive','both','none') - Tracks the stock's involvement in multiple strategies or combinations
- **tracked_strategies**: (Optional) **Many-to-many relationship** with stock_tracking table for multiple strategies tracking the stock

#### Primary Key: id (auto-increment)
#### Unique Constraint: (stock_number, market_code)
#### Indexes: 
- stock_code (unique)
- stock_number

### 2. Daily_Quote
- **dt**: DATE - Date of the stock data (this will now be part of the primary key)
- **stock_code**: VARCHAR (Foreign Key from Stock_Info)
- **open_price**: DECIMAL - Opening price
- **close_price**: DECIMAL - Closing price
- **high_price**: DECIMAL - Highest price
- **low_price**: DECIMAL - Lowest price
- **volume**: INT - Volume traded
- **turnover**: DECIMAL - Transaction turnover
- **adj_close_price**: DECIMAL - Adjusted close price (for handling dividends and stock splits)

#### Primary Key: (stock_code, date)

### 3. Finance_Data
- **stock_code**: VARCHAR (Foreign Key from Stock_Info)
- **roe**: DECIMAL - Return on equity
- **eps**: DECIMAL - Earnings per share
- **pb_ratio**: DECIMAL - Price to book ratio
- **pe_ratio**: DECIMAL - Price to earnings ratio
- **dividend_yield**: DECIMAL - Dividend yield
- **report_period**: ENUM('Q1', 'Q2', 'Q3', 'Q4', 'Annual') - Financial report period
- **announce_date**: DATE - The actual date the financial report was announced
- **dt**: DATE - The date this financial data corresponds to, linked to the period or financial year

#### Primary Key: (stock_code, report_period, date)

### 4. Funds_Flow
- **stock_code**: VARCHAR (Foreign Key from Stock_Info)
- **northbound_flow**: DECIMAL - Funds flowing into the stock from Northbound trading
- **margin_balance**: DECIMAL - Margin balance
- **short_balance**: DECIMAL - Balance of short selling activity
- **dt**: DATE - The date this fund flow corresponds to (ensures accurate time-based analysis)

#### Primary Key: (stock_code, date)

### 5. Signals
- **stock_code**: VARCHAR (Foreign Key from Stock_Info)
- **industry_score**: DECIMAL - Industry score (calculated dynamically)
- **valuation_score**: DECIMAL - Valuation score (calculated dynamically)
- **technical_score**: DECIMAL - Technical score (calculated dynamically)
- **chip_control_score**: DECIMAL - Chip control score (calculated dynamically)
- **composite_score**: DECIMAL - Composite score from various factors
- **buy_signal**: BOOLEAN - Flag indicating buy signal
- **sell_signal**: BOOLEAN - Flag indicating sell signal
- **take_profit_price**: DECIMAL - Target take profit price
- **stop_loss_price**: DECIMAL - Target stop loss price
- **signal_date**: DATE - Date the signal was generated
- **timestamp**: DATETIME - Timestamp (accurate to the minute) for tracking signal generation time
- **signal_origin**: ENUM('Backtest', 'Live') - Indicates whether the signal originated from backtesting or real-time analysis
- **strategy_version**: VARCHAR - Version of the strategy used to generate the signal
- **factor_version**: VARCHAR - Version of the factor calculation method used

#### Primary Key: (stock_code, signal_date, signal_origin)

## Relationships

- **Stock_Info** ↔ **Daily_Quote**: One-to-many (One stock can have multiple daily quotes)
- **Stock_Info** ↔ **Finance_Data**: One-to-many (One stock can have multiple financial data entries, with report period distinction)
- **Stock_Info** ↔ **Funds_Flow**: One-to-many (One stock can have multiple fund flow records on different dates)
- **Stock_Info** ↔ **Signals**: One-to-many (One stock can have multiple signals generated over time)
- **Stock_Info** ↔ **Industry_Dic**: One-to-many (One stock can be linked to multiple industry entries over time)

## Data Integration with AKShare

The data required for the above entities will be fetched from the respective functions in the `akshare.md` document:

### 1. **Stock_Info**
- **stock_number**, **market_code**, **stock_code**: 
  - Function: `stock_info_a_code_name`
  - Description: Stock identifiers in various formats. The system handles multiple input formats:
    - Pure number format (e.g., "600519")
    - Suffix format (e.g., "600519.SH")
    - Prefix format (e.g., "SH600519")
  - Note: Different AKShare APIs require different formats. Our system standardizes these internally.
- **stock_name**: 
  - Function: `stock_info_a_code_name`
  - Description: The name of the stock.
- **industry**: 
  - Function: `stock_info_a_code_name`
  - Description: The industry classification of the stock.
- **listing_date**: 
  - Function: `stock_info_a_code_name`
  - Description: The stock's listing date.
- **tracked_strategies**: 
  - Function: **Manual** (related to multiple strategy tracking, use `stock_tracking` table)

### 2. **Daily_Quote**
- **date**:
  - Function: `stock_sse_daily`
  - Description: Date of the stock data.
- **stock_code**:
  - Linked to `Stock_Info`'s `stock_code`.
- **open_price**, **close_price**, **high_price**, **low_price**, **volume**, **turnover**, **adj_close_price**:
  - Function: `stock_sse_daily`
  - Description: Stock data such as open, close, high, low prices, volume, and turnover.

### 3. **Finance_Data**
- **roe**, **eps**, **pb_ratio**, **pe_ratio**, **dividend_yield**:
  - Function: `stock_financial_summary`
  - Description: Financial data for the stock.
- **report_period**, **announce_date**, **date**:
  - Function: **Manual/AKShare** (for quarterly or annual data)

### 4. **Funds_Flow**
- **northbound_flow**, **margin_balance**, **short_balance**:
  - Function: `funds_flow_north`, `funds_flow_margin`, `funds_flow_short`
  - Description: Fund flow data for the stock.
- **date**:
  - Function: `funds_flow_north` (daily data)

### 5. **Signals**
- **industry_score**, **valuation_score**, **technical_score**, **chip_control_score**, **composite_score**, **buy_signal**, **sell_signal**, **take_profit_price**, **stop_loss_price**:
  - Aggregation: **Calculated dynamically** based on stock data from multiple sources, including industry, valuation, technical, and chip control scores.
  - Description: The generated buy/sell signal data.
- **signal_date**, **timestamp**, **signal_origin**, **strategy_version**, **factor_version**:
  - **Manual/Calculated** (based on strategy logic and system design)
"""