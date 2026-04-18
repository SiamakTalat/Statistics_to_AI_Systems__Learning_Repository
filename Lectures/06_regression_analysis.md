# Regression Analysis

Regression analysis is the statistical framework used to **model relationships between predictors (independent variables) and a response variable (dependent variable)**. It is essential for:

- Prediction and forecasting
- Effect estimation and sensitivity analysis
- Uncertainty analysis and confidence bands
- Feature importance identification
- Trend modeling and pattern discovery
- Scientific explanation and interpretability
- Surrogate modeling for optimization
- Causal-style interpretation (with caution)

Regression is one of the most important foundations of:

- Machine learning and AI
- Econometrics and time-series analysis
- Engineering simulation and FEM
- Scientific computing
- AI surrogate frameworks
- Uncertainty quantification

For AI, data science, and engineering workflows, this module is especially high value.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Construct and fit** linear regression models using least squares estimation for prediction and effect estimation
2. **Extend linear regression** to multiple predictors, polynomial, and logistic settings
3. **Diagnose regression assumptions** (linearity, independence, homoscedasticity, normality) and select appropriate models
4. **Apply regularization methods** (Ridge, Lasso, Elastic Net) to handle multicollinearity and feature selection
5. **Evaluate regression models** using appropriate metrics (RMSE, R², adjusted R², quantile loss)
6. **Interpret coefficients and confidence intervals** to quantify uncertainty and effect sizes

---

## 1. Foundations of Regression

The fundamental regression framework models the relationship between predictors and a response variable:

$$Y = f(X) + \varepsilon$$

where:
- **Y**: Response variable (what we want to predict)
- **X**: Predictor variable(s) (inputs or features)
- **f(·)**: The relationship function (what we need to learn)
- **ε**: Random error term (unexplained variability)

**Key Insight:** Regression assumes that the response Y can be decomposed into a systematic part f(X) that depends on predictors and a random part ε that doesn't. The goal is to estimate f(·) from data.

The exact form of f(·) depends on the type of regression model chosen. Different model classes make different assumptions about how Y depends on X.

**Check for Understanding:**
- If you observe Y values that don't perfectly follow your predicted values, which component (f(X) or ε) accounts for the difference?

---

## 2. Simple Linear Regression

Simple linear regression models the relationship between **one predictor and one continuous response** using a straight line.

### 2.1 Mathematical Model

$$y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$$

where:
- **β₀**: Intercept (value of Y when X = 0)
- **β₁**: Slope (change in Y per unit change in X)
- **ε_i**: Residual error for observation i (difference between actual and predicted)
- **i**: Observation index (i = 1, 2, ..., n)

### 2.2 Least Squares Estimation

The objective is to find β₀ and β₁ that minimize the sum of squared residuals:

$$\min_{\beta_0,\beta_1}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2 = \min_{\beta_0,\beta_1}\sum_{i=1}^{n}(y_i-\beta_0-\beta_1x_i)^2$$

**Why squared residuals?** Squaring penalizes large errors more than small ones and makes the math tractable.

### 2.3 Closed-Form Solutions

The least squares estimators have explicit formulas:

**Slope:**
$$\hat{\beta}_1 = \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{n}(x_i-\bar{x})^2}$$

**Intercept:**
$$\hat{\beta}_0 = \bar{y} - \hat{\beta}_1\bar{x}$$

**Interpretation:** β₁ represents the expected change in Y for a one-unit increase in X, holding all else constant.

### 2.4 Regression Assumptions

Linear regression assumes:

1. **Linearity:** The relationship between X and Y is linear (not curved or complex)
2. **Independence:** Observations are independent (no temporal clustering or spatial correlation)
3. **Homoscedasticity:** Error variance is constant across all X values (not "fanning out")
4. **Normal residuals:** Residuals (errors) are approximately normally distributed

**Why These Matter:** Violations of these assumptions can lead to biased estimates, incorrect confidence intervals, or poor predictions.

### 2.5 Use Cases

