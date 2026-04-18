# Inferential Statistics

Inferential statistics is the branch of statistics used to **draw conclusions about a population based on information obtained from a sample**. It bridges the gap between observed data and broader truths.

Unlike descriptive statistics (Lecture 1), which only summarizes observed data, inferential statistics helps answer:

- What can this sample tell us about the full population?
- How uncertain is our estimate? What's the range of plausible values?
- Is the observed effect likely to be real or due to random chance?
- How large could the true population parameter be?
- Does one method statistically outperform another?

This foundation is critical for:
- Scientific reasoning and experimental validation
- Engineering hypothesis testing and reliability analysis
- Machine learning model validation and performance reporting
- Benchmark comparison studies (comparing algorithms, methods)
- Uncertainty quantification in predictions and estimates
- Publishing research with reviewer-level statistical rigor

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Distinguish** between population parameters and sample statistics, and explain the role of estimation
2. **Construct** confidence intervals for means, proportions, and other parameters to quantify uncertainty
3. **Interpret** sampling distributions and apply the Central Limit Theorem to justify inferential procedures
4. **Conduct** hypothesis tests following the formal framework (state hypotheses, compute test statistic, p-value decision)
5. **Evaluate** Type I and Type II errors, and understand the power of a statistical test
6. **Apply** inferential methods to benchmark comparisons, model validation, and engineering problems

---

## 1. Population, Sample, Parameters, and Statistics

Before conducting inference, we must clearly distinguish the key concepts.

### 1.1 Core Definitions

- **Population:** The complete set of all possible observations or units of interest. Example: All manufactured components, all daily temperatures for a location, all potential patients with a disease.

- **Sample:** A subset of the population actually observed or collected. Example: 500 randomly selected components, 365 days of temperature data, 100 patients in a clinical trial.

- **Parameter:** An unknown numerical characteristic of the population, typically denoted by Greek letters (μ, σ, p).

- **Statistic:** A numerical quantity computed from the sample, typically denoted by Latin letters ($\bar{x}$, $s$, $\hat{p}$).

### 1.2 Common Parameter and Statistic Examples

| Concept | Population Parameter | Sample Statistic | Symbol |
|---|---|---|---|
| **Center** | Population mean | Sample mean | $\mu$ vs. $\bar{x}$ |
| **Spread** | Population variance | Sample variance | $\sigma^2$ vs. $s^2$ |
| **Proportion** | Population proportion | Sample proportion | $p$ vs. $\hat{p}$ |

**Key Goal of Inference:** Use the sample statistic to estimate the unknown population parameter.

### 1.3 Sample Mean (Review)

The sample mean is an estimator of the population mean:

$$\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i$$

### 1.4 Sample Variance (Review)

