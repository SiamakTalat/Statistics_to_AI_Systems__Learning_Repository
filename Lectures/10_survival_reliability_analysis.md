# Survival and Reliability Analysis

Survival and reliability analysis is the statistical framework used to **model time-to-event, failure behavior, hazard evolution, and system lifetime uncertainty**.

It is fundamental when the main questions are:

- **When will failure happen?** Predict failure time
- **How likely is survival beyond time t?** Compute survival probability
- **Which variables accelerate failure?** Identify risk factors
- **How does hazard evolve over time?** Understand failure risk dynamics
- **What is the reliability of a component or system?** Quantify lifetime expectations

This module is central to:
- Structural reliability and safety
- Fatigue life prediction
- Predictive maintenance and condition monitoring
- Equipment failure analytics
- Medical and engineering survival analysis
- Sensor degradation modeling
- Digital twin development
- Risk-informed engineering and asset management

For your AI + structural engineering workflows, this is extremely valuable for **failure-time prediction, maintenance optimization, and structural lifetime modeling**.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Explain** survival functions, hazard functions, and their relationships
2. **Handle censoring** (incomplete data) appropriately in lifetime analysis
3. **Apply Kaplan-Meier estimator** to produce nonparametric survival curves
4. **Interpret Cox Proportional Hazards model** for identifying risk factors
5. **Fit Weibull distributions** for parametric lifetime modeling
6. **Calculate** reliability metrics (MTTF, MTBF, system reliability)
7. **Design maintenance strategies** based on hazard function shape

---

## 1. Foundations of Survival Analysis

### 1.1 The Failure Time Variable

The primary variable of interest in survival analysis is the **failure time**:

$$T > 0$$

where T represents the time until an event of interest occurs (failure, death, breakdown, etc.).

**Examples:**
- Component failure time (cycles to crack)
- Patient survival time (years post-diagnosis)
- Device degradation time (hours until performance drops below threshold)
- Bridge deterioration time (years until repair needed)

### 1.2 Censoring: The Key Challenge

A critical feature of survival data is **censoring**—the exact failure time is not always observed.

**Right Censoring (most common):**
- Component has not failed by observation end
- We only know T > t_c (failed sometime after observation stopped)
- Example: Monitoring bridge for 10 years; if it hasn't failed, we only know T > 10

**Left Censoring:**
- Failure already occurred before observation began
- We only know T < t_start

**Interval Censoring:**
- Failure occurred sometime between two observations
- Example: Monthly inspections; failure happened sometime in the month

**Important:** Standard regression methods ignore censoring and give biased results. Survival analysis methods properly account for censored observations.

**Check for Understanding:**
- In a 5-year predictive maintenance study, if a bearing fails at year 3, is it censored? If it's still operating at year 5, is it censored?

---

## 2. Survival Function

The **survival function** gives the probability of surviving (remaining operational) beyond time t:

$$S(t)=P(T>t) = 1 - F(t)$$

where F(t) is the cumulative failure distribution.

### 2.1 Properties

- **S(0) = 1:** Everyone/everything "survives" at time 0
- **S(∞) = 0:** Eventually everything fails (under sufficient time)
- **Monotonically decreasing:** Survival probability never increases
- **Interpretation:** S(t) is the proportion of the population still functioning at time t

### 2.2 Example Interpretation

If S(1000) = 0.85, this means:
- **85% probability of surviving beyond 1000 cycles**
- **Equivalently, 15% probability of failure by 1000 cycles**

### 2.3 Relationship to Other Functions

Let:
- **F(t) = P(T ≤ t):** Cumulative failure probability
- **f(t) = dF/dt:** Failure probability density

Then:
$$S(t) = 1 - F(t)$$

---

## 3. Hazard Function

The **hazard function** h(t) models the **instantaneous failure risk** at time t, given that the component has survived until time t.

### 3.1 Mathematical Definition

$$h(t)=\lim_{\Delta t\to 0}\frac{P(t\le T < t+\Delta t \mid T \ge t)}{\Delta t}$$

