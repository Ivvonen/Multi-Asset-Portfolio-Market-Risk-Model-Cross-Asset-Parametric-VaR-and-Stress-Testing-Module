## Multi-Asset Portfolio Market Risk Model: 
## Cross-Asset Parametric Value at Risk (VaR) and Stress Testing Module

This a market risk model designed to quantify downside risk and perform macro-prudential stress testing for complex, cross-asset portfolios. This model integrates linear equity shares, non-linear derivatives (European Options), and sovereign fixed-income assets. By combining a **Delta-Gamma Taylor Series approximation** with a **Bond Duration-Convexity expansion**, the model maps risk factor dollar sensitivities into a unified **Cross-Asset Parametric Variance-Covariance (Cov-Var) Matrix** to evaluate diversified portfolio Value at Risk (VaR).

Additionally, the dedicated **Macro Stress Testing Module** subjects the mixed portfolio to absolute, non-linear structural shifts—overriding statistical historical correlations to evaluate portfolio survival horizons under catastrophic market shocks.

Live data pipelines feed the architecture dynamically, fetching historical underlying equities and real-time sovereign yield indexes while explicitly handling modern MultiIndex data structures.


## Core Analytical Features

*   **Multi-Asset Risk Unification**: Integrates distinct risk factor sensitivities—Equities, Option Greeks (Delta Δ, Gamma Γ), and Bond Cash Flows (Yield-to-Maturity, Modified Duration, Convexity)—into a single, consolidated risk framework.
*   **Non-Linear Curvature Tracking**: Utilizes a second-order Delta-Gamma Taylor Series expansion to capture option convexity, resolving the systemic underestimation of short-position tail risk common in linear models.
*   **Dynamic Fixed-Income Sensitivities**: Implements cash flow discounting via Newton-Raphson solvers to compute exact Modified Duration and Convexity price-responsiveness to yield curve parallel shifts.
*   **Diversified Parametric Covariance Matrix**: Automatically aggregates individual risk exposures against empirical asset correlations, isolating standalone asset risks to calculate exact portfolio diversification capital relief.
*   **Full Revaluation Stress Testing Module**: Performs absolute Black-Scholes repricing and quadratic duration expansions under severe macroeconomic shocks (e.g., simulating a simultaneous -30% spot crash, +25% absolute volatility surge, and a -150 bps flight-to-safety yield collapse) rather than relying on localized approximations.


## Mathematical Architecture & Core Modules

### 1. The Derivative Non-Linear Mapping (Delta-Gamma VaR Module)
To optimize calculation velocity across long historical arrays without the overhead of full revaluation, daily equity options PnL is approximated using a second-order Taylor expansion to capture price acceleration.

### 2. Fixed-Income Sensitivity Mapping (Duration-Convexity Module)
Sovereign bond allocations are mapped inversely to interest rate changes via a quadratic expansion tracking immediate percentage price modifications.

### 3. Cross-Asset Parametric Risk Aggregation
To account for diversification benefits across asset classes, exposures are translated into a standardized **Risk-Factor Dollar Sensitivity Vector**.

### 4. Deterministic Macro Stress Testing Module
During severe market shifts, historical correlation structures break down entirely. This module bypasses parametric linearizations, executing a **Full-Revaluation Framework** across all nodes simultaneously.