- Trend estimation (e.g., how does response time increase with load?)
- Sensitivity analysis (how much does output change per input unit?)
- Interpretable prediction (simple baseline models)
- FEM surrogate modeling for single-feature scenarios

**Example:** In engineering, you might model deflection (Y) as a linear function of applied load (X). The slope tells you the deflection per unit load.

**Check for Understanding:**
- If β₁ = 0.5 and β₀ = 10, what is the predicted Y when X = 20?

---

## 3. Multiple Linear Regression

Used when **multiple predictors influence the response**. This extends simple linear regression to many features.

### 3.1 Mathematical Model

$$y_i = \beta_0 + \beta_1x_{i1}+\beta_2x_{i2}+\cdots+\beta_px_{ip}+\varepsilon_i$$

where:
- **p**: Number of predictors
- **β_j**: Coefficient for predictor j (partial effect, holding others constant)
- **x_ij**: Value of predictor j for observation i

### 3.2 Matrix Form

For computational efficiency, write all observations as a matrix system:

$$\mathbf{y}=X\beta+\varepsilon$$

where:
- **y**: n×1 response vector
- **X**: n×(p+1) design matrix (includes column of 1s for intercept)
- **β**: (p+1)×1 coefficient vector
- **ε**: n×1 error vector

### 3.3 Least Squares Solution

The closed-form solution is:

$$\hat{\beta}=(X^TX)^{-1}X^Ty$$

**Computational note:** Modern software uses QR decomposition or other numerically stable methods rather than computing this directly.

### 3.4 Interpretation of Coefficients

**β_j** represents the **partial effect** of predictor j:

- **Expected change in Y** when predictor j increases by 1 unit
- **Holding all other predictors constant** (ceteris paribus)

**Important caveat:** If predictors are correlated (multicollinearity), interpretations become unstable. See Section 6 (Ridge Regression) for handling this.

### 3.5 Why Multiple Regression Is Important

Multiple regression is foundational for:
- Tabular machine learning baselines
- Surrogate modeling for expensive simulations (FEM, CFD)
- Feature screening and effect interpretation
- Engineering response surface modeling

**Example in Practice:** For structural response prediction, you might use multiple linear regression with predictors like material stiffness, cross-sectional area, load magnitude, and support type to predict displacement.

**Check for Understanding:**
- In a regression with 3 predictors, if predictor 1 has a coefficient of 2.5, does that mean it's more important than predictor 2 with coefficient 0.8? Why or why not?

---

## 4. Polynomial Regression

Used when the relationship is **nonlinear but can be approximated by polynomial terms**.

### 4.1 Mathematical Model

$$y = \beta_0+\beta_1x+\beta_2x^2+\cdots+\beta_dx^d+\varepsilon$$

where:
- **d**: Degree of polynomial
- **Higher degree terms** (x², x³, ...) capture curvature

### 4.2 Key Insight: Linear in Coefficients

Despite the nonlinear appearance, polynomial regression is still **linear in the coefficients β**. You can rewrite it by creating new features:

- Let x₁ = x, x₂ = x², x₃ = x³, ...
- Then: y = β₀ + β₁x₁ + β₂x₂ + β₃x₃ + ε

This means you can use the same least squares machinery as multiple linear regression.

### 4.3 Choosing Polynomial Degree

- **d = 1**: Linear fit
- **d = 2**: Quadratic (one bend)
- **d = 3**: Cubic (up to two bends)
- **Higher d**: More flexible but risk overfitting

**Guidance:** Start with low degree (d = 2 or 3) and increase only if data suggests curvature. Use cross-validation or information criteria (AIC, BIC) to compare.

### 4.4 Applications

- Curved trends (e.g., material strength vs. temperature often has quadratic form)
- Response surfaces (how output varies across input space)
- Smooth nonlinear approximation without domain-specific nonlinear models

**Example:** In materials science, fatigue life (Y) vs. stress (X) often follows an inverse polynomial relationship: log(Life) = β₀ - β₁·(Stress) - β₂·(Stress²).