The sample variance (with Bessel's correction) is an estimator of the population variance:

$$s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2$$

**Why divide by n−1, not n?** This adjustment (Bessel's correction) makes $s^2$ an unbiased estimator of $\sigma^2$—see Section 2.2.

**Check for Understanding:**
- You collect a sample of 50 manufacturing defect rates. Your sample mean is 2.3%. Does this exactly equal the population mean? Why or why not?

---

## 2. Parameter Estimation

Parameter estimation is the process of using sample data to estimate unknown population quantities. There are two major approaches.

### 2.1 Point Estimation

A **point estimate** is a single "best guess" numerical value for an unknown parameter.

#### Mean Estimation
$$\hat{\mu}=\bar{x}$$

The sample mean is the point estimate of the population mean.

#### Variance Estimation
$$\hat{\sigma}^2=s^2$$

The sample variance is the point estimate of the population variance.

#### Proportion Estimation
$$\hat{p}=\frac{x}{n}$$

where $x$ = count of successes, $n$ = sample size.

**Example:** If 45 out of 100 items pass quality inspection, then $\hat{p} = 0.45$.

### 2.2 Desired Properties of Estimators

An ideal estimator should have several desirable properties:

#### Unbiasedness
An estimator $\hat{\theta}$ is **unbiased** if, on average, it equals the true parameter:

$$E[\hat{\theta}] = \theta$$

**Intuition:** If you repeatedly sampled and estimated, the average of all estimates would equal the truth.

**Example:** The sample mean $\bar{x}$ is unbiased for $\mu$; however, the sample standard deviation $s$ is slightly biased for $\sigma$.

#### Consistency
An estimator is **consistent** if it converges to the true parameter as sample size increases:

$$\lim_{n \to \infty} \hat{\theta} \to \theta$$

**Intuition:** Larger samples yield better estimates.

#### Efficiency
An estimator is **efficient** if it has small variance relative to other unbiased estimators.

**Intuition:** The estimate doesn't bounce around wildly; it's stable.

#### Robustness
An estimator is **robust** if it performs well even when data violate assumptions (e.g., presence of outliers).

**Example:** The median is robust to outliers; the mean is not.

**Check for Understanding:**
- Why is $s^2$ (dividing by n−1) preferred over dividing by n for estimating population variance? Which property does this relate to?

### 2.3 Interval Estimation (Introduction)

While a point estimate gives a single number, an **interval estimate** provides a range of plausible values—accounting for sampling uncertainty. We'll explore this fully in Section 4.

---

## 3. Sampling Distributions

A **sampling distribution** is the probability distribution of a statistic when computed from repeated random samples of the same size from the same population. This is one of the most important concepts in inferential statistics.

### 3.1 The Key Insight

Even though the population is fixed, each sample yields a slightly different statistic. The distribution of those statistics across many samples is the sampling distribution.

**Example:** If you repeatedly sample 100 people from a city and compute the average height, you won't get exactly the same mean each time. The sampling distribution describes how those means vary.

### 3.2 Sampling Distribution of the Sample Mean

For a population with mean $\mu$ and variance $\sigma^2$, when drawing repeated samples of size $n$:

$$E[\bar{x}] = \mu$$

$$\text{Var}(\bar{x}) = \frac{\sigma^2}{n}$$

**Interpretation:**
- The expected value (long-run average) of $\bar{x}$ equals the population mean—$\bar{x}$ is an unbiased estimator.
- The variance of $\bar{x}$ decreases as sample size $n$ increases—larger samples yield more precise estimates.

### 3.3 Standard Error

The **standard error (SE)** is the standard deviation of the sampling distribution:

$$SE(\bar{x}) = \frac{\sigma}{\sqrt{n}}$$

**Interpretation:**
- Smaller SE = more precise estimation of the population mean
- SE decreases by a factor of $\sqrt{n}$, so doubling sample size reduces SE by $\sqrt{2} \approx 1.41$
- If $\sigma = 10$ and $n = 100$, then $SE = 10/\sqrt{100} = 1$

**Check for Understanding:**
- A population has $\sigma = 20$. For sample sizes $n = 25$, $n = 100$, and $n = 400$, compute the standard error of the mean. What pattern do you observe?

---

## 4. Central Limit Theorem (CLT)

The **Central Limit Theorem** is perhaps the most important principle in inferential statistics.

### 4.1 Statement

As sample size $n$ increases, the sampling distribution of the sample mean approaches a **normal (bell-shaped) distribution**, regardless of the shape of the population distribution.

$$\bar{x} \sim N\left(\mu, \frac{\sigma^2}{n}\right)$$

for sufficiently large $n$.

**Key insight:** Even if the original data are not normally distributed, their sample mean is approximately normal for large enough samples.

### 4.2 What "Sufficiently Large" Means

- If the population is already normally distributed: any $n$ works
- If the population is roughly symmetric: $n \geq 20$ is often sufficient
- If the population is highly skewed: $n \geq 30$ or $n \geq 50$ is recommended
- For very skewed or heavy-tailed distributions: larger $n$ (50+) may be needed

### 4.3 Why It Matters

The CLT enables:
- **Confidence intervals** (Section 5): We can use the normal distribution to create intervals around estimates
- **Hypothesis tests** (Section 6): We can compare sample statistics to hypothesized values using normal/t-distribution
- **z-tests and t-tests** (Sections 6.1–6.3): Formal procedures for testing claims
- **Asymptotic approximations:** Valid inference even when assumptions aren't perfectly met

**Example:** Manufacturing process produces components with unknown distribution. By CLT, the average of 50 components is approximately normally distributed, allowing us to create confidence intervals.

**Check for Understanding:**
- A population has a heavily right-skewed distribution. Why can you still construct a valid confidence interval for the mean using a normal distribution?

---

## 5. Confidence Intervals

A **confidence interval (CI)** provides a plausible range of values for an unknown population parameter. Instead of a single point estimate, a CI quantifies estimation uncertainty.

### 5.1 Concept

A confidence interval is an interval $[L, U]$ constructed from sample data such that a specified percentage (e.g., 95%) of such intervals, if repeatedly constructed, would contain the true parameter.

**Interpretation Example:** A 95% CI means that if you repeated sampling and interval construction 100 times, approximately 95 of those intervals would contain the true parameter.

**Important Caveat:** This does NOT mean the parameter has a 95% probability of being in this specific fixed interval. Once constructed, the interval either contains the parameter (probability 1) or doesn't (probability 0).

### 5.2 Confidence Interval for Mean (Known σ)

When the population standard deviation $\sigma$ is known:

$$CI = \bar{x} \pm z_{\alpha/2}\frac{\sigma}{\sqrt{n}}$$

where:
- $\bar{x}$ = sample mean
- $z_{\alpha/2}$ = critical z-value from standard normal distribution
- $\sigma$ = known population standard deviation
- $n$ = sample size

**Common Critical Values:**
| Confidence Level | $\alpha$ | $\alpha/2$ | $z_{\alpha/2}$ |
|---|---|---|---|
| 90% | 0.10 | 0.05 | 1.645 |
| 95% | 0.05 | 0.025 | 1.96 |
| 99% | 0.01 | 0.005 | 2.576 |

**Example:** Sample mean $\bar{x} = 100$, $\sigma = 15$, $n = 40$. The 95% CI is:
$$100 \pm 1.96 \times \frac{15}{\sqrt{40}} = 100 \pm 4.65 = [95.35, 104.65]$$

### 5.3 Confidence Interval for Mean (Unknown σ)

In practice, $\sigma$ is usually unknown. Use the **t-distribution** instead:

$$CI = \bar{x} \pm t_{\alpha/2, n-1}\frac{s}{\sqrt{n}}$$

where:
- $s$ = sample standard deviation
- $t_{\alpha/2, n-1}$ = critical t-value with $n-1$ degrees of freedom
- All other symbols as before

**Degrees of Freedom:** The parameter $n-1$ (not $n$) accounts for the fact that we estimated $\sigma$ from the sample.

**Why t instead of z?** The t-distribution has heavier tails, providing wider intervals to account for the extra uncertainty from estimating σ.

**When to Use t:** Nearly always when $\sigma$ is unknown and sample size is small or moderate ($n < 30$). For large samples, t ≈ z.

### 5.4 Confidence Interval for Proportion

For a binary outcome (success/failure), the confidence interval is:

$$CI = \hat{p} \pm z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

where:
- $\hat{p}$ = sample proportion ($x/n$)
- $n$ = sample size

**Practical Applications:**
- Classification accuracy (proportion of correct predictions)
- Defect rate or failure probability
- Success rate or yield in manufacturing

**Example:** 85 out of 100 items pass inspection ($\hat{p} = 0.85$). The 95% CI for the defect rate is:
$$0.85 \pm 1.96\sqrt{\frac{0.85 \times 0.15}{100}} = 0.85 \pm 0.070 = [0.780, 0.920]$$

**Check for Understanding:**
- A 95% CI for customer satisfaction is [0.72, 0.88]. What should you report to management? What does the width of this interval imply about sample size?

---

## 6. Margin of Error

The **margin of error (ME)** is the maximum expected distance between the point estimate and the true parameter. It equals the ± term in a confidence interval formula.

### 6.1 Margin of Error for Mean

$$ME = z_{\alpha/2}\frac{\sigma}{\sqrt{n}} \quad \text{(known σ)}$$

or

$$ME = t_{\alpha/2, n-1}\frac{s}{\sqrt{n}} \quad \text{(unknown σ)}$$

### 6.2 Margin of Error for Proportion

$$ME = z_{\alpha/2}\sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

### 6.3 Key Insight: Factors Affecting ME

The margin of error decreases when:
- **Sample size increases** ($\sqrt{n}$ in denominator): larger samples yield more precise estimates
- **Confidence level decreases:** fewer confident (narrower) interval vs. more confident (wider) interval
- **Data variability decreases:** less spread within the population means more precision

**Design Implication:** To halve the ME, you need to **quadruple the sample size** (because ME ∝ 1/√n).

---

## 7. Hypothesis Testing Framework

Hypothesis testing is the formal procedure for evaluating claims about population parameters. It's one of the most widely used inferential tools in science and engineering.

### 7.1 The Basic Idea

You have a claim or theory about a population. You collect sample data and ask: "Is the observed evidence strong enough to reject the assumption that this claim is false?"

### 7.2 The Formal Framework

**Step 1: State the Hypotheses**

- $H_0$ (Null Hypothesis): A statement of "no effect" or status quo. Example: $H_0: \mu = 100$ (the mean equals 100)
- $H_1$ (Alternative Hypothesis): The claim you're trying to gather evidence for. Examples:
  - Two-sided: $H_1: \mu \neq 100$ (the mean differs from 100)
  - One-sided (upper): $H_1: \mu > 100$ (the mean exceeds 100)
  - One-sided (lower): $H_1: \mu < 100$ (the mean is below 100)

**Step 2: Select the Test Statistic**

Choose a statistic that measures the distance between the sample and the hypothesized value. Sections 7.3–7.5 cover specific test statistics.

**Step 3: Compute the p-value**

The p-value is the probability of observing data at least as extreme as the current sample, **assuming $H_0$ is true**. Smaller p-values indicate stronger evidence against $H_0$.

**Step 4: Make a Decision**

Choose a significance level $\alpha$ (typically 0.05), and:
- If $p < \alpha$: Reject $H_0$ (conclude the evidence supports $H_1$)
- If $p \geq \alpha$: Fail to reject $H_0$ (insufficient evidence for $H_1$)

---

## 8. One-Sample Tests

### 8.1 One-Sample z-Test (Known σ)

Use when the population standard deviation is known:

$$z = \frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}$$

where:
- $\bar{x}$ = sample mean
- $\mu_0$ = hypothesized population mean (from $H_0$)
- $\sigma$ = known population standard deviation
- $n$ = sample size

**Interpretation:** $z$ measures how many standard errors the sample mean is from the hypothesized value.

### 8.2 One-Sample t-Test (Unknown σ)

More common in practice, when $\sigma$ is estimated from the sample:

$$t = \frac{\bar{x}-\mu_0}{s/\sqrt{n}}$$

where $s$ is the sample standard deviation and the test statistic follows a t-distribution with $n-1$ degrees of freedom.

**Assumptions:**
- Data are roughly normally distributed (or large sample size, by CLT)
- Observations are independent
- No extreme outliers

**Check for Understanding:**
- A manufacturing process is designed to produce components with mean weight 500g. You collect 25 components and find $\bar{x} = 495$g, $s = 8$g. Should you conclude the process is off-target? (Conduct a test with $\alpha = 0.05$.)

### 8.3 Two-Sample t-Test (Comparing Two Means)

To compare the means of two independent groups:

$$t = \frac{\bar{x}_1-\bar{x}_2}{\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}}$$

