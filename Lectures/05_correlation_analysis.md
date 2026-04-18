# Correlation Analysis

Correlation analysis is the statistical framework used to **measure the strength, direction, and structure of relationships between variables**. It is essential for understanding whether and how variables move together.

Correlation analysis helps answer critical questions such as:

- Do two variables increase together?
- Is the relationship linear or monotonic?
- Does one variable lag behind another?
- Is the relationship still present after controlling for other variables?
- Are time-series observations dependent on their own past?

This topic is foundational in:
- Feature engineering and selection
- Regression diagnostics
- Multivariate statistics
- Signal processing
- Structural health monitoring (SHM)
- Time-series modeling
- Deep learning sequence analysis
- Synthetic data validation

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Calculate and interpret** Pearson, Spearman, and Kendall correlation coefficients for different variable types
2. **Distinguish** between linear (Pearson) and monotonic (Spearman/Kendall) relationships
3. **Implement** partial correlation to control for confounding variables
4. **Apply** auto-correlation and cross-correlation to analyze temporal dependencies in time-series data
5. **Construct** correlation matrices to assess multivariate relationships and redundancy
6. **Explain** why correlation does not imply causation and when caution is needed

---

## 1. Foundations of Correlation

A correlation coefficient is a standardized measure of the strength and direction of a relationship between two variables. Most correlation coefficients are bounded:

$$-1 \le r \le 1$$

**Interpretation:**
- **r = +1**: Perfect positive relationship (as X increases, Y increases proportionally)
- **r = 0**: No linear/monotonic relationship (depending on the measure)
- **r = -1**: Perfect negative relationship (as X increases, Y decreases proportionally)
- **r between -1 and 1**: Partial relationship with strength indicated by |r|

### 1.1 Practical Strength Guidelines

A common rough interpretation (context-dependent) is:

- **|r| < 0.30**: Weak relationship
- **0.30 ≤ |r| < 0.50**: Moderate relationship
- **0.50 ≤ |r| < 0.70**: Strong relationship
- **|r| ≥ 0.70**: Very strong relationship

**Important:** These thresholds are domain-dependent and should always be interpreted with domain knowledge and visualizations.

**Check for Understanding:**
- A feature has correlation r = 0.35 with the response variable. Is this strong enough to include in a regression model? What else should you consider?

---

## 2. Pearson Correlation

The **Pearson correlation coefficient** measures the strength of a **linear relationship** between two continuous variables. It is the most widely used correlation measure.

### 2.1 Mathematical Definition

$$r_{xy} = \frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i-\bar{x})^2\sum_{i=1}^{n}(y_i-\bar{y})^2}}$$

**Equivalent covariance form:**

$$r_{xy}=\frac{\operatorname{Cov}(X,Y)}{s_X s_Y}$$

where:
- **Cov(X,Y)**: Covariance between X and Y (see Lecture 2: Pairwise Variable Inspection)
- **s_X, s_Y**: Standard deviations of X and Y respectively

### 2.2 Key Properties

- **Dimensionless:** Standardized to range [-1, 1] regardless of variable units
- **Symmetric:** r_xy = r_yx
- **Linear-specific:** Only measures linear relationships (may miss curved or other patterns)
- **Sensitive to outliers:** Extreme values can distort the coefficient
- **Scale-invariant:** Unaffected by linear transformations (e.g., converting units)

### 2.3 When to Use Pearson

Use Pearson when:
- Both variables are continuous and measured on interval/ratio scale
- The relationship appears linear (verify with scatter plot)
- Normality is roughly acceptable
- Outliers are limited or handled appropriately

### 2.4 Practical Example

**Example:** In FEM surrogate modeling, you compute the Pearson correlation between a design parameter (e.g., beam thickness) and structural displacement. A correlation of 0.82 indicates a strong positive linear relationship: thicker beams tend to reduce displacement.

**Check for Understanding:**
- A scatter plot shows a clear curved relationship between two variables. The Pearson correlation is 0.15. Does this mean the variables are unrelated?

---

## 3. Spearman Rank Correlation

The **Spearman rank correlation** measures the strength of a **monotonic relationship** between variables. Instead of using raw values, it works with ranks, making it more robust to outliers and nonlinear patterns.

### 3.1 Mathematical Definition

**For data without ties (simplified):**

$$\rho = 1 - \frac{6\sum d_i^2}{n(n^2-1)}$$

where:
- **d_i**: Difference between ranks of X and Y for observation i
- **n**: Number of observations

**General form (with ties):**
Spearman correlation is simply Pearson correlation applied to the ranks:

$$\rho = r(\operatorname{rank}(X), \operatorname{rank}(Y))$$

### 3.2 How It Works