---

## 5. Logistic Regression

Used when the response is **binary (yes/no, success/failure, pass/fail)** or represents a probability.

### 5.1 Binary Probability Model

$$P(Y=1|X)=\frac{1}{1+\exp[-(\beta_0+\beta^TX)]} = \frac{e^{\beta_0+\beta^TX}}{1+e^{\beta_0+\beta^TX}}$$

This is the **logistic (sigmoid) function**, which maps any input to probability [0, 1].

### 5.2 Logit Transformation

The log-odds form reveals the linear structure:

$$\log\left(\frac{p}{1-p}\right)=\beta_0+\beta^TX$$

where p = P(Y=1|X) and "log-odds" is the natural log of the odds ratio.

**Interpretation:** β_j represents the change in log-odds per unit increase in predictor j.

### 5.3 Estimation

Logistic regression uses maximum likelihood estimation (MLE), not least squares. The log-likelihood is:

$$\ell(\beta) = \sum_{i=1}^{n}[y_i\log(p_i)+(1-y_i)\log(1-p_i)]$$

**In practice:** Use scikit-learn, statsmodels, or similar libraries that implement MLE automatically.

### 5.4 Use Cases

- Classification baselines (disease presence, defect presence)
- Defect prediction (will component fail?)
- Anomaly detection (is this observation abnormal?)
- Pass/fail modeling (what's the probability of success?)

**Example:** Predicting whether a manufactured component will pass quality inspection based on material properties and manufacturing parameters.

**Check for Understanding:**
- If logistic regression predicts P(Y=1|X) = 0.75, what is the predicted probability that Y = 0?

---

## 6. Ridge Regression

Ridge regression addresses **multicollinearity** (correlation among predictors) by adding **L2 regularization**.

### 6.1 Motivation: Why Regularization?

When predictors are highly correlated:
- Ordinary least squares estimates become unstable (small data changes cause large coefficient swings)
- Coefficients may have wrong signs or unreasonable magnitudes
- Predictions may be poor despite good fit on training data

**Ridge regression shrinks coefficients** to stabilize them.

### 6.2 Objective Function

$$\min_\beta \sum_{i=1}^{n}(y_i-\hat{y}_i)^2 + \lambda\sum_{j=1}^{p}\beta_j^2$$

where:
- **First term**: Residual sum of squares (data fit)
- **Second term**: L2 penalty on coefficients (regularization)
- **λ (lambda)**: Regularization parameter controlling the trade-off

**Interpretation:** Ridge penalizes large coefficients, forcing them toward zero.

### 6.3 Closed Form

$$\hat{\beta}_{ridge}=(X^TX+\lambda I)^{-1}X^Ty$$

**Key difference:** Adding λI to X^TX makes the matrix invertible even when predictors are highly correlated.

### 6.4 Regularization Path

As λ increases:
- λ = 0: Ordinary least squares (no regularization)
- λ increases: Coefficients shrink toward zero
- λ → ∞: All coefficients → 0

**Cross-validation** is used to select the optimal λ.

### 6.5 When to Use Ridge

- Many correlated predictors (multicollinearity detected)
- More predictors than observations (p > n)
- Interpretation less critical than prediction accuracy
- Small sample size with many features

**Example:** For your 59-input FEM datasets with likely correlations, Ridge regression would stabilize estimates compared to ordinary least squares.

---

## 7. Lasso Regression

Lasso uses **L1 regularization** and has a unique property: it **performs automatic feature selection** by shrinking some coefficients exactly to zero.

### 7.1 Objective Function

$$\min_\beta \sum_{i=1}^{n}(y_i-\hat{y}_i)^2 + \lambda\sum_{j=1}^{p}|\beta_j|$$

The key difference from Ridge: **|β_j|** (absolute value) instead of **β_j²**.

### 7.2 Sparsity Property

**Lasso can shrink some coefficients exactly to zero**, effectively removing features:

- Features with β = 0 are excluded from the model
- Creates sparse models with only the most important features
- More interpretable for feature selection

**Ridge, by contrast, shrinks but never zeros out coefficients.**

### 7.3 Estimation

Lasso cannot be solved in closed form; use iterative algorithms like coordinate descent or LARS (Least Angle Regression).

**In practice:** Use scikit-learn's `Lasso` or `LassoCV` classes.

### 7.4 When to Use Lasso

- Automatic feature selection desired
- Many irrelevant predictors present
- Sparse, interpretable model needed
- Moderate multicollinearity

**Example:** With 59 input features, Lasso can automatically identify which 10-15 truly matter for prediction, discarding the rest.

**Check for Understanding:**
- If Lasso sets 40 out of 59 coefficients to zero, what's the practical benefit?

---

## 8. Elastic Net

Elastic Net combines **Ridge and Lasso penalties** to balance their strengths.

### 8.1 Objective Function

$$\min_\beta \sum_{i=1}^{n}(y_i-\hat{y}_i)^2 + \lambda_1\sum_{j=1}^{p}|\beta_j| + \lambda_2\sum_{j=1}^{p}\beta_j^2$$

Or equivalently, with mixing parameter α ∈ [0, 1]:

$$\min_\beta \sum_{i=1}^{n}(y_i-\hat{y}_i)^2 + \lambda[\alpha\sum_{j=1}^{p}|\beta_j| + (1-\alpha)\sum_{j=1}^{p}\beta_j^2]$$

### 8.2 When to Use Elastic Net

- **Many correlated predictors** (like Ridge, but with feature selection)
- **Sparse + stable solution desired** (combines Lasso and Ridge benefits)
- Grouped features (correlated features tend to be selected together)

**Why grouped features?** Lasso tends to arbitrarily pick one feature from a group of correlated features. Ridge keeps them all. Elastic Net balances this by selecting groups together.

**Example:** If three design parameters are highly correlated, Elastic Net is more likely to include all three or exclude all three, rather than picking one arbitrarily.

---

## 9. Poisson Regression

Used when the response variable is **count data** (non-negative integers).

### 9.1 Examples of Count Data

- Number of component failures
- Defect count in manufactured batch
- Crack count in structural inspection
- Number of events in time period

### 9.2 Model Structure

$$Y_i \sim \text{Poisson}(\lambda_i)$$

where:
- Y_i follows a Poisson distribution with parameter λ_i
- λ_i (lambda) is the expected count for observation i

### 9.3 Link Function

To relate predictors to the count:

$$\log(\lambda_i)=\beta_0+\beta^TX_i$$

**Why log link?** Because λ must be positive, the log link ensures this.

### 9.4 Mean-Variance Property

A key feature of Poisson distribution:

$$E(Y)=\text{Var}(Y)=\lambda$$

**Interpretation:** For count data, mean and variance are equal. If data violate this (overdispersion), negative binomial regression is better.

### 9.5 When to Use Poisson Regression

- Count data (0, 1, 2, 3, ...)
- Events occurring at relatively low rates
- Counts are independent across observations

**Example:** Predicting defect counts based on manufacturing parameters.

---

## 10. Quantile Regression

Quantile regression models **conditional quantiles** instead of only the mean, useful for understanding the full distribution of the response.

### 10.1 Motivation

- **Ordinary regression** estimates E[Y|X] (the mean/conditional expectation)
- **Quantile regression** estimates the τ-th quantile: Q_τ[Y|X]

For example:
- τ = 0.5: Median regression
- τ = 0.25: 25th percentile (lower quartile)
- τ = 0.75: 75th percentile (upper quartile)

### 10.2 Model

$$Q_{\tau}(Y|X)=\beta_0^\tau+\beta_1^\tau X_1+\cdots+\beta_p^\tau X_p$$

Note: Each quantile τ has its own coefficients **β^τ**.

### 10.3 Pinball Loss Function

Quantile regression uses asymmetric loss (pinball loss):

$$\rho_\tau(u)=u(\tau-\mathbf{1}_{u<0})$$

which can be written as:
- If u ≥ 0: Loss = τu
- If u < 0: Loss = (τ-1)u

**Intuition:** Errors above the predicted quantile are weighted by τ; errors below are weighted by (1-τ).

### 10.4 When to Use Quantile Regression

- Robust prediction (less sensitive to outliers than mean regression)
- Uncertainty bands (estimate confidence bounds on predictions)
- Percentile forecasting (predict 90th percentile for safety margins)
- Safety-critical applications (want upper percentile, not just mean)

**Example in Engineering:** For strength prediction, you want to know not just the average strength but the 5th percentile (minimum expected strength) for safety design.

**Check for Understanding:**
- If you fit quantile regression at τ = 0.9 and τ = 0.1, what do these models predict?

---

## 11. Nonlinear Regression

Used when the relationship **cannot be adequately modeled as linear in coefficients**, requiring domain-specific nonlinear functions.

### 11.1 General Form

$$y=f(x,\theta)+\varepsilon$$

where:
- **f(·)**: Nonlinear function (not a linear combination of coefficients)
- **θ**: Unknown parameters to estimate
- **ε**: Random error

### 11.2 Examples of Nonlinear Functions

- **Exponential growth:** f(x, θ) = θ₁ exp(θ₂x)
- **Saturation/Michaelis-Menten:** f(x, θ) = θ₁x / (θ₂ + x)
- **Power law:** f(x, θ) = θ₁ x^(θ₂)
- **Fatigue curves:** f(x, θ) = θ₁ x^(θ₂) + θ₃

### 11.3 Estimation

Usually solved by **nonlinear least squares**:

$$\min_\theta \sum_{i=1}^{n}(y_i-f(x_i,\theta))^2$$

**Challenges:**
- No closed-form solution
- Requires iterative algorithms (Gauss-Newton, Levenberg-Marquardt)
- Initial guess affects convergence
- Local minima possible

### 11.4 Applications

- Material fatigue curves (stress vs. cycles to failure)
- Growth models (exponential, logistic)
- Kinetic reaction models (chemistry, biology)
- Dose-response curves (pharmacology, toxicology)

**Example:** In materials, S-N curves (stress vs. number of cycles) follow a power law: log(S) = β₀ - β₁·log(N), which is nonlinear in the original variables.

---

## 12. Model Evaluation Metrics

Regression models are evaluated using different metrics depending on the context.

### 12.1 Mean Absolute Error (MAE)

$$\text{MAE}=\frac{1}{n}\sum_{i=1}^{n}|y_i-\hat{y}_i|$$

- **Interpretation:** Average magnitude of prediction errors in original units
- **Robust:** Less sensitive to outliers than RMSE
- **Use when:** Interpretability in original units important

### 12.2 Root Mean Square Error (RMSE)

$$\text{RMSE}=\sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}$$