**Degrees of Freedom:** Approximately $n_1 + n_2 - 2$ (more precisely, given by Welch's approximation).

**Practical Applications:**
- Comparing algorithm A vs. algorithm B (benchmark comparison)
- Control vs. treatment group in experiments
- Method 1 vs. Method 2 in engineering

**Important:** This tests whether the means differ, not whether the methods are practically equivalent.

---

## 9. Statistical Significance

### 9.1 p-Value

The **p-value** is the probability of observing a test statistic at least as extreme as the one computed, **assuming the null hypothesis is true**.

**Interpretation:**
- $p$ close to 0: Very unlikely to observe this data if $H_0$ were true → strong evidence against $H_0$
- $p$ close to 1: Likely to observe this data if $H_0$ were true → weak evidence against $H_0$

**Decision Rule:**
$$\text{If } p < \alpha, \text{ reject } H_0; \text{ otherwise, fail to reject}$$

**Typical Significance Levels:**
- $\alpha = 0.10$: exploratory analysis, preliminary evidence
- $\alpha = 0.05$: standard in most fields (journals, engineering)
- $\alpha = 0.01$: high bar for evidence (safety-critical applications)

### 9.2 Important Caution: Statistical vs. Practical Significance

**Statistical significance** (small p-value) does NOT necessarily imply:
- **Practical importance:** An effect can be statistically significant but practically negligible. Example: A drug reduces symptoms by 0.1% with $p = 0.02$ (statistically significant but practically irrelevant).
- **Engineering relevance:** A statistically significant difference may not meet engineering specifications.
- **Causal validity:** Significance says an effect exists in *this* sample, not why it exists or if causation is involved.
- **Robustness:** Results may not generalize if assumptions are violated or in different contexts.

**Best Practice:** Always report:
1. **p-value** (statistical significance)
2. **Confidence interval** (magnitude and uncertainty of effect)
3. **Effect size** (practical importance: how large is the difference?)
4. **Domain interpretation** (does this make sense in context?)

**Check for Understanding:**
- A study finds a statistically significant difference (p = 0.03) between two algorithms, but the confidence interval for the difference in accuracy is [0.2%, 0.5%]. Is this practically important? Why?

---

## 10. Type I and Type II Errors

Every hypothesis test carries two types of error risk.

### 10.1 Type I Error (False Positive)

**Definition:** Rejecting the null hypothesis when it is actually true.

**Probability:** $\alpha$ (the significance level)

**Interpretation:** You conclude an effect exists when it actually doesn't.

**Example:** A drug is approved as safe when it actually carries risks.

### 10.2 Type II Error (False Negative)

**Definition:** Failing to reject the null hypothesis when it is actually false.

**Probability:** $\beta$

**Interpretation:** You fail to detect an effect that truly exists.

**Example:** A flawed component design is declared safe when it's actually risky.

### 10.3 Statistical Power

**Power** is the probability of correctly rejecting $H_0$ when it is false (detecting a true effect):

$$\text{Power} = 1 - \beta$$

**Desired Practice:**
- High power (typically $\geq 0.80$) is preferred in research
- Large samples increase power
- Larger true effects are easier to detect (higher power)
- Trade-off: Type I and Type II errors relate inversely; lowering $\alpha$ increases $\beta$

### 10.4 Error Decision Table

| Decision | $H_0$ True | $H_0$ False |
|---|---|---|
| **Reject $H_0$** | Type I Error (prob. = $\alpha$) | Correct (prob. = Power = $1 - \beta$) |
| **Fail to Reject** | Correct (prob. = $1 - \alpha$) | Type II Error (prob. = $\beta$) |

**Check for Understanding:**
- In a safety-critical application (aircraft design), would you prefer to set $\alpha = 0.01$ or $\alpha = 0.10$? Why? What's the trade-off?

---

## 11. Practical Applications in AI and Engineering

### 11.1 Common Use Cases

Inferential statistics is essential for:
- **Algorithm benchmark comparison:** Does Method A outperform Method B?
- **ML model performance validation:** Is the reported accuracy reliable?
- **Performance metric confidence intervals:** Report RMSE with uncertainty bands
- **Optimization indicator comparison:** Does the new optimizer converge faster?
- **Defect rate estimation:** What's the failure rate with confidence bounds?
- **Structural reliability inference:** How often will a component fail?
- **GAN synthetic data validation:** Does synthetic data match real distributions?
- **Reviewer-level statistical rigor:** Publishing papers with proper statistical testing

### 11.2 Recommended Inferential Practices

For your workflow, prioritize:

1. **Confidence intervals for performance curves:** Always report uncertainty, not just point estimates
2. **t-tests for model comparisons:** Formally test if differences are significant
3. **Parameter estimation with uncertainty:** Quantify what estimates mean
4. **Standard error reporting:** Communicate precision of estimates
5. **Significance testing with context:** p-values + effect sizes + domain judgment

---

## Key Takeaways

1. **Estimation** bridges samples to populations: use point estimates for single values, confidence intervals for uncertainty ranges.

2. **Sampling distributions** describe statistic behavior across repeated samples; understanding them justifies inferential procedures.

3. **Central Limit Theorem** ensures that sample means are approximately normal for large samples, enabling confidence intervals and hypothesis tests.

4. **Confidence intervals** quantify estimation uncertainty; always report them alongside point estimates.

5. **Hypothesis testing** formally evaluates claims; follow the framework (state hypotheses, compute test statistic, compare p-value to $\alpha$).

6. **Statistical significance** (small p-value) must be paired with effect size and domain context; avoid mistaking statistical significance for practical importance.

7. **Type I and Type II errors** carry different costs; design experiments with appropriate power and significance levels.

---

## Connection to Next Modules

The tools introduced here—confidence intervals, hypothesis tests, p-values—form the backbone for Lecture 4 (Hypothesis Testing), where you'll explore specific test procedures (t-tests, ANOVA, correlation tests) and Lecture 5 (Correlation Analysis), where you'll assess relationships between variables with proper statistical inference.
