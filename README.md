## Multi-Asset Portfolio Market Risk Model: Cross-Asset Parametric Value at Risk (VaR) and Stress Testing Module

This market risk model is designed to quantify downside risk and perform macro-prudential stress testing for complex, cross-asset portfolios. It integrates linear equity shares, non-linear derivatives (European Options), and sovereign fixed-income assets. 
By combining a **Delta-Gamma Taylor Series approximation** with a **Bond Duration-Convexity expansion**, the model maps risk factor sensitivities into a unified **Cross-Asset Parametric Variance-Covariance (Cov-Var) Matrix** to evaluate diversified portfolio Value at Risk (VaR).

Additionally, the **Macro Stress Testing Module** subjects the mixed portfolio to absolute, non-linear structural shocks - overriding statistical historical correlations to evaluate portfolio survival horizons under catastrophic market shifts.
