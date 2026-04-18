# Exploratory Data Analysis (EDA)

Exploratory Data Analysis (EDA) is the process of **investigating datasets to understand their structure, quality, distribution, relationships, and hidden patterns before formal modeling**. It is often the most critical stage in data science and engineering analytics workflows because it answers essential questions:

- Is the data clean and reliable?
- Are there missing values, and how are they distributed?
- Are extreme outliers present? Are they valid or errors?
- What is the distributional shape of each variable?
- Are variables strongly correlated or redundant?
- Are nonlinear relationships visible?
- Are there temporal trends or seasonality?
- Does the dataset exhibit class imbalance?

A thorough EDA stage reduces downstream modeling errors, improves model interpretability, and prevents costly mistakes in production.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Conduct** systematic exploratory analysis using visualization and summary statistics
2. **Interpret** distribution properties and identify when transformations are needed
3. **Detect** outliers using multiple methods (Z-score, IQR) and assess their impact
4. **Analyze** missing data mechanisms and choose appropriate handling strategies
5. **Examine** pairwise relationships between variables to identify correlations and interactions
6. **Create** and interpret visual summaries (boxplots, histograms, correlation matrices) to communicate data characteristics

---

## 1. Distribution Analysis

Distribution analysis studies how values are spread across the range of a variable. By understanding the distributional shape, you can determine whether transformations are needed and which statistical methods are appropriate.

### 1.1 Goals of Distribution Analysis

- Identify symmetry or skewness (refer to Lecture 1: Skewness)
- Detect multimodal behavior (multiple peaks)
- Assess whether normality assumptions hold for statistical tests
- Identify bounded or truncated variables (e.g., percentages capped at 100%)
- Compare real vs. synthetic data distributions for validation

### 1.2 Key Numerical Measures

**Central Tendency** (see Lecture 1 for detailed definitions):

$$\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i$$

**Dispersion Measures** (Refer to Lecture 1 for detailed explanations):

Variance:
$$s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2$$

Standard Deviation:
$$s=\sqrt{s^2}$$

### 1.3 Shape Metrics

**Skewness** (see Lecture 1, Section 5.1):
$$g_1 = \frac{\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^3}{\left(\sqrt{\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^2}\right)^3}$$

**Kurtosis** (see Lecture 1, Section 6):
$$g_2 = \frac{\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^4}{\left(\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^2\right)^2} - 3$$

### 1.4 Why It Matters

Understanding distribution shape informs critical decisions:
- **Normalization needs:** Should we standardize or scale variables?
- **Transformation requirements:** Do we need log, square-root, or Box-Cox transformations?
- **Proper statistical tests:** Parametric tests (ANOVA, t-test) assume normality; non-parametric tests (Mann-Whitney, Kruskal-Wallis) don't
- **Model assumptions:** Tree-based models are distribution-agnostic; neural networks benefit from normalized inputs

**Check for Understanding:**
- You observe that a variable has skewness = -2.5 and kurtosis = 4. What does this tell you about the distribution shape? What transformation might help?

---

## 2. Outlier Detection

Outliers are observations that deviate strongly from the majority of data. They demand special attention because they can distort statistics and model coefficients.

### 2.1 Sources of Outliers

Outliers may represent:
- **Measurement errors** — equipment malfunction, data entry mistakes
- **Rare but valid events** — equipment failure, extreme weather, fraud
- **Sensor failures** — stuck sensors, calibration drift
- **Numerical instability** — computational errors, overflow/underflow
- **Critical engineering events** — structural failure modes, safety-critical scenarios

**Important:** Not all outliers should be removed. In safety-critical engineering, outliers may represent the most valuable (and rare) failure states.

### 2.2 Z-Score Method

The Z-score measures standardized distance from the mean:

$$z_i = \frac{x_i-\bar{x}}{s}$$

**Interpretation:**
- $|z| > 2$: unusual (falls outside ~95% of normal data)
- $|z| > 3$: highly unusual (falls outside ~99.7% of normal data)

**Limitations:**
- Assumes normally distributed data
- Sensitive to the presence of outliers (outliers inflate standard deviation)
- Less effective for skewed distributions

