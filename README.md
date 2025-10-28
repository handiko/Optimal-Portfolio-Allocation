# 🚀 Multi-Criteria Portfolio Optimizer: Monte Carlo Allocation Engine

This project provides a robust Python solution for determining **optimal portfolio allocations** based on a set of user-defined stock tickers and historical data. Utilizing the **Monte Carlo Simulation** technique, this tool identifies portfolios that excel across several key risk-adjusted metrics, moving beyond just the traditional Sharpe Ratio to offer a more nuanced view of capital efficiency and risk management.

---

## ⚙️ Core Technical Architecture and Methodology

The system is built around the financial engineering principle of the **Efficient Frontier**, computationally searching the space of all possible weight combinations to find the highest return for a given level of risk (volatility).

### 1. Data Ingestion and Financial Preprocessing
The application leverages the `yfinance` library for seamless acquisition of historical stock price data.

* **Data Source**: Fetches **adjusted closing prices** for the specified list of `TICKERS`.
* **Transformation**: Calculates **daily percentage returns** (`data.pct_change()`).
* **Annualization**: Establishes the **annualization factor** (default: 252 trading days) to calculate the **Annualized Mean Returns Vector** and the **Annualized Covariance Matrix ($\Sigma$)**. This matrix is critical as it captures the interplay (correlation/co-variance) between the assets, forming the backbone of portfolio volatility calculation: $\sigma_p = \sqrt{\mathbf{w}^T \mathbf{\Sigma} \mathbf{w}}$.

### 2. Monte Carlo Simulation

The core optimizer runs a high-volume **Monte Carlo Simulation** (default: 50,000 iterations). In each run, a set of **random weights ($\mathbf{w}$)** is generated and normalized ($\sum w_i = 1$). For every simulated portfolio, five key metrics are computed:

* **Annualized Return ($\mu_p$)**
* **Annualized Volatility ($\sigma_p$)**
* **Sharpe Ratio**
* **Sortino Ratio**
* **Calmar Ratio**

### 3. Advanced Risk-Adjusted Metrics

This optimizer provides enhanced insights by calculating metrics that specifically target *downside* or *drawdown* risk, offering a superior measure of portfolio quality compared to metrics relying solely on total volatility ($\sigma_p$):

| Metric | Calculation Focus | Technical Implementation |
| :--- | :--- | :--- |
| **Sharpe Ratio** | Return per unit of **Total Volatility**. | $(\mu_p - R_f) / \sigma_p$ |
| **Sortino Ratio** | Return per unit of **Downside Deviation**. | $(\mu_p - R_f) / \text{Downside Deviation}$ |
| **Calmar Ratio** | Return relative to the **Maximum Drawdown**. | $\mu_p / \text{Max Drawdown}$ |

The implementation of **Max Drawdown** involves calculating the cumulative returns and tracking the difference between the running *peak* and the *trough* that follows. **Downside Deviation** is calculated as the volatility of only the returns that fall below a Minimum Acceptable Return (MAR).

---

## ✨ Identifying the Optimal Portfolios

The simulation results are collated into a `pandas.DataFrame`, enabling efficient identification of four critical benchmark portfolios:

1.  **Max Sharpe Ratio Portfolio**: The ideal point on the classic Efficient Frontier.
2.  **Min Volatility Portfolio**: The **Global Minimum Variance Portfolio (GMVP)**, which carries the lowest possible risk for the selected assets.
3.  **Max Sortino Ratio Portfolio**: The most effective portfolio in mitigating **downside risk**.
4.  **Max Calmar Ratio Portfolio**: The most effective portfolio for **capital preservation** against significant historical losses.

## 🖼️ Visualization: The Efficient Frontier

The results are beautifully visualized using `matplotlib`. The plot maps the entire simulated universe in the **Return vs. Volatility** space. Portfolios are color-coded by their **Sharpe Ratio** for instant insight into efficiency, and the four optimal portfolios are highlighted with distinct markers to pinpoint the best allocations according to each specific risk metric.

---

## 🛠️ Getting Started

### Prerequisites

```bash
pip install yfinance pandas numpy matplotlib
