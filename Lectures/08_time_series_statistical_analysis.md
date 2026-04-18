# Time Series Statistical Analysis

Time series statistical analysis focuses on **data collected sequentially over time**, where the temporal order carries critical information about dependencies, patterns, and future behavior.

Unlike standard regression or correlation (which treat observations as independent), time series methods explicitly model:

- Temporal dependence (autocorrelation)
- Lag effects (current value depends on past values)
- Persistence (trends lasting over time)
- Seasonality (repeating patterns)
- Trends (long-term direction)
- Shocks (sudden changes)
- Cyclical behavior (medium-term patterns)
- Forecasting uncertainty and confidence bounds

This module is fundamental for:
- Forecasting and prediction
- Sensor stream analytics
- Optimization convergence history analysis
- Financial modeling and analysis
- SHM sensor streams for structural health
- Demand prediction and planning
- Training-loss curve analysis in machine learning
- Environmental and weather monitoring

For AI, engineering, and sequential simulation workflows, this is one of the most practical advanced statistics modules.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Decompose time series** into trend, seasonal, and residual components
2. **Apply smoothing techniques** (moving average, exponential smoothing) for denoising and forecasting
3. **Fit autoregressive and ARIMA models** to capture temporal dependencies
4. **Test for stationarity** and determine appropriate differencing
5. **Forecast future values** using fitted time-series models with uncertainty bounds
6. **Interpret ACF and PACF plots** for model order selection
7. **Handle seasonal patterns** using SARIMA and seasonal decomposition

---

## 1. Foundations of Time Series

### 1.1 Time Series Notation and Structure

A time series is a sequence of observations ordered by time:

$$\{x_t\}_{t=1}^{T}$$

where:
- **t**: Time index (t = 1, 2, ..., T)
- **x_t**: Observed value at time t
- **T**: Number of observations

**Key characteristic:** Order matters. Shuffling observations destroys the structure.

### 1.2 Time Series Components

Most time series can be decomposed into **four main components**:

1. **Trend (T_t):** Long-term increasing or decreasing direction
2. **Seasonality (S_t):** Repeating patterns at fixed intervals (daily, weekly, yearly)
3. **Cyclical (C_t):** Medium-term oscillations with non-fixed frequency
4. **Irregular/Noise (R_t):** Random fluctuations

### 1.3 Decomposition Models

**Additive Model:**
$$x_t = T_t + S_t + C_t + R_t$$

Use when:
- Seasonal variation is roughly constant over time
- Components are independent

**Multiplicative Model:**
$$x_t = T_t \times S_t \times C_t \times R_t$$

Use when:
- Seasonal variation increases or decreases with trend
- Components interact

**Example:** Daily temperature often additive (seasonal pattern consistent). Quarterly sales often multiplicative (seasonal swings larger in growth periods).

**Check for Understanding:**
- If a time series has trend 20, seasonal component 5, and noise 2, what are the expected values using additive decomposition?

---

## 2. Trend Analysis

Trend analysis captures the **long-term direction** of the sequence, separating it from short-term fluctuations.

### 2.1 Linear Trend Model

The simplest trend is linear:

$$x_t = \beta_0 + \beta_1 t + \epsilon_t$$

where:
- **β₀**: Intercept (baseline level at t = 0)
- **β₁**: Slope (change per time unit)
- **ε_t**: Residual noise
- **t**: Time index

**Interpretation:**
- **β₁ > 0**: Increasing trend
- **β₁ < 0**: Decreasing trend
- **β₁ ≈ 0**: Flat trend (no systematic direction)

### 2.2 Polynomial Trend

For curved trends, use polynomial terms:

$$x_t = \beta_0 + \beta_1 t + \beta_2 t^2 + \epsilon_t$$

- Quadratic trends curve once (e.g., growth then plateau)
- Higher degrees capture more complex curves

### 2.3 Piecewise Linear Trends

When the trend changes direction at known breakpoints:

