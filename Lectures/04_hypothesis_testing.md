# Hypothesis Testing

Hypothesis testing is the formal statistical framework used to **evaluate claims about population parameters, compare methods, and determine whether observed differences are likely due to real effects or random sampling variation**.

This methodology is central to:
- Scientific research and experimental validation
- Engineering experiments and quality control
- Machine learning benchmark comparisons and model validation
- A/B testing in industry and optimization
- Algorithm comparison and performance benchmarking
- Reviewer-level statistical validation for publications

This module is organized into **two major parts**:
- **Part I — Parametric hypothesis tests:** Assume normally distributed data; test means
- **Part II — Nonparametric hypothesis tests:** Don't assume normality; based on ranks or counts

Both are essential for proper statistical inference.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Select and execute** the appropriate hypothesis test based on data type, distribution, and research question
2. **Interpret** test results including p-values, test statistics, and decisions to reject or fail to reject null hypotheses
3. **Compare two groups** using both parametric (t-test) and nonparametric (Mann-Whitney, Wilcoxon) methods
4. **Compare three or more groups** using ANOVA (parametric) and Kruskal-Wallis or Friedman (nonparametric) tests
5. **Assess categorical associations** using chi-square tests
6. **Choose appropriately** between parametric and nonparametric tests based on assumption violations

---

## Review: Hypothesis Testing Framework

(See Lecture 3, Section 7 for detailed introduction to hypothesis testing concepts.)

### Core Definitions

**Null Hypothesis** ($H_0$): The default assumption, typically representing:
- No difference between groups
- No effect of a treatment
- Equality of parameters
- The "status quo" claim

**Alternative Hypothesis** ($H_1$ or $H_a$): The competing claim representing:
- A difference exists
- An effect is present
- Superiority or association
- What you're gathering evidence for

### The General Framework

**Step 1 — State Hypotheses:** Write $H_0$ and $H_1$ in mathematical form

**Step 2 — Select a Test:** Choose the appropriate test based on data characteristics

**Step 3 — Compute Test Statistic:** Calculate a value that measures distance from $H_0$

**Step 4 — Compute p-value:** Determine the probability of observing data this extreme under $H_0$

**Step 5 — Make Decision:** Compare p-value to significance level $\alpha$:
- If $p < \alpha$: Reject $H_0$ (evidence supports $H_1$)
- If $p \geq \alpha$: Fail to reject $H_0$ (insufficient evidence)

### Significance Levels

- $\alpha = 0.10$: Exploratory analysis, preliminary evidence
- $\alpha = 0.05$: Standard in most scientific and engineering fields
- $\alpha = 0.01$: High bar for evidence; safety-critical applications

---

# Part I — Parametric Hypothesis Tests

Parametric tests assume specific distributional structures, most commonly **normality**, and typically focus on comparing means. Use these when:
- Data are approximately normally distributed
- Sample sizes are moderate to large (for normality robustness via CLT)
- You want maximum power to detect real effects

---

## 1. One-Sample Tests

### 1.1 z-Test

The **z-test** evaluates claims about a population mean when the population standard deviation is known (or sample size is large).

**Hypotheses:**
$$H_0: \mu = \mu_0 \quad \text{(two-sided)}$$

$$H_1: \mu \neq \mu_0$$

Alternatively, one-sided: $H_1: \mu > \mu_0$ or $H_1: \mu < \mu_0$

**Test Statistic:**
$$z = \frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}$$

where:
- $\bar{x}$ = sample mean
- $\mu_0$ = hypothesized population mean (from $H_0$)
- $\sigma$ = known population standard deviation
- $n$ = sample size

**Interpretation:** $z$ measures how many standard errors the sample mean deviates from the hypothesized value.

**When to Use:**
- Large sample sizes ($n > 30$)
- Population variance is known (rare in practice)
- Process control and quality monitoring
- Benchmarking against a known specification

**Example:** A manufacturing process is designed to produce parts with mean weight 500g and known $\sigma = 10$g. Test whether a sample of $n = 50$ parts with $\bar{x} = 502$g indicates the process has drifted.

**Check for Understanding:**
- If you observe $\bar{x} = 505$g in the example above, will the z-statistic be larger or smaller? Why?

---

### 1.2 One-Sample t-Test

The **one-sample t-test** is used when the population variance is unknown and must be estimated from the sample. This is more common in practice than the z-test.