1. **Rank** the X values from 1 to n (lowest to highest)
2. **Rank** the Y values from 1 to n
3. **Compute** Pearson correlation on the rank pairs

### 3.3 When to Use Spearman

Use Spearman when:
- Relationship is monotonic but nonlinear (consistently increases or decreases)
- Variables are ordinal (natural ordering exists)
- Outliers are present (ranks are robust to extreme values)
- Sample size is small
- Data violate normality assumptions

### 3.4 Example Use Cases

- Model complexity rank vs. performance rank in optimization
- Benchmark method ranking comparisons
- Ordinal survey scores relationships
- Quality control severity rankings

**Check for Understanding:**
- A variable X = [1, 2, 3, 4, 5] and Y = [1, 4, 9, 16, 25]. The Spearman correlation is perfect (ρ = 1), but Pearson correlation is not. Why?

---

## 4. Kendall Tau Correlation

The **Kendall tau coefficient** measures ordinal association using **concordant and discordant pairs**. It is often more interpretable than Spearman for assessing ranking consistency.

### 4.1 Mathematical Definition

$$\tau = \frac{C-D}{\frac{n(n-1)}{2}}$$

where:
- **C**: Number of concordant pairs (pairs with same relative ordering in both X and Y)
- **D**: Number of discordant pairs (pairs with opposite relative ordering)
- **n(n-1)/2**: Total number of pairs

### 4.2 Interpretation

- **τ = 1**: Perfect agreement in ranking
- **τ = 0**: No agreement (random ordering)
- **τ = -1**: Perfect disagreement (reverse ordering)

### 4.3 Tie-Adjusted Version (Kendall Tau-b)

For data with ties, the standard version is:

$$\tau_b = \frac{C-D}{\sqrt{(T_x)(T_y)}}$$

where T_x and T_y account for ties in X and Y respectively.

### 4.4 Why Kendall Tau Is Important

Kendall tau is excellent for:
- Ranking agreement assessment (do two evaluators rank items similarly?)
- Optimizer ranking stability (does ranking change between runs?)
- Comparing reviewer rankings (consensus evaluation)
- Ordinal scientific judgments

### 4.5 Kendall vs. Spearman

- **Spearman:** Uses rank distances; more common in practice
- **Kendall:** Uses pair agreement; often more statistically robust for small samples; more directly interpretable as probability

**Comparison Example:** For the same data, Kendall tau typically gives slightly lower values than Spearman. Both are robust to outliers, but Kendall may be preferred when sample sizes are very small.

---

## 5. Partial Correlation

The **partial correlation** measures the relationship between two variables **after controlling for one or more additional variables**. This is critical when confounding variables or multivariate dependencies exist.

### 5.1 First-Order Partial Correlation (Controlling One Variable)

To measure the relationship between X and Y while controlling for Z:

$$r_{xy\cdot z} = \frac{r_{xy}-r_{xz}r_{yz}}{\sqrt{(1-r_{xz}^2)(1-r_{yz}^2)}}$$

where:
- **r_xy**: Pearson correlation between X and Y (unadjusted)
- **r_xz**: Correlation of X and Z
- **r_yz**: Correlation of Y and Z
- **r_xy·z**: Partial correlation of X and Y controlling for Z

### 5.2 Interpretation

**r_xy·z** tells you: **"If we remove the influence of Z from both X and Y, how strongly are X and Y related?"**

**Example:** In engineering, you might observe a correlation between design parameter A and failure rate. But both might be influenced by material grade B. The partial correlation r_AB·grade shows the true relationship between A and B after removing the material grade effect.

### 5.3 Why Partial Correlation Matters

This helps answer critical questions:

- **Is the relationship real, or only caused by a third variable?**
- Which variables remain important after controlling for others?
- What is the "pure" relationship after accounting for confounders?

### 5.4 Practical Applications

- Feature relevance after controlling for other inputs
- Hidden confounders in engineering response analysis
- Causal-style exploratory reasoning in observational data
- GAN synthetic data validation (correlations should match real data even after controlling)

**Example in Structural Engineering:** For your FEM surrogate datasets, you might find that beam thickness and material stiffness are both correlated with displacement. Using partial correlation, you can determine whether thickness matters even after controlling for stiffness.

**Check for Understanding:**
- Two variables have r_xy = 0.6. After controlling for a third variable, r_xy·z = 0.15. What does this suggest?

---

## 6. Auto-Correlation

**Auto-correlation** (or autocorrelation) measures the correlation of a variable with its own lagged values. This is fundamental in time-series analysis and sequential data.

### 6.1 Mathematical Definition

**Lag-k auto-correlation:**

$$\rho_k = \frac{\sum_{t=k+1}^{n}(x_t-\bar{x})(x_{t-k}-\bar{x})}{\sum_{t=1}^{n}(x_t-\bar{x})^2}$$