- **Interpretation:** Typical magnitude of prediction errors
- **Penalizes large errors:** Quadratic penalty emphasizes outliers
- **Use when:** Large errors are particularly costly

### 12.3 Coefficient of Determination (R²)

$$R^2 = 1-\frac{\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}{\sum_{i=1}^{n}(y_i-\bar{y})^2} = 1 - \frac{SS_{res}}{SS_{tot}}$$

- **Interpretation:** Proportion of variance in Y explained by the model
- **Range:** 0 to 1 (higher is better)
- **Limitation:** Always increases when adding predictors; use adjusted R² instead

### 12.4 Adjusted R-Squared

$$R^2_{adj}=1-(1-R^2)\frac{n-1}{n-p-1}$$

- **Interpretation:** R² adjusted for number of predictors
- **Penalizes overfitting:** Decreases if new predictor doesn't improve model enough
- **Use when:** Comparing models with different numbers of predictors

**Comparison:** When adding a predictor, R² increases, but R²_adj may decrease if the improvement is minimal relative to the added complexity.

**Check for Understanding:**
- A model has R² = 0.92. Does this mean predictions are good? What else should you check?

---

## 13. Regression Diagnostics

After fitting a regression model, check whether assumptions are satisfied.

### 13.1 Residual Plots

Plot residuals (errors) vs. fitted values to check:
- **Linearity:** Residuals randomly scattered (no pattern)
- **Homoscedasticity:** Constant spread; no "funnel" or "megaphone" pattern
- **Outliers:** Identify extreme residuals that may violate assumptions