**Hypotheses:**
$$H_0: \mu = \mu_0$$
$$H_1: \mu \neq \mu_0 \quad \text{(or one-sided)}$$

**Test Statistic:**
$$t = \frac{\bar{x}-\mu_0}{s/\sqrt{n}}$$

where:
- $s$ = sample standard deviation
- $n$ = sample size

**Degrees of Freedom:** $df = n-1$

The test statistic follows a t-distribution with $n-1$ degrees of freedom.

**Assumptions:**
- Observations are independent
- Data are approximately normally distributed (or large sample)
- Interval/ratio scale measurement

**When to Use:**
- Unknown population variance (typical scenario)
- Moderate sample sizes ($n > 10$, ideally $n \geq 20$)
- Testing against a fixed benchmark value

**Why This Is Important:** This is one of the most widely used tests in science and engineering. It's the foundation for many other procedures.

**Example:** An educational program claims students improve by 5 points. You test 25 students and observe mean improvement $\bar{x} = 6.5$ points with $s = 2$ points. Test whether the claimed improvement is met.

**Check for Understanding:**
- Why does the t-distribution have heavier tails than the normal distribution? What does this mean for confidence intervals?

---

## 2. Two-Sample Tests (Comparing Two Groups)

### 2.1 Independent Samples t-Test

The **independent t-test** compares means of two **independent groups**. Use when:
- You have two separate groups
- Data from one group don't influence the other

**Hypotheses:**
$$H_0: \mu_1 = \mu_2 \quad \text{(means are equal)}$$
$$H_1: \mu_1 \neq \mu_2 \quad \text{(means differ)}$$

**Welch's t-Statistic** (preferred, doesn't assume equal variances):
$$t = \frac{\bar{x}_1-\bar{x}_2}{\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}}$$

