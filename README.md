## Project Overview

This Jupyter notebook contains the code my team developed for the 2024 Alpha Trading Competition, hosted by the Quant Team at the Smith Investment Fund, a student-run investment group at the University of Maryland. Our team placed first among five competing teams. We focused on two strategies: **Residual Momentum** and **Internal Bar Strength**. Both strategies were backtested using SIF's internal infrastructure, achieving cumulative returns of approximately 80% and 100%, respectively, over the period from January 1, 2010, to December 31, 2015.

## The Backtester

The backtester is Python class built using SIF's internal infrastructure. Its purpose is to pull stock data from the SIF database and store it in memory for backtesting. The key parameters are:

- `start_date`, `end_date`: The start and end dates of the backtesting period.
- `universe_rebalance_dates`: The dates when the portfolio is rebalanced. In this case, the portfolio is rebalanced monthly.
- `max_lookback`: The number of days to look back at each rebalance date. In other words, at each rebalance date, a sliding window of size `max_lookback` is used to construct trading metrics and signals.
- `universe_size`: The number of stocks in the portfolio.
- `factors`: The attributes used to construct trading signals. In our strategies, these include close prices, high and low prices, market capitalizations, and price-to-book ratios.

## The Strategies

An alpha strategy is a Python class that shares a subset of attributes — `max_lookback`, `universe_size`, and `factors`— with the backtester. Its key method, `generate_day`, returns a vector representing the portfolio weights of each stock on the rebalance date. We briefly described the strategies we used. 

### Residual Momentum

According to the Fama-French three-factor model, a stock's return at any given time can be explained by its exposure to the market portfolio, the Small Minus Big (SMB) portfolio, and the High Minus Low (HML) portfolio. The portion of returns not explained by these factors is calld the the stock's **alpha**. The residual momentum strategy seeks to select stocks with consistent positive alphas. The procedure is as follows:  

At each rebalance date, we look at the stock data in the past 36 months and construct the Fama-French factor portfolios on a daily basis. We then run a linear regression of each stock’s returns against the factor returns to estimate the alphas. The last 12 months of this 36-month window are called the **formation period**, where we compute **risk-adjusted residual returns** (defined as the mean of the alphas divided by their standard deviation). Finally, we rank the stocks based on this metric and take long positions in the top decile. 