### 2.3 Interquartile Range (IQR) Method

The IQR method identifies outliers based on quartiles (introduced in Lecture 1):

$$IQR = Q_3 - Q_1$$

**Outlier Rules:**
- Lower outliers: $x < Q_1 - 1.5 \times IQR$
- Upper outliers: $x > Q_3 + 1.5 \times IQR$

**Advantages:**
- Robust to skewed distributions
- Doesn't assume normality
- Resistant to extreme outliers

**Visual**: Outliers appear as individual points beyond boxplot whiskers.

### 2.4 Why It Matters

Outliers can strongly distort analytical results:
- **Mean:** One extreme outlier can shift the mean arbitrarily
- **Standard deviation:** Outliers inflate variance estimates
- **Regression coefficients:** A single leverage point can reverse the slope sign
- **Model calibration:** Outlier-biased training damages generalization

**Decision Framework:**
1. **Identify** outliers using Z-score and IQR methods
2. **Investigate** the source—is it an error or a valid rare event?
3. **Document** your decision: remove, cap, separate, or keep
4. **Test impact:** Rerun analysis with and without outliers to assess sensitivity

**Check for Understanding:**
- A dataset has $Q_1 = 100$, $Q_3 = 200$, and $IQR = 100$. A data point has value 350. Is this an outlier? What is the upper bound for non-outlier observations?

---

## 3. Missing Value Analysis

Missing data analysis studies the amount, pattern, and mechanism of missing values. The strategy for handling missing data depends critically on understanding **why** data is missing.

### 3.1 Missing Data Mechanisms

**MCAR (Missing Completely At Random):**
- Missingness is unrelated to any variable, observed or unobserved
- Example: A random sensor malfunction
- **Implication:** Safe to delete rows or use simple imputation

**MAR (Missing At Random):**
- Missingness depends on observed variables, but not on unobserved data
- Example: Older sensor models fail more often, creating systematic missingness
- **Implication:** Requires analysis by group; multiple imputation recommended

**MNAR (Missing Not At Random):**
- Missingness depends on the unobserved value itself
- Example: High-income individuals less likely to report income (the missing value itself is likely high)
- **Implication:** Most problematic; may introduce bias no matter what imputation method you use

### 3.2 Missing Ratio Quantification

**Overall missing ratio:**
$$\text{Missing Ratio} = \frac{\text{Number of Missing Values}}{\text{Total Number of Values}}$$

**Per-variable missing ratio:**
$$MR_j = \frac{m_j}{n}$$

where $m_j$ is the number of missing values in variable $j$ and $n$ is the total observations.

### 3.3 Common Missing Data Treatments

1. **Deletion** — Remove rows with any missing values
   - Pro: Simple, maintains data validity
   - Con: Loses information; introduces bias if data is MAR/MNAR

2. **Mean/Median Imputation** — Fill with central tendency
   - Pro: Simple; preserves sample size
   - Con: Reduces variance; assumes MCAR; ignores relationships

3. **Mode Imputation** — Use most frequent value (categorical data)
   - Pro: Sensible for categorical variables
   - Con: Reduces diversity; can create artificial clusters

4. **Interpolation** — Fill based on neighbors (temporal or spatial data)
   - Pro: Respects trends and patterns
   - Con: Assumes smooth continuity; works best with sequential data

5. **Model-Based Imputation** — Use regression or k-NN to predict missing values
   - Pro: Leverages multivariate relationships
   - Con: Computationally expensive; can underestimate variance if not careful

### 3.4 Why It Matters

Missing values influence:
- **Bias:** Different handling strategies can introduce or reduce systematic bias
- **Variance:** Imputation introduces uncertainty; different imputations yield different results
- **Model robustness:** Models trained on imputed data may fail on truly missing values in production
- **Feature usability:** Features with >50% missingness may not be worth including

**Check for Understanding:**
- A dataset has 10,000 rows. Variable A has 500 missing values; Variable B has 9,500 missing values. Which variable should you keep? Why?

---

## 4. Boxplot Analysis

A boxplot is a compact visual summary that simultaneously displays central tendency, spread, skewness, and outliers. It's one of the most useful EDA tools.