**Degrees of Freedom:** Approximately $n_1 + n_2 - 2$ (Welch's approximation gives more precise values)

**Assumptions:**
- Groups are independent
- Data are approximately normally distributed
- Similar variances preferred (Welch's t-test is robust to unequal variances)

**Practical Applications:**
- Comparing two machine learning models on the same test set
- Testing if two materials have different properties
- Comparing two optimization algorithms' convergence speeds
- Control vs. treatment group in experiments

**Example:** You compare Algorithm A (mean RMSE = 0.85, $s = 0.12$, $n = 20$ runs) vs. Algorithm B (mean RMSE = 0.92, $s = 0.15$, $n = 20$ runs). Does Algorithm A significantly outperform Algorithm B?

**Check for Understanding:**
- In the example above, which test would you use: one-sample t or independent t? Why?

---

### 2.2 Paired t-Test

The **paired t-test** compares means when observations come in **matched pairs**. This is powerful because it controls for pair-level variation.

**Common Scenarios:**
- Before vs. after measurements on the same subjects
- Same problem instances tested under two different algorithms
- Repeated benchmark runs with paired random seeds

**Procedure:**

1. Compute the paired difference for each pair: $d_i = x_i - y_i$
2. Treat the differences as a single sample
3. Apply one-sample t-test to the differences

**Test Statistic:**
$$t = \frac{\bar{d}}{s_d/\sqrt{n}}$$

where:
- $\bar{d}$ = mean of paired differences
- $s_d$ = standard deviation of differences
- $n$ = number of pairs

**Degrees of Freedom:** $df = n - 1$ (number of pairs minus 1)

**Why It's Powerful:** By using within-pair differences, paired testing controls for subject-level variation, increasing statistical power.

**Example:** You test 15 algorithms on the same 15 optimization problems. For each problem, you compute the difference in performance (Algorithm A − Algorithm B). A paired t-test determines if A significantly outperforms B across problems.

**Connection to Nonparametric Methods:** Section 3.2 below covers the Wilcoxon signed-rank test, the nonparametric alternative when normality assumptions fail.

**Check for Understanding:**
- Why is the paired t-test more powerful than an independent t-test for comparing the same algorithm runs under two conditions?

---

## 3. Multiple Group Tests (3+ Groups)

### 3.1 One-Way ANOVA

**ANOVA (Analysis of Variance)** compares means of 3 or more **independent groups**. Instead of conducting many t-tests (which would inflate Type I error), ANOVA performs one global test.

**Hypotheses:**
$$H_0: \mu_1=\mu_2=\cdots=\mu_k \quad \text{(all means equal)}$$
$$H_1: \text{At least one mean differs}$$

**Core Idea:** Compare:
- **Between-group variability** (differences in group means)
- **Within-group variability** (spread within each group)

If between-group variability is large relative to within-group variability, groups likely differ.

**F Statistic:**
$$F = \frac{MS_{\text{between}}}{MS_{\text{within}}}$$

where:
$$MS_{\text{between}}=\frac{SS_{\text{between}}}{df_{\text{between}}} = \frac{\text{between-group sum of squares}}{\text{between-group degrees of freedom}}$$

$$MS_{\text{within}}=\frac{SS_{\text{within}}}{df_{\text{within}}} = \frac{\text{within-group sum of squares}}{\text{within-group degrees of freedom}}$$

**Sum of Squares:**

**Between Groups:**
$$SS_{\text{between}} = \sum_{j=1}^{k} n_j(\bar{x}_j-\bar{x}_{\text{grand}})^2$$

where $\bar{x}_{\text{grand}}$ is the overall mean across all groups.

**Within Groups:**
$$SS_{\text{within}} = \sum_{j=1}^{k}\sum_{i=1}^{n_j}(x_{ij}-\bar{x}_j)^2$$

**Interpretation:** Large $F$ indicates strong between-group differences relative to within-group variation.

**Assumptions:**
- Observations within groups are independent
- Data are approximately normally distributed (robust for large samples via CLT)
- Homogeneity of variance: variances across groups are roughly equal

**Post-Hoc Tests:** If ANOVA rejects $H_0$, you know at least one group differs, but not which. Use post-hoc tests (Tukey HSD, Bonferroni) to identify specific differences.

**Practical Application:** Compare mean performance of 5 different optimization algorithms across a benchmark suite.

---

### 3.2 Repeated Measures ANOVA

Used when **the same subjects (or problem instances) are measured under multiple conditions**. This controls for subject-level variability.

**Examples:**
- Same ML model trained with different hyperparameter settings
- Same structural cases tested under multiple analysis methods
- Same person measured at multiple time points

**Model Concept:**
$$Y_{ij}=\mu + \tau_j + s_i + \epsilon_{ij}$$

where:
- $\mu$ = overall mean
- $\tau_j$ = effect of treatment/condition j
- $s_i$ = effect of subject i (accounts for subject-level variability)
- $\epsilon_{ij}$ = random error

By accounting for subject effects, repeated measures ANOVA controls variability and increases power.

**Why Important:** This is the parametric counterpart to the Friedman test (covered in Section 4.2 of nonparametric tests). Ideal for optimization and algorithm comparison papers.

---

# Part II — Nonparametric Hypothesis Tests

Nonparametric tests are essential when:
- **Normality assumption is violated:** Data are skewed or have heavy tails
- **Sample size is very small:** CLT doesn't apply
- **Data are ordinal or ranked:** Natural ordering, not interval-scale
- **Robustness is preferred:** Want protection against outliers and assumption violations

These are **extremely important in optimization and machine learning benchmarking** where data may violate normality.

---

## 4. Two-Sample Nonparametric Tests

### 4.1 Mann-Whitney U Test

The **Mann-Whitney U test** (also called Wilcoxon rank-sum test) is the nonparametric alternative to the independent t-test.

**Use When:**
- Comparing two independent groups
- Normality assumption is violated
- Sample sizes are small
- Data are ordinal or contain outliers

**Hypotheses:**
$$H_0: \text{Distributions of the two groups are equal}$$
$$H_1: \text{Distributions differ}$$

**Procedure:**
1. Rank all observations from both groups together (lowest = 1, highest = n)
2. Sum the ranks in group 1: $R_1$
3. Compute U statistic

**U Statistic:**
$$U_1=n_1n_2+\frac{n_1(n_1+1)}{2}-R_1$$

**Interpretation:** If groups are identical, rank sums should be similar; if $U$ is small, one group tends to have smaller values.

**Advantage over t-test:** Doesn't assume normality; robust to outliers and skewed distributions.

**Practical Application:** Compare two optimizers over independent benchmark runs without assuming normal distributions.

---

### 4.2 Wilcoxon Signed-Rank Test

The **Wilcoxon signed-rank test** is the nonparametric alternative to the paired t-test. This is one of the **most important tests in optimization and algorithm comparison papers**.

**Use When:**
- Comparing two paired/matched groups
- Normality assumption is violated
- Small samples
- Robust, rank-based comparison preferred

**Hypotheses:**
$$H_0: \text{Paired differences come from a symmetric distribution centered at 0}$$
$$H_1: \text{Differences are not centered at 0}$$

**Procedure:**
1. Compute paired differences: $d_i = x_i - y_i$
2. Rank the **absolute values** of non-zero differences
3. Restore signs to the ranks: $W^+$ (sum of positive-signed ranks), $W^-$ (sum of negative-signed ranks)
4. Test statistic: $W = \min(W^+, W^-)$

**Why Important for Your Work:**
- Perfect for comparing two algorithms on the same problem set
- Avoids assumptions about distributional shape
- Preferred by many reviewers for optimization papers
- Accounts for magnitude (unlike sign test) while maintaining robustness

**Example:** Test 20 optimization problems with Algorithm A vs. Algorithm B. For each problem, compute the performance difference. Wilcoxon test determines if A significantly outperforms B.

**Advantage:** Combines robustness (rank-based) with power (uses magnitude information).

---

## 5. Multiple Group Nonparametric Tests

### 5.1 Kruskal-Wallis Test

The **Kruskal-Wallis test** is the nonparametric alternative to one-way ANOVA. Use for comparing 3+ independent groups.

**Hypotheses:**
$$H_0: \text{All group distributions are identical}$$
$$H_1: \text{At least one distribution differs}$$

**Test Statistic:**
$$H = \frac{12}{N(N+1)}\sum_{j=1}^{k}\frac{R_j^2}{n_j}-3(N+1)$$

where:
- $R_j$ = sum of ranks in group j
- $n_j$ = size of group j
- $N$ = total observations
- $k$ = number of groups

**Interpretation:** Rank all observations together; if groups have similar distributions, rank sums should be similar.

**When to Use:**
- 3 or more independent groups
- Normality assumption violated
- Ordinal data
- Robustness desired

**Practical Application:** Compare multiple ML models or optimization algorithms when normality doesn't hold.

---

### 5.2 Friedman Test

The **Friedman test** is the nonparametric alternative to repeated measures ANOVA. This is one of the **most important benchmark comparison tests in AI and optimization research** and is widely expected by reviewers.

**Use When:**
- Comparing 3+ algorithms on the same benchmark problems
- Same problem instances, multiple methods (matched groups)
- Normality assumption violated
- Reviewer-standard test expected

**Hypotheses:**
$$H_0: \text{All methods have equal performance distributions}$$
$$H_1: \text{Performance distributions differ}$$

**Procedure:**
1. For each problem, rank the k algorithms (1 = best, k = worst)
2. Sum the ranks for each algorithm: $R_1, R_2, \ldots, R_k$
3. Compute the Friedman statistic

**Friedman Statistic:**
$$Q = \frac{12N}{k(k+1)}\sum_{j=1}^{k}\bar{R}_j^2 - 3N(k+1)$$

where:
- $N$ = number of problems/datasets
- $k$ = number of methods
- $\bar{R}_j$ = average rank of method j

**Why This Is Important:**
- **Reviewer standard:** Expected in optimization and ML papers comparing multiple methods
- **Principled:** Avoids assumptions while properly accounting for multiple comparisons
- **Aligned with your research:** Perfect for algorithm benchmarking across problem sets
- **Nonparametric:** Doesn't assume normality or specific distributions

**Post-Hoc Tests:** If Friedman rejects $H_0$, use post-hoc tests (e.g., Nemenyi test) to identify which pairs of methods significantly differ.

**Example:** Compare 5 optimization algorithms on 30 benchmark problems. Rank each algorithm on each problem (1-5), sum ranks, and conduct Friedman test.

---

## 6. Categorical Tests

### 6.1 Chi-Square Test

The **chi-square test** evaluates associations in categorical data. It has two main applications.

#### Goodness-of-Fit Test

Tests whether observed frequencies match expected frequencies under a hypothesized distribution.

**Hypotheses:**
$$H_0: \text{Data follow the hypothesized distribution}$$
$$H_1: \text{Data do not fit the hypothesized distribution}$$

**Test Statistic:**
$$\chi^2 = \sum_{i} \frac{(O_i-E_i)^2}{E_i}$$

where:
- $O_i$ = observed frequency in category i
- $E_i$ = expected frequency under $H_0$

**Example:** Test whether die rolls follow a uniform distribution (each number equally likely).

#### Independence Test

Tests whether two categorical variables are independent or associated.

**Hypotheses:**
$$H_0: \text{Variables A and B are independent}$$
$$H_1: \text{Variables A and B are associated}$$

**Contingency Table:** Rows = categories of A, Columns = categories of B. Each cell shows count.

**Expected Count Under Independence:**
$$E_{ij}=\frac{(\text{row total} \times \text{column total})}{\text{grand total}}$$

**Test Statistic:**
$$\chi^2 = \sum_{i=1}^{r}\sum_{j=1}^{c}\frac{(O_{ij}-E_{ij})^2}{E_{ij}}$$

**Practical Applications:**
- Defect type vs. severity in quality control
- Predicted class vs. actual class (confusion matrix analysis)
- Product color vs. pass/fail rate
- Category association in categorical data

**Check for Understanding:**
- You observe that 95% of red products pass QC while 40% of blue products pass. Is this association statistically significant? How would chi-square help?

---

## 7. Choosing the Right Test: Decision Guide

### Parametric Tests

| Situation | Test | Assumptions |
|---|---|---|
| One mean vs. target, known σ | z-test | Known variance, large n |
| One mean vs. target, unknown σ | One-sample t | Normal (or large n), independent |
| Two independent groups | Independent t-test | Normal, independent, similar variances |
| Two paired groups | Paired t-test | Normal differences, dependent pairs |
| 3+ independent groups | One-way ANOVA | Normal, independent, equal variances |
| 3+ matched groups | Repeated measures ANOVA | Normal, dependent groups |

### Nonparametric Tests

| Situation | Test | Use When |
|---|---|---|
| Two independent groups | Mann-Whitney U | Normality violated, small n, robustness |
| Two paired groups | Wilcoxon signed-rank | Normality violated, robust needed |
| 3+ independent groups | Kruskal-Wallis | Normality violated, multiple groups |
| 3+ matched groups | Friedman test | Normality violated, multiple algorithms |
| Paired, sign-only | Sign test | Very small samples, sign only |
| Categorical frequencies | Chi-square | Categorical variables, association/fit |

### Decision Flowchart (Simplified)

1. **How many groups?** 1 → One-sample. 2 → Two-sample. 3+ → Multi-group.
2. **Are samples independent or paired?** → Selects between independent/paired tests.
3. **Is data normal?** Yes → Parametric. No → Nonparametric.
4. **Categorical?** Yes → Chi-square.

---

## 8. Practical Applications in AI and Engineering

### Highest-Value Tests for Your Workflow

1. **Wilcoxon signed-rank test** — Comparing two algorithms on the same problems
2. **Friedman test** — Comparing multiple algorithms on multiple problems (reviewer standard)
3. **Paired t-test** — Rapid comparison of two methods when normality is reasonable
4. **Repeated measures ANOVA** — Parametric alternative to Friedman
5. **Kruskal-Wallis test** — Comparing multiple independent conditions
6. **Chi-square for categorical defect analysis** — Quality control and classification

### Applications

These tests directly support:
- Algorithm and optimizer comparisons
- GAN benchmarking and evaluation
- Machine learning model validation papers
- Structural engineering experiment design
- Reviewer responses with statistical rigor
- A/B testing and online experimentation
- Confidence in published claims

---

## Key Takeaways

1. **Parametric tests** (t-test, ANOVA) assume normality and are most powerful when assumptions hold; use for well-behaved data.

2. **Nonparametric tests** (Mann-Whitney, Wilcoxon, Friedman) avoid distributional assumptions; use when normality fails or for robustness.

3. **Paired tests** (paired t, Wilcoxon) are more powerful than independent tests by controlling within-pair variation.

4. **Multiple group tests** (ANOVA, Kruskal-Wallis, Friedman) enable one global test instead of multiple pairwise comparisons, controlling Type I error.

5. **Friedman test** is the reviewer-expected standard for algorithm comparison across multiple problem instances.

6. **Categorical tests** (chi-square) assess association and goodness-of-fit for categorical variables.

7. **Always pair statistical testing with practical significance:** p-value < 0.05 means statistical significance, but report effect sizes and confidence intervals to assess practical importance.

---

## Connection to Next Modules

The hypothesis testing framework introduced here—choosing tests, interpreting p-values, understanding power—directly applies to Lecture 5 (Correlation Analysis), where you'll test whether correlations between variables are statistically significant, and Lecture 6 (Regression Analysis), where you'll test whether regression coefficients significantly differ from zero.