### 13.2 Q-Q Plot

Plot quantiles of residuals vs. normal distribution quantiles:
- **If approximately linear:** Residuals are normal
- **If curved:** Heavy tails or skewness suggests non-normality

### 13.3 Multicollinearity Check

Compute **variance inflation factor (VIF)**:

$$\text{VIF}_j = \frac{1}{1-R_j^2}$$

where R_j² is the R² from regressing predictor j on all other predictors.

- **VIF = 1:** No collinearity
- **VIF > 5-10:** Moderate to severe multicollinearity; consider Ridge, Lasso, or feature removal

---

## 14. Choosing the Right Regression Model

### Quick Decision Guide

- **Linear regression** → Single linear effect; simple baseline
- **Multiple regression** → Many predictors; tabular data
- **Polynomial** → Curved trends; response surfaces
- **Logistic** → Binary outcome; classification
- **Ridge** → Correlated predictors; multicollinearity
- **Lasso** → Feature selection; sparse model wanted
- **Elastic Net** → Grouped correlated features; balanced sparsity + stability
- **Poisson** → Count data; non-negative integer response
- **Quantile** → Robust prediction; percentile forecasting; safety margins
- **Nonlinear** → Physics-based nonlinear laws; domain-specific models

---

## 15. Practical Use in AI and Engineering