$$x_t = \begin{cases}
\beta_0 + \beta_1 t + \epsilon_t & \text{if } t \leq t_0 \\
\beta_0' + \beta_1' t + \epsilon_t & \text{if } t > t_0
\end{cases}$$

### 2.4 Applications

- **Structural degradation:** How does material strength decline over service life?
- **Optimization convergence:** How does objective function improve over iterations?
- **Model drift:** How does model performance degrade over time in production?
- **Demand growth:** How do sales grow over years?

**Example for Your Work:** For optimization algorithm analysis, trend analysis quantifies convergence rate: if slope β₁ = -0.5, the objective improves by 0.5 units per iteration.

---

## 3. Seasonal Decomposition

Seasonality represents **repeating periodic patterns** that occur at regular intervals.

### 3.1 Types of Seasonality

- **Hourly:** Patterns within a day
- **Daily:** Patterns within a week
- **Weekly:** Patterns within a month
- **Monthly:** Patterns within a year
- **Yearly:** Annual cycles

### 3.2 Additive Seasonal Decomposition

$$x_t = T_t + S_t + R_t$$

Each component is isolated:
1. **Estimate trend** T_t (e.g., using moving average)
2. **Detrend:** Subtract trend to get x_t - T_t
3. **Extract seasonal:** Average detrended values by season
4. **Residual:** Remainder after removing trend and seasonality

### 3.3 Multiplicative Seasonal Decomposition

$$x_t = T_t \cdot S_t \cdot R_t$$

Use when seasonal variation scales with trend level. Estimation:
1. Estimate trend T_t
2. Compute ratio: x_t / T_t
3. Extract seasonal pattern from ratios
4. Isolate residual

### 3.4 Why Seasonal Decomposition Matters

Understanding seasonality is critical for:
- **Demand cycles:** Store sales peak during holidays; forecasting must account for this
- **Traffic patterns:** Rush hour peaks; network must be provisioned accordingly
- **Weather signals:** Temperature varies daily and yearly; HVAC systems must respond
- **Building energy loads:** Peak demand hours differ by season
- **SHM environmental cycles:** Temperature and humidity affect structural measurements

**Check for Understanding:**
- A building's daily electricity consumption shows a peak at 7-9 AM and again at 6-8 PM. Is this seasonality, a trend, or noise?

---

## 4. Moving Average

Moving average **smooths short-term fluctuations**, revealing underlying patterns.

### 4.1 Simple Moving Average (SMA)

With window size k:

$$\text{SMA}_t = \frac{1}{k}\sum_{i=0}^{k-1} x_{t-i} = \frac{x_t + x_{t-1} + \cdots + x_{t-k+1}}{k}$$

**Effect:**
- **Larger k:** Smoother curve; more lag; loses fine detail
- **Smaller k:** Retains more detail; noisier; less lag

### 4.2 Centered Moving Average

For symmetry (no lag), use center window:

$$\text{CMA}_t = \frac{1}{k}\sum_{i=-\lfloor k/2 \rfloor}^{\lfloor k/2 \rfloor} x_{t+i}$$

Better for visualization but cannot forecast (uses future values).

### 4.3 Exponential Weighting

Give higher weight to recent observations:

$$\text{EMA}_t = \alpha x_t + (1-\alpha) \text{EMA}_{t-1}$$

where α ∈ (0, 1):
- **Larger α:** More responsive to recent data
- **Smaller α:** Smoother; more inertia

### 4.4 Applications

- **Denoising:** Remove high-frequency noise from sensor streams
- **Trend extraction:** Estimate underlying trend by smoothing
- **Convergence smoothing:** Smooth noisy optimization loss curves
- **Sensor data:** Moving average often standard preprocessing for IoT data

**Example:** For SHM accelerometer data with high noise, a 10-sample moving average reveals true structural vibration modes.

---

## 5. Exponential Smoothing

Exponential smoothing gives **more weight to recent observations**, making it responsive to new data while maintaining smoothness.

### 5.1 Simple Exponential Smoothing

