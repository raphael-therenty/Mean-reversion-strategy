# Algorithmic Trading: Mean Reversion Strategy (US Tech & US Energy)


## Project Overview

This project implements and backtests a **Mean Reversion** trading strategy on a diverse portfolio of US Tech stocks (e.g., AAPL, NVDA) and US Energy stocks (e.g., XOM, CVX).

The hypothesis is that asset prices tend to oscillate around a historical average. When prices deviate significantly (Overbought/Oversold), they are likely to revert to the mean.

## Mathematical Concepts

### Theoretical Framework: The Ornstein-Uhlenbeck Process

Mathematically, Mean Reversion is often modeled using the **Ornstein-Uhlenbeck (OU)** stochastic process. Unlike a standard Random Walk (where prices drift endlessly), an OU process has a "tether" that pulls the price back to a central value.

The change in price $dx_t$ is defined as:

$$dx_t = \theta (\mu - x_t)dt + \sigma dW_t$$

Where:
* $x_t$: The current asset price.
* $\mu$: The long-term mean (equilibrium price).
* **$\theta$:** The **speed of mean reversion**. The larger this value, the faster the price snaps back to the mean.
* $(\mu - x_t)$: The "spread" or distance from the mean. The further the price is from the mean, the stronger the pull back.
* $\sigma dW_t$: Random market noise (volatility).

**In this strategy:**
* We approximate the mean $\mu$ using the **20-day Moving Average (SMA)**.
* We identify when the "spread" is statistically significant using **Bollinger Bands** ($2\sigma$ deviation).


The strategy relies on two key technical indicators to identify statistical outliers:

### 1. Relative Strength Index (RSI)
Measures the speed and change of price movements to identify overbought or oversold conditions.

$$RSI = 100 - \frac{100}{1 + RS}$$

*Where RS (Relative Strength) is the average of 'Up' closes divided by the average of 'Down' closes over 14 periods.*

### 2. Bollinger Bands (BB)
Measures market volatility and provides a relative definition of high and low prices.

$$\text{Upper Band} = \mu + k\sigma$$
$$\text{Lower Band} = \mu - k\sigma$$

*Where $\mu$ is the 20-day Simple Moving Average (SMA) and $\sigma$ is the standard deviation.*

---

## Strategy Logic

The algorithm executes trades based on the confluence of momentum (RSI) and volatility (Bollinger Bands):

* **Entry Signal (Buy):**
    1.  **RSI < 30**: Indicates the asset is oversold.
    2.  **Price < Lower Bollinger Band**: Confirms the price is statistically low (2 standard deviations below the mean).

* **Exit Signal (Sell):**
    * **RSI > 70**: The asset is becoming overbought.
    * *OR* **Price > SMA 20**: The price has successfully reverted to the mean.
    * *OR* **Time Stop**: Position held for > 15 days (capital preservation).

---

## Backtest Results

The strategy was backtested against an equal-weight **Buy & Hold** benchmark over the last year.

* **Initial Capital:** $200,000
* **Total Return:** +8.66%
* **Sharpe Ratio:** 0.52
* **Max Drawdown:** -13.08%


---

## Installation & Usage

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/mean-reversion-trading.git](https://github.com/YOUR_USERNAME/mean-reversion-trading.git)
    cd mean-reversion-trading
    ```

2.  **Install requirements:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the strategy:**
    Open `main_.ipynb` in Jupyter Notebook or VS Code to run the simulation.

