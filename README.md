# Volume Outlier Strategy

## Overview

This repository contains the implementation of the Volume Outlier Strategy, designed to identify and trade potential breakout stocks based on significant volume spikes and technical indicators. The strategy uses the Zerodha API to fetch historical and real-time data, executes buy/sell decisions, and maintains detailed trade logs and performance statistics.

## Features

- **Volume Outlier Detection**: Identifies stocks with volumes significantly higher than their recent averages.
- **Technical Indicators**: Uses Simple Moving Averages (SMA) and volume averages to confirm breakouts.
- **Automated Buy/Sell Mechanism**: Implements conditions to ensure optimal entry and exit points.
- **Trade Logging**: Maintains a detailed log of all trades for performance tracking and analysis.
- **Scheduler**: Automates the strategy to run during market hours for timely execution.

## Prerequisites

- Python 3.7 or higher
- Zerodha API access
- Required Python libraries: `pandas`, `numpy`, `schedule`, `datetime`, `csv`, `logging`

## Installation

1. **Clone the repository**:
    ```bash
    git clone https://github.com/yourusername/volume-outlier-strategy.git
    cd volume-outlier-strategy
    ```

2. **Install dependencies**:
    ```bash
    pip install pandas numpy schedule datetime
    ```

3. **Set up Zerodha API**:
    - Follow the [Zerodha API documentation](https://kite.trade/docs/connect/v3/) to obtain your API key and secret.
    - Update the API credentials in the code.

## Usage

### 1. Data Fetching

The strategy fetches historical and real-time data for all Nifty 500 stocks using the Zerodha API. It gathers daily and 15-minute interval data to identify volume breakouts and confirm breakout points.

### 2. Identifying Breakout Stocks

- **Volume Outlier Detection**:
  - A stock is considered for a breakout if its current volume is at least 5 times its 10-day exponential moving average volume (`AVG_Volume_10`).
  - The current volume should also be greater than or equal to its maximum volume over the past 132 days (`Max_Volume_132`).
  - The stock's closing price should be above its 66-day and 198-day Simple Moving Averages (`SMA_66` and `SMA_198`).

- **Selection of Top Breakout Stocks**:
  - From the filtered stocks, the top 5 stocks with the highest volumes are selected for potential trading.

### 3. Buy Mechanism

- **Initial Breakout Confirmation**:
  - After identifying a breakout stock, the strategy waits for the first 15-minute candle of the day.
  - If any subsequent 15-minute candle closes above the high of this first candle, it confirms the breakout.
  - The stock is bought at the close of this breakout candle.

- **Previous Breakout Days**:
  - If the breakout is identified based on the previous day's data, the strategy tracks the highest prices of the 15-minute candles on the following days.
  - If a subsequent 15-minute candle breaks these highs within two days of the initial breakout, the stock is bought at the close of this breakout candle.

- **Position Allocation**:
  - A fixed amount of capital is allocated per stock, calculated by dividing the total fund by the number of stocks in the portfolio.
  - The number of shares bought is determined by dividing this fixed amount by the breakout candle's closing price.

### 4. Sell Mechanism

- **Stop-Loss**:
  - A stop-loss is set at 8% below the buy price.
  - If the stock's price falls to or below this stop-loss level, the position is sold to limit the loss.

- **Target Achievement**:
  - The strategy sets a profit target based on a significant price increase.
  - If the stock's price reaches this target, the position is sold to lock in the profit.

- **Updating Trade Log**:
  - The strategy continuously monitors the stock's high and low prices after buying.
  - It updates the trade log with the highest and lowest values reached during the holding period.
  - It also logs the exit reason (stop-loss hit or target achieved) and calculates the return on investment (ROI) for the trade.

### 5. Trade Logging

- **Trade Log**:
  - Maintains a detailed log of each trade, including:
    - Entry and exit dates and times
    - Buy and sell prices
    - Number of shares
    - Buy and sell values
    - Profit/loss and ROI
    - High and low values during the holding period
    - Maximum drawdown
    - Exit reason (stop-loss or target)
    - Status (open or closed)
  - This log is saved in a CSV file and updated daily.

- **Summary Statistics**:
  - Tracks overall performance metrics, including:
    - Total profit/loss
    - Total return percentage
    - Compound Annual Growth Rate (CAGR)
    - Total number of trades
    - Winning trades and winning percentage
    - Maximum drawdown
  - These statistics are saved in a CSV file and updated daily.

### 6. Scheduler

- The strategy includes a scheduler that runs every minute during market hours (09:15 to 15:30).
- It checks if the current day is a trading day and performs the daily routine of fetching data, identifying breakout stocks, managing trades, and updating logs.

## Configuration

- Update the `config.py` file with your Zerodha API credentials and other relevant settings.

## Running the Strategy

1. **Start the Scheduler**:
    ```bash
    python main.py
    ```

2. **Monitor Logs**:
    - Check the `logs` directory for detailed logs of the strategy's execution.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License



---