where:
- **k**: The lag (how many time steps back)
- **x_t**: Value at time t
- **x_t-k**: Value k time steps earlier

### 6.2 Interpretation

- **High positive ρ_k**: Strong persistence or memory (value tends to stay near previous values)
- **Negative ρ_k**: Oscillation tendency (value tends to alternate above/below mean)
- **Near zero ρ_k**: Weak temporal dependence; current value independent of past

**Example:** For optimization convergence history, high auto-correlation at lag-1 suggests smooth convergence, while negative auto-correlation suggests oscillating improvements.

### 6.3 ACF Plot

An **autocorrelation function (ACF) plot** shows ρ_k for multiple lags k = 1, 2, 3, ..., helping identify temporal patterns:
- Slow decay → trend or non-stationarity
- Sharp cutoff → moving average behavior
- Regular cycles → seasonal patterns

### 6.4 Applications

- SHM sensor streams (is vibration dependent on previous measurements?)
- Optimization convergence history (is improvement persistent?)
- ML training loss curves (are improvements stable or oscillating?)
- Demand forecasting (do past demands predict future ones?)

**Example for Your Work:** For your convergence analysis, auto-correlation can quantify whether optimizer improvements are **temporally persistent** or subject to random fluctuations.

---

## 7. Cross-Correlation

**Cross-correlation** measures similarity between two signals or sequences across time lags. It is useful when one variable may influence another after a delay (lagged relationship).

### 7.1 Mathematical Definition

**Discrete lag-k cross-correlation (unnormalized):**

$$R_{xy}(k)=\sum_t x_t y_{t-k}$$

**Normalized form:**

$$\rho_{xy}(k)=\frac{\sum_t (x_t-\bar{x})(y_{t-k}-\bar{y})}{\sqrt{\sum_t (x_t-\bar{x})^2\sum_t (y_t-\bar{y})^2}}$$

where:
- **k**: Time lag (Y lags behind X by k steps)
- **t**: Time index

### 7.2 Interpretation

**ρ_xy(k) peaks at lag k means:** X's behavior at time t predicts Y's behavior at time t+k.

- **Positive ρ at lag k > 0:** Y follows X with k-step delay
- **Negative ρ at lag k > 0:** Y opposes X with delay
- **Peak at k = 0:** Contemporaneous relationship (no lag)

### 7.3 Why Cross-Correlation Is Important

Cross-correlation helps detect:
- Lagged dependencies (cause-effect with time delay)
- Delayed response mechanisms
- Cause-effect timing hints (for causal inference)
- Signal synchronization and alignment
- System identification in engineering

### 7.4 Example Applications

- **Structural engineering:** Wind load vs. bridge response delay (load causes response after brief lag)
- **Sensor networks:** Sensor signal alignment (do multiple sensors measure the same phenomenon with phase shifts?)
- **Forecasting:** Predicted vs. observed time shift (do predictions systematically lag?)
- **Training metrics:** Learning rate change vs. loss improvement delay

**Example in SHM:** For structural health monitoring, cross-correlation between input excitation (wind, seismic) and structural response reveals the system's transfer characteristics and can indicate damage (phase shift changes).

**Check for Understanding:**
- Cross-correlation shows maximum positive value at lag k=5. What does this suggest about the relationship between the two signals?

---

## 8. Correlation vs. Causation

A critical scientific warning that must always be remembered:

**Correlation does not imply causation.**

A high correlation may arise from several mechanisms:

### 8.1 Possible Explanations for Correlation

1. **Direct causation:** X directly causes Y (true causal effect)
2. **Reverse causation:** Y actually causes X (causality reversed)
3. **Confounding variable:** Third variable Z causes both X and Y to move together
4. **Common trend:** Both X and Y follow same underlying trend (e.g., seasonal pattern)
5. **Pure coincidence:** Random alignment with no meaningful mechanism

### 8.2 Example

Observation: Ice cream sales and drowning deaths are highly correlated.

**Incorrect conclusion:** Ice cream causes drowning!

**Correct explanation:** Both are driven by temperature (confounding variable). Summer heat increases ice cream sales AND increases swimming (drowning risk).

### 8.3 Why This Matters for Your Work

- Correlation in features doesn't mean causal relationships
- High correlation between predictors can indicate multicollinearity, not importance
- Observed correlations in GAN synthetic data may not reflect true underlying mechanisms
- Engineering relationships require mechanistic understanding, not just correlation

### 8.4 Tools to Investigate Causation

- **Partial correlation:** Remove confounders' effects
- **Experimental design:** Random assignment controls confounding
- **Causal inference methods:** Covered in Lecture 15 (Causal Inference)
- **Domain expertise:** Understanding physical mechanisms