$$\hat{x}_{t+1}=\alpha x_t + (1-\alpha)\hat{x}_t$$

where:
- **α**: Smoothing parameter (0 < α < 1)
- **x_t**: Current observation
- **ĥ_{t}**: Smoothed estimate
- **ĥ_{t+1}**: One-step-ahead forecast

**Interpretation:** Forecast is weighted average of current observation (weight α) and previous estimate (weight 1-α).

### 5.2 Holt's Trend Smoothing

Adds trend component:

$$\ell_t = \alpha x_t + (1-\alpha)(\ell_{t-1}+b_{t-1})$$
$$b_t = \beta(\ell_t-\ell_{t-1})+(1-\beta)b_{t-1}$$

**Forecast:** $\hat{x}_{t+h} = \ell_t + h \cdot b_t$

- **ℓ_t**: Level (current smoothed value)
- **b_t**: Trend (slope)
- **h**: Steps ahead to forecast

### 5.3 Holt-Winters Seasonal Smoothing

Adds seasonality to Holt's method (multiplicative or additive).

**Good for:** Daily peaks, yearly cycles, etc.

### 5.4 When to Use Exponential Smoothing

- Short-term forecasting (next few time periods)
- Requires simple, fast implementation
- Nonstationary data with trend/seasonality
- Real-time systems (new observation updates forecast immediately)

---

## 6. Autoregressive Models (AR)

Autoregressive models predict the present from **past values of the same variable**.

### 6.1 AR(p) Model

$$x_t = c + \sum_{i=1}^{p}\phi_i x_{t-i}+\epsilon_t$$

where:
- **c**: Constant
- **p**: Number of lags
- **φ_i**: Autoregressive coefficients
- **ε_t**: White noise error (zero mean, constant variance)

**Interpretation:** Current value depends on p previous values.

### 6.2 AR(1) Model

The simplest case:

$$x_t = c + \phi x_{t-1}+\epsilon_t$$

- **φ > 0**: Positive persistence (high values followed by high values)
- **φ < 0**: Negative persistence; oscillation
- **|φ| close to 1**: Strong memory; slow decay
- **|φ| close to 0**: Weak memory; quickly forgets past

### 6.3 Stationarity Condition

For AR(p) to be stationary, roots of the characteristic equation must lie outside the unit circle.

**For AR(1):** Requires |φ| < 1.

### 6.4 Applications

- **SHM streams:** Acceleration at time t depends on previous accelerations
- **Optimizer progress:** Improvement at iteration t relates to previous improvements
- **Load prediction:** Today's power demand relates to yesterday's
- **Asset prices:** Market persistence effects

**Example:** For optimization convergence, AR(1) model with φ = 0.8 suggests strong persistence: large improvements tend to follow improvements.

**Check for Understanding:**
- If an AR(1) model has φ = -0.5, what does this mean for the time series behavior?

---

## 7. ARIMA Models

**ARIMA** (AutoRegressive Integrated Moving Average) is one of the most important classical forecasting models. It combines three components to handle trend and autocorrelation.

### 7.1 Components

- **AR (p):** Autoregressive part; current value depends on p past values
- **I (d):** Integrated (differencing); differencing d times to induce stationarity
- **MA (q):** Moving average part; current value depends on q past errors

### 7.2 ARIMA(p,d,q) Model

After differencing d times:

$$\phi(B)(1-B)^d x_t = \theta(B)\epsilon_t$$

where:
- **p**: AR order
- **d**: Differencing order
- **q**: MA order
- **B**: Backshift operator (Bx_t = x_{t-1})

### 7.3 Differencing for Stationarity

If a time series has a trend, take first differences:

$$\Delta x_t = x_t - x_{t-1}$$

Second-order differences:

$$\Delta^2 x_t = \Delta x_t - \Delta x_{t-1}$$

Differencing removes trends and induces stationarity.

### 7.4 Examples