### 4.1 Boxplot Components

- **Minimum:** Lowest value (or lower whisker bound)
- **Q₁ (Lower Hinge):** 25th percentile (bottom of the box)
- **Median (Q₂):** 50th percentile (line inside the box)
- **Q₃ (Upper Hinge):** 75th percentile (top of the box)
- **Maximum:** Highest value (or upper whisker bound)
- **Outliers:** Individual points beyond the whiskers

### 4.2 Whisker Definition

Whiskers typically extend to:
$$Q_1 - 1.5 \times IQR \quad \text{and} \quad Q_3 + 1.5 \times IQR$$

(This matches the IQR outlier detection rule from Section 2.3.)

### 4.3 What Boxplots Reveal

- **Skewness:** If the median line is off-center within the box, the data is skewed
- **Spread:** Longer boxes and whiskers indicate more variability
- **Outliers:** Points plotted individually beyond whiskers
- **Group comparison:** Multiple boxplots side-by-side show distributional differences across categories
- **Data quality:** Outliers can indicate errors or unusual events

### 4.4 Why It Matters

Boxplots efficiently communicate:
- Distribution shape at a glance
- Presence and severity of outliers
- Differences between groups or classes
- Whether synthetic data aligns with real data
- Quick visual check before formal statistical testing

**Check for Understanding:**
- You compare boxplots of two variables: Variable A has a much taller box but shorter whiskers; Variable B has a shorter box but longer whiskers. What does this suggest about relative skewness and spread?

---

## 5. Histogram Analysis

A histogram groups continuous data into bins and displays the frequency (or density) within each bin. Histograms reveal the fine structure of distributions more clearly than summary statistics alone.

### 5.1 Histogram Construction

For a variable divided into bins of width $w$:

**Frequency Density** (area-normalized):
$$\text{Density} = \frac{f_i}{n \cdot w}$$

where:
- $f_i$ = count (frequency) in bin $i$
- $n$ = total number of observations

**Property:** Total area under the histogram = 1 (when using density)

### 5.2 What Histograms Reveal

- **Normality:** Symmetric, bell-shaped histograms suggest normal distribution
- **Multimodal behavior:** Multiple peaks indicate distinct subgroups or mixture components
- **Skewness:** Asymmetric tails reveal left or right skew
- **Long tails:** Heavy tail behavior indicates outliers or rare events
- **Threshold zones:** Artificial gaps or clustering reveal data collection boundaries

### 5.3 Bin Width Effects

- **Too few bins:** Obscures important patterns (oversmoothing)
- **Too many bins:** Creates noise and fragments the distribution (undersmoothing)
- **Practical rule:** Start with $\sqrt{n}$ bins or use Sturges' rule: $k = \lceil \log_2 n + 1 \rceil$

### 5.4 Why It Matters

Histograms are essential for:
- Identifying normality violations before parametric testing
- Detecting multimodal mixtures that summarize statistics hide
- Assessing residual distributions in regression and model diagnostics
- Comparing real vs. synthetic data
- Communicating distributional properties to non-technical audiences

---

## 6. Pairwise Variable Inspection

Pairwise analysis examines relationships between pairs of variables. While univariate analysis (single variables) reveals marginal distributions, pairwise analysis reveals dependencies and interactions.

### 6.1 Common Visualization Tools

- **Scatter plots:** Show (X, Y) points; reveal linear/nonlinear relationships
- **Pair plots:** Scatter plots for all variable pairs (useful for ~5–10 variables)
- **Joint density plots:** Combine marginal histograms with a central scatter/contour plot
- **Hexbin plots:** Binned 2D histograms (effective for large datasets with overplotting)
- **Heatmaps:** Color-coded matrices for many variables at once

### 6.2 Covariance

**Covariance** measures how two variables co-vary:

$$\text{Cov}(X,Y)=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})$$

**Interpretation:**
- $\text{Cov}(X,Y) > 0$: Positive relationship (higher X tends to mean higher Y)
- $\text{Cov}(X,Y) < 0$: Negative relationship
- $\text{Cov}(X,Y) \approx 0$: No linear relationship