---

## 9. Correlation Matrix

For datasets with multiple variables, pairwise correlations are typically organized into a correlation matrix.

### 9.1 Mathematical Form

For p variables, the correlation matrix is:

$$R = \begin{bmatrix} 1 & r_{12} & r_{13} & \cdots & r_{1p} \\ r_{21} & 1 & r_{23} & \cdots & r_{2p} \\ r_{31} & r_{32} & 1 & \cdots & r_{3p} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ r_{p1} & r_{p2} & r_{p3} & \cdots & 1 \end{bmatrix}$$

**Properties:**
- Symmetric: R^T = R (r_ij = r_ji)
- Diagonal entries = 1 (correlation of each variable with itself)
- All entries bounded in [-1, 1]

### 9.2 Interpretation

Key patterns to look for:
- **High correlations (|r| > 0.7):** Feature redundancy; consider removing one
- **Multicollinearity:** Multiple predictors highly intercorrelated; regression becomes unstable
- **Block structure:** Variables cluster into groups with high within-group correlations
- **Sparse correlations:** Few strong relationships; variables are largely independent

### 9.3 Visual Representation

Correlation matrices are often visualized as **heatmaps**:
- Color intensity indicates strength (darker = higher |r|)
- Positive correlations typically one color (e.g., blue)
- Negative correlations another color (e.g., red)
- Heatmaps make patterns visually obvious

### 9.4 Applications

- Feature redundancy detection
- Multicollinearity assessment before regression
- Dimensionality reduction (PCA) preparation
- Clustering detection
- GAN synthetic data validation (compare real vs. synthetic correlation structures)

**Example for Your Work:** For your 59-input structural tabular datasets, a correlation matrix immediately reveals which input features are redundant, helping reduce model complexity.

**Check for Understanding:**
- A correlation matrix shows that features A and B have r = 0.92. What should you do? Why?

---

## 10. Practical Use in AI and Engineering

### 10.1 Highest-Value Correlation Tools

For your research workflow:

1. **Pearson correlation:** Feature-response linearity in regression and surrogate modeling
2. **Spearman correlation:** Rank-based benchmark relationships and nonlinear monotonic patterns
3. **Kendall tau:** Ranking consistency in algorithm comparisons
4. **Partial correlation:** Confounder analysis in observational data
5. **Auto-correlation:** Convergence history analysis and temporal pattern detection
6. **Cross-correlation:** Sensor signal lag analysis and system identification
7. **Correlation matrices:** Feature engineering, redundancy detection, multicollinearity assessment

### 10.2 Applications

These tools directly support:
- FEM surrogate modeling (feature-response relationships)
- SHM data analysis (temporal and lag relationships)
- Optimizer comparison (ranking consistency)
- GAN synthetic validation (correlation structure matching)
- Feature engineering (redundancy detection)
- Temporal AI models (sequential dependencies)
- Algorithm benchmarking (ranking correlations)

---

## 11. Choosing the Right Correlation Measure

### Quick Decision Guide

- **Pearson (r)** → Linear continuous relationship; most common; assumes approximate normality
- **Spearman (ρ)** → Monotonic rank relationship; robust to outliers and nonlinearity
- **Kendall (τ)** → Ranking agreement; small samples; interpretable as pair concordance probability
- **Partial (r·z)** → Control confounders; multivariate confounding
- **Auto-correlation (ρ_k)** → Same variable across time lags; temporal dependencies
- **Cross-correlation (ρ_xy(k))** → Two variables across time lags; lagged relationships

---

## Key Takeaways

1. **Pearson correlation** measures linear relationships and is sensitive to outliers; use for well-behaved continuous data.

2. **Spearman and Kendall** are robust rank-based measures; use when relationships are monotonic but nonlinear or when outliers are present.

3. **Partial correlation** reveals true relationships after removing confounding effects; essential for multivariate reasoning.

4. **Auto-correlation** quantifies temporal self-dependence; critical for time-series and sequential data analysis.

5. **Cross-correlation** identifies lagged relationships between variables; useful for system identification and delay detection.

6. **Correlation matrices** summarize multivariate relationships; detect redundancy, multicollinearity, and clustering.

7. **Correlation does not imply causation**; use domain knowledge, experimental design, and causal methods to investigate mechanisms.

---

## Connection to Next Modules

Correlation analysis forms the foundation for Lecture 6 (Regression Analysis), where you'll use correlations to build predictive models and assess multicollinearity. The concept of controlling for variables extends to Lecture 7 (Multivariate Analysis) and Lecture 15 (Causal Inference), where you'll employ more sophisticated methods to isolate true relationships in complex datasets.