**Interpretation:** Probability of failure in the next infinitesimal time interval, conditional on surviving until time t.

**Alternative form:**
$$h(t) = \frac{f(t)}{S(t)} = -\frac{d}{dt}\log S(t)$$

### 3.2 Cumulative Hazard

The cumulative hazard accumulates risk over time:

$$H(t) = \int_0^t h(u) du$$

**Relationship:**
$$S(t) = e^{-H(t)}$$

### 3.3 Hazard Shapes and Their Interpretation

- **Decreasing hazard (h'(t) < 0):** Infant mortality; early failures removed; improving reliability over time
- **Constant hazard (h(t) = λ):** No aging effect; failures random and memoryless (exponential distribution)
- **Increasing hazard (h'(t) > 0):** Wear-out; aging effect; failure risk accelerates with time

**Bathtub Curve:** Many real systems show all three phases: early failures → constant hazard → wear-out.

### 3.4 Why Hazard Matters

- **Failure risk evolution:** Understand when failures are most likely
- **Maintenance scheduling:** Plan preventive maintenance based on hazard increase
- **Component aging:** Quantify wear-out rate
- **System design:** Account for failure acceleration in safety margins

**Example in SHM:** A concrete bridge may have decreasing hazard initially (construction defects fixed), then constant hazard (normal aging), then increasing hazard (deterioration acceleration). Knowing this curve guides maintenance timing.

**Check for Understanding:**
- If a bearing's hazard function is constant over time, what type of failures does it experience? Why would it differ from a fracture-mechanics failure?

---

## 4. Kaplan-Meier Estimator

The **Kaplan-Meier estimator** is the most important nonparametric method for estimating survival curves, especially when data contain censoring.

### 4.1 Formula

$$\hat{S}(t)=\prod_{t_i\le t}\left(1-\frac{d_i}{n_i}\right)$$

where:
- **d_i:** Number of failures at time t_i
- **n_i:** Number at risk (still functioning) just before time t_i

### 4.2 How It Works

1. **List all failure times** (and censoring times)
2. **For each failure time t_i:**
   - Count failures: d_i
   - Count at risk: n_i
   - Update survival: S(t_i) = S(t_{i-1}) × (1 - d_i/n_i)
3. **Between failure times:** S(t) = constant (stepwise function)

### 4.3 Properties

- **Handles censoring:** Properly accounts for censored observations
- **Nonparametric:** Makes no distributional assumptions
- **Stepwise:** Survival curve is flat between events
- **Confidence bands:** Can add 95% confidence intervals

### 4.4 Applications

- **Survival curves:** Visualize and compare lifetime distributions
- **Maintenance studies:** Compare maintenance strategies
- **Fatigue survival plots:** Standard in materials engineering
- **Algorithm comparison:** "Survival" under stopping criteria (e.g., convergence)

**Example:** Compare bearing survival under two different materials. Plot Kaplan-Meier curves; the material with higher S(t) survives longer on average.

---

## 5. Cox Proportional Hazards Model

The **Cox model** is the most important semi-parametric survival regression. It models how predictors (covariates) influence the hazard function.

### 5.1 Model Structure

$$h(t|X) = h_0(t) \exp(\beta^T X)$$

where:
- **h(t|X):** Hazard given predictors X
- **h_0(t):** Baseline hazard (unspecified, nonparametric)
- **X:** Predictor variables (p × 1 vector)
- **β:** Effect coefficients (p × 1 vector to estimate)

### 5.2 Interpretation: Hazard Ratios

For a one-unit increase in predictor x_j:

$$HR_j = e^{\beta_j}$$

**Interpretation:**
- **HR = 1:** No effect; hazard unchanged
- **HR > 1:** Risk factor; hazard increases; β_j > 0
- **HR < 1:** Protective factor; hazard decreases; β_j < 0

**Example:** If temperature coefficient β = 0.05, then HR = e^0.05 = 1.051. Each 1°C temperature increase multiplies hazard by 1.051 (5.1% increase).

### 5.3 Proportional Hazards Assumption

The key assumption is that the **hazard ratio is constant over time**:

$$\frac{h(t|X=x_1)}{h(t|X=x_2)} = \text{constant}$$

This means relative risk doesn't change with time. Can be tested via residual plots or statistical tests.

### 5.4 Estimation

Uses **partial likelihood** (not full likelihood, since h_0(t) is unspecified). Estimated via Newton-Raphson iteration.

### 5.5 Applications

- **Effect estimation:** Which variables accelerate failure?
- **Risk scoring:** Assign risk scores based on covariates
- **Maintenance optimization:** When to service based on risk factors
- **Reliability informed design:** Understand how design parameters affect lifetime

**Example in Structural Engineering:** Model bridge fatigue crack growth as function of:
- Load intensity
- Environmental exposure (temperature, humidity)
- Material properties
- Crack size

Cox model reveals which factors most accelerate crack propagation, guiding maintenance strategy.

**Check for Understanding:**
- If a Cox model shows HR = 2.5 for a material type, what does this mean for failure risk?

---

## 6. Weibull Analysis

The **Weibull distribution** is one of the most important lifetime models in engineering. It provides a parametric framework (unlike Cox, which is semi-parametric).

### 6.1 Survival Function

$$S(t) = \exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]$$

where:
- **η (eta):** Scale parameter (characteristic life; 63.2% failure point)
- **β (beta):** Shape parameter

### 6.2 Hazard Function

$$h(t) = \frac{\beta}{\eta}\left(\frac{t}{\eta}\right)^{\beta-1}$$

### 6.3 Shape Parameter Interpretation

- **β < 1:** **Infant mortality / decreasing hazard**
  - Early failures dominant; reliability improves with age
  - Common in electronics (burn-in period)
  
- **β = 1:** **Constant hazard / exponential distribution**
  - No aging; memoryless
  - Random failures independent of age
  
- **β > 1:** **Aging / wear-out / increasing hazard**
  - Failure risk accelerates with time
  - Typical for mechanical wear, fatigue, degradation
  - **β = 2:** Linear increasing hazard
  - **β = 3.5:** Approximately normal distribution

### 6.4 Parameter Estimation

**Method of moments or maximum likelihood:**
- Estimate β from shape of observed failure distribution
- Estimate η from scale of distribution

### 6.5 Why Weibull Is Important

- **Flexible:** Single family covers decreasing, constant, and increasing hazards
- **Practical:** Default choice for engineering lifetime modeling
- **Scalable:** Works well from small datasets to full population studies
- **Interpretable:** Shape parameter directly reveals failure mode

### 6.6 Applications

- **Fatigue life prediction:** Cycles to crack initiation/propagation
- **Component wear:** Predictive maintenance timing
- **Bridge deterioration:** Rate of concrete/steel degradation
- **Bearing failure:** Common assumption in condition monitoring

**Example:** Analysis of bearing failures shows β = 1.8. This indicates wear-out (increasing hazard), supporting preventive maintenance strategy.

---

## 7. Failure Rate and MTBF

For **repairable systems** (components replaced after failure and put back in service), we track failure occurrences.

### 7.1 Failure Rate

$$\lambda = \frac{\text{Number of failures}}{{\text{Total operating time}}}$$

**Units:** Failures per unit time (e.g., failures/year, failures/million hours).

### 7.2 Mean Time Between Failures (MTBF)

$$MTBF = \frac{1}{\lambda}$$

**Interpretation:** Average time between consecutive failures.

**Example:** If λ = 0.02 failures/month, then MTBF = 50 months.

### 7.3 Relationship to Survival

For constant hazard (exponential lifetimes):
- λ = h(t) (hazard equals failure rate)
- MTBF = MTTF (same for exponential)

### 7.4 Use Cases

- **Maintenance KPIs:** Track system reliability over time
- **Equipment performance:** Compare reliability of different models
- **Predictive maintenance:** Estimate when next failure expected
- **System availability:** MTBF affects uptime calculations

---

## 8. Reliability Estimation

**Reliability** is the probability that a system/component survives its mission time t_m without failure:

$$R(t) = P(T > t) = S(t)$$

### 8.1 Mean Time To Failure (MTTF)

$$MTTF = E[T] = \int_0^\infty S(t) dt$$

**Interpretation:** Expected lifetime.

**For exponential:** MTTF = 1/λ
**For Weibull:** MTTF = η · Γ(1 + 1/β)

### 8.2 System Reliability

When multiple components form a system:

**Series System (all must survive):**
$$R_s(t) = \prod_{i=1}^{n} R_i(t)$$

**Interpretation:** System reliability is product of component reliabilities (most stringent).

**Parallel System (at least one survives):**
$$R_p(t) = 1 - \prod_{i=1}^{n} [1 - R_i(t)]$$

**Interpretation:** System reliability is higher than any single component (redundancy benefit).

### 8.3 Structural Redundancy

**Example:** Bridge with two load paths. Each path has R_path = 0.99 over design life. System reliability:
$$R_{system} = 1 - (1-0.99)^2 = 1 - 0.0001 = 0.9999$$

Redundancy dramatically improves system reliability.

**Check for Understanding:**
- If three independent components each have R = 0.9 over mission time, what is reliability of a series system? A parallel system?

---

## 9. Practical Guide: Choosing Reliability Methods

| Goal | Method | Use When |
|------|--------|----------|
| Empirical survival curve | Kaplan-Meier | Have failure/censoring data; nonparametric estimate wanted |
| Covariate effects | Cox PH model | Want to identify risk factors; flexible distributional assumption |
| Parametric modeling | Weibull | Strong evidence of failure mode (increasing/decreasing hazard) |
| MTBF/availability | Failure rate analysis | Repairable systems; maintenance tracking |
| Mission reliability | Reliability estimation | Need P(survive to time t); system configurations |
| Maintenance timing | Hazard modeling | Want optimal preventive maintenance schedule |

---

## 10. Practical Use in AI and Engineering

### 10.1 Highest-Value Tools

1. **Weibull distribution:** Fatigue life, wear-out modeling
2. **Cox PH model:** Degradation drivers, maintenance covariates
3. **Kaplan-Meier:** Observed survival comparison, material/design benchmarking
4. **Hazard modeling:** Instantaneous failure risk, maintenance scheduling
5. **System reliability:** Redundancy evaluation, robustness analysis

### 10.2 Applications

These directly support:
- **Predictive maintenance:** SHM sensor stream analysis
- **Bridge lifetime modeling:** Concrete/steel deterioration prediction
- **Structural fatigue:** S-N curve to Weibull translation
- **Failure-aware digital twins:** Real-time degradation tracking
- **Reliability-informed optimization:** Design for specified target reliability
- **Risk-based asset management:** Maintenance prioritization

---

## Key Takeaways

1. **Survival function S(t)** = P(T > t); main object of interest; decreases from 1 to 0.

2. **Hazard function h(t)** = instantaneous failure risk given survival; shapes reveal failure mode (infant mortality, random, wear-out).

3. **Censoring** (incomplete lifetime data) is handled properly by survival methods; standard regression gives biased results.

4. **Kaplan-Meier estimator** provides nonparametric empirical survival curves; handles censoring naturally.

5. **Cox Proportional Hazards** relates covariates to hazard; yields hazard ratios for effect interpretation.

6. **Weibull distribution** is parametric standard for engineering lifetimes; shape parameter β distinguishes failure modes.

7. **System reliability** analysis accounts for series (product) and parallel (redundancy) configurations.

8. **Maintenance strategy** design should follow hazard shape: predictive for increasing hazard, reactive for constant.

---

## Connection to Next Modules

Survival analysis connects to Lecture 11 (Resampling and Validation), where bootstrap methods provide confidence bounds for survival curves. Lecture 14 (Uncertainty Quantification) builds on reliability estimation for robust design under uncertainty. Finally, Lecture 15 (Causal Inference) extends survival regression to causal effect estimation in time-to-event settings.