**Limitation:** Magnitude of covariance is scale-dependent (hard to interpret absolute values).

### 6.3 Why It Matters

Pairwise inspection detects:
- **Linear relationships:** Direct, proportional dependencies
- **Nonlinear dependencies:** Curved, polynomial, or threshold effects
- **Clusters:** Distinct groups of observations
- **Heteroscedasticity:** Spread (variance) changes across X values
- **Interaction effects:** The relationship between X and Y depends on a third variable
- **Input–response behavior:** In engineering, critical for understanding how system inputs drive outputs

**Check for Understanding:**
- Two variables have covariance = 100. Is this strong or weak relationship? What's missing from this information?

---

## 7. Correlation Matrix

A correlation matrix summarizes pairwise linear relationships among all variables simultaneously. It's a concise and widely-used tool for multivariate EDA.

### 7.1 Pearson Correlation Coefficient

**Pearson correlation** standardizes covariance to range [−1, 1]:

$$r_{xy}=\frac{\sum (x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum (x_i-\bar{x})^2\sum (y_i-\bar{y})^2}}$$

**Interpretation:**
- $r_{xy} = 1$: Perfect positive linear relationship
- $r_{xy} = 0$: No linear relationship
- $r_{xy} = -1$: Perfect negative linear relationship
- $|r_{xy}| > 0.7$: Generally considered strong correlation

### 7.2 Correlation Matrix Structure

For $p$ variables $X_1, X_2, \ldots, X_p$, the correlation matrix is:

$$R = \begin{bmatrix} 1 & r_{12} & \cdots & r_{1p} \\ r_{21} & 1 & \cdots & r_{2p} \\ \vdots & \vdots & \ddots & \vdots \\ r_{p1} & r_{p2} & \cdots & 1 \end{bmatrix}$$

**Properties:**
- Diagonal is always 1 (correlation of variable with itself)
- Symmetric: $r_{ij} = r_{ji}$

### 7.3 Limitations

- **Measures only linear relationships:** Two variables can be strongly related nonlinearly (e.g., $Y = X^2$) but have $r_{xy} \approx 0$
- **Affected by outliers:** Extreme points can distort correlations
- **Spurious correlations:** High correlation doesn't imply causation

### 7.4 Why It Matters

The correlation matrix helps detect:
- **Redundancy:** Highly correlated variables provide similar information (consider dropping one)
- **Multicollinearity:** When multiple predictors are correlated, regression coefficients become unstable
- **Latent groups:** Clusters of correlated variables suggest underlying shared factors
- **Feature reduction:** High correlations indicate dimensionality reduction opportunities
- **Synthetic data realism:** Generated data should match real correlation structure

**Example:** In a 59-feature engineering dataset (as mentioned in the lecture), a correlation matrix immediately reveals which features are redundant.

---

## 8. Cross-Tabulation

Cross-tabulation (or contingency table) summarizes the relationship between **two categorical variables**. It's the categorical analogue to scatterplots for continuous variables.

### 8.1 Contingency Table Structure

For categories $A_i$ (i = 1, ..., k) and $B_j$ (j = 1, ..., m):

$$C_{ij}=\text{count}(A_i \text{ and } B_j)$$

**Example:**
| Defect Type | Low Severity | High Severity |
|---|---|---|
| Type A | 45 | 12 |
| Type B | 30 | 8 |

### 8.2 Conditional Probability

To understand the relationship, compute conditional probabilities:

$$P(B_j|A_i)=\frac{C_{ij}}{\sum_j C_{ij}}$$

**Example:** Given a Type A defect, what's the probability it's high severity?
$$P(\text{High}|\text{Type A}) = \frac{12}{12+45} = 0.21$$

### 8.3 Why It Matters

Cross-tabulation is useful for:
- **Association analysis:** Do defect types associate with severity levels?
- **Classification evaluation:** Confusion matrices are cross-tabs of actual vs. predicted class labels
- **Categorical dependence:** Statistical tests (chi-square) assess whether categories are independent
- **Business insights:** Reveals patterns in categorical relationships

**Check for Understanding:**
- A cross-tabulation shows that 80% of red products pass quality checks, while only 40% of blue products pass. What does this suggest? What analysis would you do next?

