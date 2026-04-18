# Descriptive Statistical Analysis

Descriptive statistical analysis is the foundation of all data-driven reasoning. It focuses on **summarizing, organizing, and interpreting the main characteristics of a dataset** before moving into inference, modeling, or decision-making.

This module introduces the most important descriptive tools used in statistics, AI, engineering, and data science.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Explain** the purpose and role of descriptive statistics in data analysis and data science workflows
2. **Calculate** measures of central tendency (mean, median, mode) and interpret their differences in context
3. **Quantify** data spread using variance, standard deviation, and interquartile range for different distributions
4. **Construct** and interpret frequency tables for both categorical and numerical data
5. **Diagnose** distribution shape using skewness and kurtosis to inform modeling decisions
6. **Prepare** datasets for downstream modeling by analyzing and understanding distributional properties

---

## 1. Measures of Central Tendency

Central tendency measures describe the "typical" or "center" value of a dataset. Choosing the right measure depends on your data distribution and the presence of outliers.

### 1.1 Mean

The **arithmetic mean** (or average) measures the central tendency of numerical data.

$$\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i$$

**Key Properties:**
- Sensitive to extreme values (outliers can pull the mean significantly)
- Uses all data points in the calculation
- Appropriate for symmetric, normally distributed data
- The "center of mass" of the data

**When to Use:** For normally distributed data without extreme outliers, or when you want to give equal weight to all observations.

**Example:** For dataset [10, 12, 14, 16], the mean is 13. But for [10, 12, 14, 100], the mean is 34, heavily influenced by the outlier 100.

### 1.2 Median

The **median** is the middle value when data is ordered from smallest to largest. For an even number of observations, it's the average of the two middle values.

**Key Properties:**
- Robust to outliers (resistant to extreme values)
- Ideal for skewed distributions
- Depends on position, not magnitude, of values
- The 50th percentile

**When to Use:** For skewed data, when outliers are present, or when you need a resistant measure of center.

**Example:** For dataset [10, 12, 14, 100], the median is 13 (average of 12 and 14), unaffected by the outlier 100.

**Check for Understanding:**
- Compare the mean and median for household incomes in a city. Why might they differ significantly? Which would you report to the public?

### 1.3 Mode

The **mode** is the most frequently occurring value or category. A dataset can have one mode (unimodal), multiple modes (multimodal), or no mode.

**Common Uses:**
- **Categorical data**: Which product color is most popular?
- **Discrete counts**: What is the most common number of defects?
- **Class imbalance checks**: Which category dominates the dataset?

**Example:** In a dataset of product colors [red, blue, red, green, red], the mode is "red" (appears 3 times out of 5).

---

## 2. Measures of Dispersion (Spread)

While measures of central tendency tell us about the center, measures of dispersion quantify how spread out the data is around that center. Two datasets can have the same mean but very different dispersions.

### 2.1 Range

The **range** measures the total span of data.

$$\text{Range}=x_{\max}-x_{\min}$$

**Limitations:**
- Uses only the extreme values, ignoring the distribution between them
- Highly sensitive to outliers
- Doesn't indicate where most data points cluster

**When to Use:** Quick, rough estimate of spread; rarely used alone in serious analysis.

### 2.2 Variance

**Variance** quantifies the average squared deviation from the mean.

$$s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2$$

**Properties:**
- Each deviation is squared, so larger deviations have disproportionate weight
- Measured in **squared units** of the original data (not intuitive)
- Higher variance indicates more spread; lower variance indicates data clustered near the mean

**Check for Understanding:**
- If you calculate variance in dollars² for price data, how would you interpret a variance of 25 (dollars²)?

### 2.3 Standard Deviation

The **standard deviation** is the square root of variance.

$$s=\sqrt{s^2}$$

**Advantages:**
- Measured in the same units as the original data (intuitive interpretation)
- The most widely used measure of dispersion
- For normally distributed data: ~68% of data falls within 1 standard deviation of the mean

**Example:** A dataset with mean 100 and standard deviation 15 tells you that most values fall roughly between 85 and 115.

### 2.4 Interquartile Range (IQR)

The **IQR** measures the spread of the middle 50% of data.

$$IQR=Q_3-Q_1$$

where $Q_1$ is the 25th percentile and $Q_3$ is the 75th percentile.

**Advantages:**
- Robust to outliers (ignores the top 25% and bottom 25% of data)
- Preferred for skewed distributions
- Foundation for outlier detection (standard rule: values beyond $Q_3 + 1.5 \times IQR$ are outliers)

**When to Use:** For skewed data or when outliers are present; often visualized with box plots.

---

## 3. Percentiles and Quartiles

**Percentiles** divide ordered data into 100 equal parts. The p-th percentile is the value below which p% of observations fall.

**Quartiles** are specific, commonly-used percentiles:
- **Q₁ (First Quartile):** 25th percentile — 25% of data is at or below this value
- **Q₂ (Second Quartile):** 50th percentile = **Median** — 50% of data is at or below
- **Q₃ (Third Quartile):** 75th percentile — 75% of data is at or below this value

**Practical Example:** If a student's test score is at the 85th percentile, that student performed better than 85% of all test-takers.

**Check for Understanding:**
- In a dataset of 100 salaries, the 75th percentile is $80,000. What percentage of employees earn more than $80,000?

---

## 4. Frequency Tables and Distributions

