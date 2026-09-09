# Multi-Asset Portfolio Market Risk Engine: Non-Linear Delta-Gamma VaR & Cross-Asset Parametric Stress Framework

An institutional-grade market risk engine designed to quantify downside risk and perform macro-prudential stress testing for complex, cross-asset portfolios. This engine integrates linear equity shares, non-linear derivatives (European Options), and sovereign fixed-income assets. By combining a **Delta-Gamma Taylor Series approximation** with a **Bond Duration-Convexity expansion**, the framework maps risk factor dollar sensitivities into a unified **Cross-Asset Parametric Variance-Covariance (Cov-Var) Matrix** to evaluate diversified portfolio Value at Risk (VaR).

Furthermore, the dedicated **Deterministic Macro Stress Testing Module** subjects the mixed book to absolute, non-linear structural shifts—overriding statistical historical correlations to evaluate portfolio survival horizons under catastrophic market dislocations.

Live data pipelines feed the architecture dynamically via the Yahoo Finance API, fetching historical underlying equities and real-time sovereign yield indexes while explicitly handling modern MultiIndex data structures.

---

## 🚀 Core Analytical Features

*   **Multi-Asset Risk Unification**: Integrates distinct risk factor sensitivities—Equities, Option Greeks (Delta Δ, Gamma Γ), and Bond Cash Flows (Yield-to-Maturity, Modified Duration, Convexity)—into a single, consolidated risk framework.
*   **Non-Linear Curvature Tracking**: Utilizes a second-order Delta-Gamma Taylor Series expansion to capture option convexity, resolving the systemic underestimation of short-position tail risk common in linear models.
*   **Dynamic Fixed-Income Sensitivities**: Implements cash flow discounting via Newton-Raphson solvers to compute exact Modified Duration and Convexity price-responsiveness to yield curve parallel shifts.
*   **Diversified Parametric Covariance Matrix**: Automatically aggregates individual risk exposures against empirical asset correlations, isolating standalone asset risks to calculate exact portfolio diversification capital relief.
*   **Full Revaluation Stress Testing Module**: Performs absolute Black-Scholes repricing and quadratic duration expansions under severe macroeconomic crisis profiles (e.g., simulating a simultaneous -30% spot crash, +25% absolute volatility surge, and a -150 bps flight-to-safety yield collapse) rather than relying on localized approximations.
*   **Production Deployment UI/UX**: Bound entirely within a high-performance Streamlit visual dashboard layer utilizing dynamic text inputs, sliders, and Plotly interactive data distributions.

---

## 📐 Mathematical Architecture & Core Modules

### 1. The Derivative Non-Linear Mapping (Delta-Gamma VaR Module)
To optimize calculation velocity across long historical arrays without the overhead of full revaluation, daily equity options PnL is approximated using a second-order Taylor expansion to capture price acceleration (Γ):

\[\Delta \Pi_{\text{Option}} \approx Q_{\text{contracts}} \cdot 100 \cdot \left[ \Delta \cdot (S_0 R_{\text{stock}}) + \frac{1}{2}\Gamma \cdot (S_0 R_{\text{stock}})^2 \right]\]

### 2. Fixed-Income Sensitivity Mapping (Duration-Convexity Module)
Sovereign bond allocations are mapped inversely to interest rate changes (Δ y) via a quadratic expansion tracking immediate percentage price modifications:

\[\frac{\Delta B}{B} \approx -D_{\text{mod}} \cdot \Delta y + \frac{1}{2} C \cdot (\Delta y)^2\]

### 3. Cross-Asset Parametric Risk Aggregation
To account for diversification benefits across asset classes, exposures are translated into a standardized **Risk-Factor Dollar Sensitivity Vector (\(\mathbf{w}\))**:

\[\mathbf{w} = \begin{bmatrix} (q_{\text{stock}} \cdot S_0) + (Q_{\text{contracts}} \cdot 100 \cdot S_0 \cdot \Delta) \\ B_0 \cdot D_{\text{mod}} \end{bmatrix}\]

Given the empirical historical cross-asset covariance matrix (\(\mathbf{\Sigma}\)), the diversified portfolio return volatility (\(\sigma_{\text{port}}\)) and Parametric Value at Risk (\(\text{VaR}_{\alpha}\)) are modeled as:

\[\sigma_{\text{portfolio}} = \sqrt{\mathbf{w}^T \mathbf{\Sigma} \mathbf{w}}\]

\[\text{VaR}_{\alpha} = Z_{\alpha} \cdot \sigma_{\text{portfolio}} \quad \text{where } Z_{0.99} = 2.33 \text{ for 99\% confidence}\]

### 4. Deterministic Macro Stress Testing Module
During severe market dislocations (Δ S > 20%), historical correlation structures break down entirely. This module bypasses parametric linearizations, executing an exact **Full-Revaluation Framework** across all nodes simultaneously:

\[\text{PnL}_{\text{Stressed, Total}} = \Delta \Pi_{\text{Equity, Stressed}} + \Delta \Pi_{\text{Derivative, Stressed (BS Full Reval)}} + \Delta \Pi_{\text{Bond, Stressed (Convexity Match)}}\]

Where:
\[\Delta \Pi_{\text{Derivative, Stressed}} = Q \cdot 100 \cdot \left[ \text{BS}(S_{\text{shocked}}, \sigma_{\text{shocked}}) - \text{BS}(S_0, \sigma_0) \right]\]

---

## 🛠️ Tech Stack & Dependencies

*   **Language**: Python 3.10+
*   **Quantitative Analytics**: NumPy, SciPy (Optimization & Statistical distributions), Pandas
*   **Data Pipelines**: yfinance API (With built-in MultiIndex column flattening safeguards)
*   **Visualization & UI**: Streamlit Cloud, Plotly Interactive Analytics

---

## 🚀 Local Installation & Execution

1. Clone this repository to your workstation:
   ```bash
   git clone https://github.com
   cd multi-asset-risk-matrix
   ```

2. Establish an isolated virtual environment and install the required dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   pip install streamlit yfinance pandas numpy scipy plotly
   ```

3. Ensure a `requirements.txt` file exists in the root directory for cloud compilation:
   ```text
   streamlit
   yfinance
   pandas
   numpy
   scipy
   plotly
   ```

4. Launch the local interactive web dashboard:
   ```bash
   streamlit run app.py
   ```

---

## 💡 Quantitative Interview Defense: Key Nuances Explained

When defending this project to a Risk Director or Quantitative Panel, highlight these production-grade architectural choices:

*   **The Delta-Equivalent Linearization Caveat**: Acknowledge that while parametric variance-covariance aggregation scales exceptionally well across cross-asset books, it treats the options position as a linear "Delta-equivalent" during the covariance matrix step. Point out that your engine counters this by utilizing **Historical Delta-Gamma Simulation** to plot the standalone equity return distribution chart, preserving the non-normal skewness parameters.
*   **Why Switch to Full Revaluation for Stress Testing?** Taylor Series approximations are localized and degrade rapidly during severe structural shifts. For the macro stress-testing suite, the engine completely abandons approximations and performs exact Black-Scholes full repricing to ensure mathematical truth under extreme dislocations (e.g., a -30% underlying shock).
*   **The Yield Curve Parallel Shift Limitation**: Note that the fixed-income module assumes a parallel shift across the yield curve (short and long rates moving equally). State that an enterprise upgrade would incorporate **Key Rate Durations** to break the yield curve into distinct nodes (2Y, 5Y, 10Y, 30Y) to account for curve steepening or flattening twists.