### 15.1 Highest-Value Regression Models

For modern AI and engineering workflows:

1. **Multiple linear regression:** Tabular data baseline; FEM surrogate modeling
2. **Ridge regression:** Stability with correlated features
3. **Lasso:** Feature selection and sparse models
4. **Elastic Net:** Balanced feature selection with stability
5. **Polynomial regression:** Curved relationships; response surfaces
6. **Quantile regression:** Robust prediction; uncertainty bands; safety margins
7. **Nonlinear surrogate fitting:** Domain-specific physical models

### 15.2 Applications

These directly support:
- Surrogate modeling for expensive simulations (FEM, CFD, optimization)
- Structural response prediction
- Feature selection and engineering interpretation
- Time-dependent forecasting
- Optimization metamodels
- Scientific effect estimation with proper uncertainty quantification

---

## Key Takeaways

1. **Linear regression** provides simple, interpretable models for continuous responses using least squares; verify assumptions before inference.

2. **Multiple regression** extends linear regression to many predictors; coefficients have partial effect interpretation (holding others constant).

3. **Polynomial regression** captures nonlinear curved relationships while remaining linear in coefficients; use low degrees to avoid overfitting.

4. **Logistic regression** models binary outcomes using the sigmoid function; coefficients represent changes in log-odds.

5. **Ridge and Lasso** address multicollinearity and overfitting; Ridge shrinks coefficients, Lasso performs feature selection (sparsity).

6. **Elastic Net** combines Ridge and Lasso; useful for grouped correlated features.

7. **Poisson regression** models count data with mean-variance equality; use when response is non-negative integer counts.

8. **Quantile regression** estimates conditional quantiles (median, percentiles) for robust prediction and uncertainty bands.

9. **Model evaluation** requires multiple metrics (RMSE, R², adjusted R², MAE) and diagnostic checks (residual plots, Q-Q plots, VIF).

---

## Connection to Next Modules

Regression concepts extend naturally to Lecture 7 (Multivariate Analysis), where you'll work with multiple response variables and latent structure discovery. The uncertainty quantification and interval estimation from regression form the foundation for Lecture 14 (Uncertainty Quantification), where prediction intervals and confidence bands become central. Finally, causal inference (Lecture 15) builds on regression by carefully controlling confounders to estimate causal effects.