A **frequency table** is a structured summary showing how many times each value, category, class, or interval appears in a dataset. Frequency tables are the foundation for understanding the overall structure and pattern of your data.

### 4.1 Core Formulas

**Frequency** ($f_i$) — count of observations in class $i$:

$$f_i = \text{count of observations in class } i$$

**Relative Frequency** ($r_i$) — the proportion of total observations:

$$r_i=\frac{f_i}{n}$$

**Percentage** — relative frequency expressed as a percentage:

$$p_i=\frac{f_i}{n}\times100$$

**Cumulative Frequency** ($F_i$) — running total of frequencies up to class $i$:

$$F_i=\sum_{j=1}^{i}f_j$$

### 4.2 Practical Applications

Frequency tables help detect:
- **Dominant classes or categories** — Which value appears most often?
- **Rare or infrequent categories** — Which values are underrepresented?
- **Class imbalance issues** — Is one category overwhelmingly dominant?
- **Grouped distributions** — How are values clustered in ranges?
- **Defect or failure counts** — What is the distribution of problems in quality control?

These tables form the foundation for visualizations like bar charts and histograms.

**Check for Understanding:**
- You have customer purchase amounts: $50, $50, $75, $100, $100, $100. Construct a frequency table with absolute frequency, relative frequency, and percentage. What insight does this reveal?

---

## 5. Distribution Shape: Skewness

**Skewness** measures the **asymmetry of a distribution**. A symmetric distribution has zero skewness; asymmetric distributions have non-zero skewness.

### 5.1 Mathematical Definition

**Population Skewness:**
$$\gamma_1=\frac{E[(X-\mu)^3]}{\sigma^3}$$

**Sample Skewness:**
$$g_1=\frac{\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^3}{\left(\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^2\right)^{3/2}}$$

### 5.2 Interpretation

- **Positive skewness (> 0):** Right-skewed — the tail extends to the right, pulling the mean higher than the median
- **Negative skewness (< 0):** Left-skewed — the tail extends to the left, pulling the mean lower than the median
- **Near zero:** Approximately symmetric distribution

### 5.3 Practical Importance

Understanding skewness is critical for:
- **Feature transformation** — Do we need to log-transform or box-cox transform variables before modeling?
- **Residual checks** — Are model errors normally distributed or skewed?
- **Rare-event dominance** — Is one category extremely dominant (e.g., fraud detection with 99.9% non-fraud)?
- **Synthetic data validation** — Does generated data match the real distribution shape?

**Example:** Income data is typically right-skewed (tail toward high earners), while test scores are often left-skewed (tail toward low performers).

---

## 6. Distribution Shape: Kurtosis

**Kurtosis** measures **tail heaviness and the propensity to generate extreme values**. It tells you whether your distribution has heavier or lighter tails than a normal distribution.

### 6.1 Mathematical Definition

**Population Kurtosis:**
$$\beta_2=\frac{E[(X-\mu)^4]}{\sigma^4}$$

**Excess Kurtosis** (most commonly used, normalized to 0 for normal distribution):
$$\text{Excess Kurtosis}=\beta_2-3$$

**Sample Excess Kurtosis:**
$$g_2=\frac{\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^4}{\left(\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^2\right)^2}-3$$

### 6.2 Interpretation

- **Leptokurtic** (excess kurtosis > 0): Heavy tails, more extreme values than normal distribution
- **Mesokurtic** (excess kurtosis ≈ 0): Normal-like tails (e.g., standard normal distribution)
- **Platykurtic** (excess kurtosis < 0): Lighter tails, fewer extremes than normal distribution

### 6.3 Why It Matters

Understanding kurtosis is essential for:
- **Anomaly detection** — Are we seeing more extreme outliers than expected?
- **Safety-critical engineering** — How often do worst-case scenarios actually occur?
- **Model residual analysis** — Are model errors behaving as theoretically expected?
- **Uncertainty quantification** — How wide should confidence intervals be to account for tail risk?
- **Rare-event simulation** — How extreme can outcomes become in the tails?

**Example:** Financial returns often exhibit positive kurtosis (leptokurtic), meaning extreme market crashes and rallies occur more frequently than a normal distribution would predict.

**Check for Understanding:**
- What would high kurtosis mean for a financial investment portfolio? Why would understanding this be important for risk management?

---

## Key Takeaways

1. **Central tendency** (mean, median, mode) describes the "typical" value. Choose based on data symmetry: use mean for normal data, median for skewed data.

2. **Dispersion** (variance, standard deviation, IQR) quantifies spread. Use standard deviation for normal data and IQR for skewed data or when outliers are present.

3. **Frequency tables** organize and summarize categorical and grouped numerical data, revealing class imbalance, rare categories, and dominant patterns.

4. **Percentiles and quartiles** partition data into meaningful segments, enabling outlier detection (IQR method) and distribution understanding.

5. **Skewness** reveals distributional asymmetry, guiding transformation decisions and helping diagnose when mean ≠ median.

6. **Kurtosis** indicates tail extremeness, critical for understanding rare events and assessing whether normal distribution assumptions hold.

---

## Connection to Later Modules

These foundational descriptive statistics form the **statistical language layer** for all subsequent modules in this curriculum. Master these concepts before advancing to exploratory data analysis (Lecture 2), inferential statistics, hypothesis testing, and modeling.