---

## 9. Trend Inspection

Trend inspection is used when data are sequential or time-indexed (time series). It identifies long-term patterns, cycles, and structural breaks.

### 9.1 Temporal Patterns

Trend inspection reveals:
- **Increasing or decreasing trends:** Systematic long-term changes
- **Cycles and seasonality:** Regular repeating patterns (e.g., yearly, weekly)
- **Drift:** Slow, persistent shifts in level
- **Sudden regime changes:** Breaks or discontinuities (e.g., after equipment maintenance)
- **Volatility changes:** Periods of high vs. low variability

### 9.2 Simple Trend Model

A basic linear trend model:
$$y_t = a + bt + \epsilon_t$$

where:
- $a$ = intercept (baseline level)
- $b$ = trend slope (rate of change per time unit)
- $t$ = time index (0, 1, 2, ...)
- $\epsilon_t$ = random error

**Interpretation:** If $b > 0$, trend is upward; if $b < 0$, trend is downward; if $b \approx 0$, trend is flat.

### 9.3 Moving Average

**Moving Average** (MA) smooths out short-term noise to reveal trends:

$$MA_t = \frac{1}{k}\sum_{i=t-k+1}^{t} x_i$$

where $k$ is the window size (e.g., 7 for weekly moving average).

**Effect:** Larger $k$ produces smoother curves; smaller $k$ retains more detail.

### 9.4 Why It Matters

Trend analysis is critical for:
- **Sensor data (SHM):** Detect degradation trends in structural health monitoring
- **Building energy data:** Identify changes in consumption patterns, HVAC efficiency
- **Water demand forecasting:** Plan capacity based on usage trends
- **ML training curves:** Monitor loss curves to detect overfitting or convergence issues
- **Optimization runs:** Track objective function progress over algorithm iterations

---

## 10. Practical EDA in AI and Engineering

### 10.1 Common Applications

EDA is essential in:
- **FEM simulation datasets:** Validate outputs, check for numerical artifacts
- **GAN synthetic data:** Compare synthetic vs. real distributions
- **Surrogate modeling:** Understand input–output relationships before building models
- **Feature engineering:** Identify which variables matter most
- **Optimization benchmark studies:** Validate algorithm performance across diverse problem structures
- **Structural health monitoring (SHM):** Detect anomalies and degradation
- **Uncertainty quantification:** Characterize variability and tail behavior

### 10.2 Recommended EDA Workflow

For most engineering datasets:

1. **Univariate analysis** (Lecture 1 + Sections 1–5):
   - Summary statistics, distributions, outliers
   
2. **Missingness assessment** (Section 3):
   - Quantify and characterize missing data
   
3. **Pairwise relationships** (Sections 6–8):
   - Correlation matrix, scatter plots, cross-tabs
   
4. **Temporal patterns** (Section 9, if applicable):
   - Trends, cycles, regime changes
   
5. **Domain expertise validation**:
   - Do patterns align with physical understanding?
   - Are outliers explainable?

---

## Key Takeaways

1. **Distribution analysis** examines shapes and properties; use histograms, skewness, and kurtosis to guide transformation decisions.

2. **Outlier detection** requires both statistical methods (Z-score, IQR) and domain judgment; assess whether outliers are errors or valid rare events.

3. **Missing data** mechanisms (MCAR, MAR, MNAR) determine handling strategy; document your approach and assess sensitivity.

4. **Boxplots** efficiently display distribution, spread, skewness, and outliers in a compact, visual format.

5. **Correlation matrices** reveal linear relationships and redundancy; supplement with scatter plots to detect nonlinear dependencies.

6. **Trend inspection** is essential for time-indexed data; use moving averages and regression to identify patterns.

7. **EDA informs modeling:** A thorough EDA prevents surprises, guides feature engineering, and improves model quality.

---

## Connection to Next Modules

The insights from EDA directly inform downstream work. In Lecture 3 (Inferential Statistics), you'll use distributional assumptions discovered here to choose appropriate tests. In Lecture 4 (Hypothesis Testing), you'll validate whether observed patterns are statistically significant.