- **ARIMA(1,0,0):** AR(1) model (stationary series)
- **ARIMA(1,1,0):** AR(1) on differenced series (one unit root; one trend)
- **ARIMA(0,1,1):** Exponential smoothing (simple trend+MA)
- **ARIMA(1,1,1):** Classic Holt's method

### 7.5 Why ARIMA Is Important

- **Unified framework:** Handles many types of series
- **Trend handling:** Differencing removes trend
- **Lag dependencies:** AR and MA capture autocorrelation
- **Forecasting:** Generates confidence bounds for predictions
- **Baseline benchmark:** Often outperforms simpler methods

**Example:** For convergence trajectory, ARIMA(1,1,1) might fit well: differences capture acceleration, AR captures persistence, MA captures shock effects.

---

## 8. SARIMA Models

**SARIMA** extends ARIMA with **seasonal components**.

### 8.1 Model Notation

$$\text{SARIMA}(p,d,q) \times (P,D,Q)_s$$

where:
- **(p,d,q):** Non-seasonal AR, differencing, MA orders
- **(P,D,Q):** Seasonal AR, differencing, MA orders
- **s:** Seasonal period (e.g., 12 for monthly data with yearly seasonality)

### 8.2 Full Form

$$\Phi(B^s)\phi(B)(1-B)^d(1-B^s)^D x_t = \Theta(B^s)\theta(B)\epsilon_t$$

Non-seasonal and seasonal operators act together, multiplicatively.

### 8.3 Applications

- **Electrical demand:** Monthly/yearly cycles in energy consumption
- **Retail sales:** Holiday/seasonal peaks
- **Climate data:** Yearly temperature cycles
- **Building occupancy:** Daily and weekly patterns

**Example:** SARIMA(1,1,1)(0,1,1)₁₂ for monthly data with yearly seasonality: first differencing (trend), seasonal differencing (yearly pattern), AR and MA for dynamics.

---

## 9. Stationarity Tests

Most classical time-series models **assume stationarity**: the statistical properties don't change over time.

### 9.1 What Is Stationarity?

