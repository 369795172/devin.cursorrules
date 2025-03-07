# Quantitative Trading System User Guide

## Introduction

The Quantitative Trading System is a comprehensive platform for financial data analysis, strategy development, and trading automation. This guide will walk you through setting up and using the system effectively.

## Table of Contents
1. [Getting Started](#getting-started)
2. [System Architecture](#system-architecture)
3. [Using the Dashboard](#using-the-dashboard)
4. [Stock Analysis](#stock-analysis)
5. [Strategy Backtesting](#strategy-backtesting)
6. [Portfolio Monitoring](#portfolio-monitoring)
7. [Trading Strategy Implementation](#trading-strategy-implementation)
8. [Risk Management](#risk-management)
9. [Customization](#customization)
10. [Troubleshooting](#troubleshooting)

## Getting Started

### Prerequisites
- Python 3.8 or higher
- Node.js 14 or higher
- PostgreSQL database
- Modern web browser

### Installation

1. **Clone the repository:**
   ```bash
   git clone [repository-url]
   cd qmt_general
   ```

2. **Create and activate virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install backend dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration (database credentials, API keys, etc.)
   ```

5. **Initialize database:**
   ```bash
   alembic upgrade head
   ```

6. **Install frontend dependencies:**
   ```bash
   cd frontend
   npm install
   cd ..
   ```

7. **Start the application:**
   ```bash
   # Start backend (in one terminal)
   cd backend
   uvicorn api.app:app --reload
   
   # Start frontend (in another terminal)
   cd frontend
   npm start
   ```

8. **Access the application:**
   - Frontend: http://localhost:3000
   - API documentation: http://localhost:8000/docs

## System Architecture

The system consists of two primary layers:

### Stable Layer
- Financial data monitoring
- Dividend announcement tracking
- Valuation alerts
- Manual decision support
- Long-term investment strategies

### Aggressive Layer
- Dynamic industry/stock selection
- Technical indicator analysis
- Automated trading signals
- Short-term trading strategies
- Risk management

## Using the Dashboard

The dashboard is the central control hub for monitoring your portfolio, market trends, and trading signals.

### Key Components
1. **Portfolio Summary**: View your current holdings, allocation, and performance metrics.
2. **Market Overview**: Monitor market indices, sector performance, and global market indicators.
3. **Alert Center**: Receive notifications for price movements, technical signals, and earnings announcements.
4. **Performance Metrics**: Track your strategies' performance with key metrics like Sharpe ratio, drawdown, and win rate.

### Customizing Your Dashboard
1. Click the "Settings" icon in the top-right corner
2. Select "Dashboard Settings"
3. Drag and drop widgets to reorganize
4. Configure each widget's settings for personalized data display

## Stock Analysis

The Stock Analysis tool provides comprehensive data and visualization for individual stocks.

### Features
1. **Stock Search**: Find stocks by code, name, or industry
2. **Price Charts**: View historical prices with customizable timeframes
3. **Technical Indicators**: Apply indicators like MA, MACD, RSI, Bollinger Bands
4. **Fundamental Data**: Analyze financial statements, ratios, and valuation metrics
5. **Industry Comparison**: Compare a stock against industry peers

### Performing Analysis
1. Enter a stock code in the search bar
2. Select the desired analysis type from the tabs
3. Configure parameters for technical indicators
4. Generate reports for comprehensive analysis

## Strategy Backtesting

Test trading strategies using historical data to evaluate their performance before deploying them.

### Available Strategies
1. **Moving Average Crossover**: Trade based on the crossing of two moving averages
2. **Mean Reversion**: Trade based on the principle that prices tend to revert to their mean
3. **Custom Strategies**: Implement your own strategies using the strategy framework

### Running a Backtest
1. Navigate to "Strategy Backtesting"
2. Select a strategy type
3. Configure strategy parameters
4. Select stock(s) and time period
5. Run the backtest
6. Analyze results including returns, drawdowns, and trade statistics

## Portfolio Monitoring

Track your portfolio performance and get insights for optimization.

### Key Features
1. **Holdings Overview**: View all positions with current market value and unrealized P&L
2. **Performance Analysis**: Track returns over time with benchmarking
3. **Risk Metrics**: Monitor portfolio risk with metrics like beta, volatility, and VaR
4. **Diversification Analysis**: Visualize allocation by sector, asset class, and geographic region

### Managing Your Portfolio
1. Add positions manually or import from CSV
2. Configure alerts for price movements or portfolio thresholds
3. View recommended portfolio adjustments based on risk management rules

## Trading Strategy Implementation

Implement and deploy trading strategies for automated or semi-automated trading.

### Strategy Development Process
1. Design strategy logic using the strategy framework
2. Backtest thoroughly with historical data
3. Paper trade to validate in current market conditions
4. Deploy with proper risk management parameters

### Strategy Types
1. **Technical**: Based on price patterns and indicators
2. **Fundamental**: Based on financial data and valuation metrics
3. **Statistical**: Based on statistical analysis and mean reversion
4. **Hybrid**: Combining multiple approaches for robust performance

## Risk Management

The system includes comprehensive risk management tools to protect your capital.

### Key Risk Management Features
1. **Position Sizing**: Automatically calculate position sizes based on risk parameters
2. **Stop Loss Management**: Implement and track stop loss orders
3. **Exposure Limits**: Set maximum exposure per stock, sector, or strategy
4. **Drawdown Protection**: Automatically reduce position sizes during drawdowns
5. **Correlation Analysis**: Identify and manage portfolio correlations

### Configuring Risk Management
1. Navigate to "Settings" → "Risk Management"
2. Set global risk parameters like maximum position size and portfolio exposure
3. Configure strategy-specific risk rules
4. Set up risk alerts and notifications

## Customization

The system offers extensive customization options to suit individual trading styles and preferences.

### Customization Options
1. **Custom Indicators**: Create and save custom technical indicators
2. **Strategy Parameters**: Customize existing strategies or create new ones
3. **UI Themes**: Choose between light and dark modes or customize colors
4. **Alert Settings**: Configure personalized alerts and notifications
5. **Reporting**: Customize reports and data exports

## Troubleshooting

### Common Issues and Solutions

1. **Connection Issues**
   - Ensure the backend server is running
   - Check database connection parameters in .env file
   - Verify network connectivity

2. **Data Discrepancies**
   - Refresh data using the "Sync Data" button
   - Check data source configuration
   - Verify the date range for historical data

3. **Performance Issues**
   - Reduce the amount of data loaded simultaneously
   - Close unnecessary browser tabs
   - Clear browser cache

### Getting Support
- Check the API documentation at http://localhost:8000/docs
- Review logs in the logs/ directory
- Visit the project repository for known issues and solutions 