A **weakly stationary** process has:
- **Constant mean:** E[x_t] = μ (doesn't change over time)
- **Constant variance:** Var(x_t) = σ² (doesn't change over time)
- **Lag-only covariance:** Cov(x_t, x_{t-k}) = γ_k (depends only on lag k, not on t)

**Non-stationary examples:**
- Series with trend (mean changes)
- Series with increasing variance (heteroscedasticity over time)

### 9.2 Augmented Dickey-Fuller (ADF) Test

Tests the hypothesis:

$$H_0: \text{Unit root exists (non-stationary)}$$
$$H_1: \text{Series is stationary}$$

**Test regression:**

$$\Delta x_t = \alpha + \beta t + \gamma x_{t-1}+\sum_{i=1}^{p} \delta_i \Delta x_{t-i}+\epsilon_t$$

Tests whether **γ = 0** (unit root, non-stationary) or **γ < 0** (stationary).

**Interpretation:**
- **p-value < 0.05:** Reject H₀; series is stationary
- **p-value ≥ 0.05:** Fail to reject H₀; series likely non-stationary

### 9.3 KPSS Test

An alternative test with opposite null hypothesis:

$$H_0: \text{Series is stationary}$$
$$H_1: \text{Unit root (non-stationary)}$$

Use both ADF and KPSS for confirmation.

### 9.4 Implications for Modeling

- **Stationary series:** Use ARMA, AR, ARIMA with d=0
- **Non-stationary series:** Difference (d=1 or d=2) to induce stationarity, then fit ARIMA

**Example:** Stock price typically non-stationary (ADF p-value > 0.05). Stock returns (first differences) typically stationary.

**Check for Understanding:**
- If ADF test gives p-value = 0.08, should you difference the series before fitting ARIMA?

---

## 10. ACF and PACF for Model Selection

**Autocorrelation function (ACF)** and **partial autocorrelation function (PACF)** plots guide model order selection.

### 10.1 ACF Plot

ACF(k) measures correlation between x_t and x_{t-k}.

**Interpretation for order selection:**
- **MA(q) signature:** ACF cuts off sharply after lag q; PACF tails off
- **AR(p) signature:** ACF tails off; PACF cuts off sharply after lag p
- **ARMA signature:** Both tail off gradually

### 10.2 PACF Plot

PACF(k) measures correlation of x_t with x_{t-k} after removing intermediate lags.

**More useful for AR order identification:**
- **AR(p):** PACF ≠ 0 for lags 1 to p; PACF ≈ 0 for lags > p

### 10.3 Practical Workflow

1. Plot ACF and PACF
2. If series is non-stationary (ACF doesn't decay), difference it
3. Look for cutting-off points in ACF (suggests MA) and PACF (suggests AR)
4. Estimate p, d, q and fit ARIMA
5. Validate residuals are white noise (ACF of residuals should show no pattern)

---

## 11. Practical Guide: Choosing Time-Series Methods

| Goal | Method | Use When |
|------|--------|----------|
| Identify long-term direction | Trend analysis | Trend is clear; want to quantify slope |
| Extract periodic patterns | Seasonal decomposition | Seasonality evident; want to isolate it |
| Smooth noisy data | Moving average | Need simple denoising; don't need forecast |
| Forecast with trend/seasonality | Exponential smoothing | Need quick, interpretable forecasting |
| Model lag dependence | AR models | Current value depends on past values |
| Unified trend + lag forecasting | ARIMA | Trend + autoregressive dynamics |
| Handle seasonal patterns | SARIMA | Trend + lags + seasonality |
| Validate stationarity | ADF/KPSS tests | Check model assumptions |

---

## 12. Practical Use in AI and Engineering

### 12.1 Highest-Value Time-Series Tools

1. **Trend analysis:** Optimization convergence, model drift detection
2. **Moving averages:** Sensor denoising, real-time smoothing
3. **AR models:** Lag persistence quantification
4. **ARIMA:** Classical forecasting benchmark
5. **SARIMA:** Seasonal forecasting (energy, demand)
6. **Stationarity tests:** Model validity checks
7. **ACF/PACF:** Model order determination

### 12.2 Applications

- **SHM sensor monitoring:** Detect anomalies; forecast degradation
- **Smart-building analytics:** Energy demand forecasting
- **Load forecasting:** Electrical demand prediction
- **Convergence trajectory:** Analyze optimization progress
- **Environmental monitoring:** Air quality, water quality prediction
- **Digital twins:** Predict and track virtual system behavior

---

## Key Takeaways

1. **Time-series decomposition** isolates trend, seasonality, and residuals; additive for stable patterns, multiplicative for scale-dependent seasonality.

2. **Trend analysis** quantifies long-term direction using linear or polynomial models; critical for degradation and growth modeling.

3. **Seasonal decomposition** reveals repeating periodic patterns; essential for demand, energy, and environmental forecasting.

4. **Moving averages and exponential smoothing** provide simple, effective smoothing and short-term forecasting.

5. **Autoregressive models (AR)** capture persistence: current value depends on past values; quantify lag dependence.

6. **ARIMA** combines autoregression, differencing, and moving average; unified framework for trend + lag forecasting.

7. **SARIMA** extends ARIMA with seasonality; use for energy, demand, and environmental data with seasonal cycles.

8. **Stationarity tests (ADF, KPSS)** determine whether differencing is needed before fitting ARIMA.

9. **ACF and PACF plots** guide order selection; ACF for MA, PACF for AR, both decay for mixed models.

---

## Connection to Next Modules

Time series methods form the foundation for Lecture 9 (Statistical Simulation and Stochastic Modeling), where you'll generate synthetic time-series data with specified properties. Uncertainty quantification (Lecture 14) builds on time-series forecasting by providing prediction intervals. Finally, advanced AI models like LSTMs and attention mechanisms (Lecture 17) apply these concepts at scale for deep temporal learning.
