# Experimental Design and Statistical Quality

Experimental design and statistical quality provide the framework for **planning experiments so that conclusions are valid, efficient, reproducible, and statistically defensible**.

Unlike data analysis (which happens after collection), experimental design asks: **How do we generate data correctly so that analysis is trustworthy?**

This module is essential for:
- Scientific experiments and hypothesis testing
- Machine learning benchmarking and algorithm comparison
- Engineering simulations and design optimization
- Quality assurance and process control
- Sensitivity studies and ablation analysis
- Controlled comparisons and fair evaluation
- Process monitoring and anomaly detection
- Reproducible research and publication standards

The main objective is to understand how to **design fair experiments, reduce bias, quantify uncertainty, and report results with statistical rigor**.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Design controlled experiments** with proper randomization, replication, and blocking
2. **Apply factorial and fractional factorial designs** to systematically explore factor combinations
3. **Use Response Surface Methodology** for optimization and sensitivity analysis
4. **Plan statistical power** to ensure adequate sample size for detecting real effects
5. **Implement quality control charts** for process monitoring
6. **Report experiments reproducibly** with complete methodology and uncertainty quantification
7. **Conduct ablation studies** in ML to identify component contributions

---

## 1. Foundations of Experimental Design

### 1.1 Key Terminology

- **Factor:** Controlled input variable (e.g., load level, temperature, learning rate)
- **Level:** Value assigned to a factor (e.g., 10 N, 25°C, 0.001)
- **Treatment:** Specific combination of factor levels
- **Response:** Measured outcome (Y)
- **Experimental unit:** Object or specimen being tested
- **Replication:** Repeating same treatment multiple times
- **Randomization:** Random order of experiments or treatment assignment
- **Blocking:** Grouping similar units to reduce variation

### 1.2 General Response Model

$$Y = f(X) + \epsilon$$

- **X:** Controlled factors
- **Y:** Measured response
- **ε:** Random experimental error/noise
- **Goal:** Isolate true factor effect from noise

**Check for Understanding:**
- In an optimization algorithm comparison, what are the factors, levels, treatments, and response?

---

## 2. Randomization

Randomization is the cornerstone of valid experimentation. Prevents confounding and hidden bias.

### 2.1 Why Randomize?

Without randomization, observed differences may arise from:
- **External influences:** Time of day, temperature drift, measurement decay
- **Operator bias:** Systematically favoring one treatment
- **Ordering effects:** First experiments perform differently than last
- **Learning effects:** Experimenter improves with practice
- **Correlated nuisance factors:** Systematic variation unrelated to treatment

### 2.2 Randomization in Practice

1. **Randomize run order:** Don't test treatment A in morning, B in afternoon
2. **Randomize assignment:** Don't allocate all good samples to one group
3. **Set random seed:** Document seed for reproducibility

---

## 3. Replication

Replication repeats treatments to:
- Estimate variability: $s^2 = \frac{1}{r-1}\sum(Y_i - \bar{Y})^2$
- Compute standard error: $SE = \frac{s}{\sqrt{r}}$
- Construct confidence intervals
- Increase statistical power

**More replication → lower SE → narrower CI → higher power to detect effects**

---

## 4. Blocking

Blocking controls for known nuisance factors by grouping similar units.

**Model:** $Y_{ij} = \mu + \tau_i + \beta_j + \epsilon_{ij}$
- τ_i: Treatment effect
- β_j: Block effect

**Example:** In FEM simulations with different material batches (nuisance factor), block by batch to isolate design effect.

---

## 5. Full Factorial Design

Evaluates all possible combinations of factor levels.

**Total runs:** $N = L^k$ (L levels, k factors)

**Two-level 3-factor example:** 2³ = 8 runs

**Advantages:**
- Captures main effects and interactions
- Reveals nonlinear patterns
- Complete understanding of factor space

**Disadvantages:**
- Expensive for many factors
- Combinatorial explosion

---

## 6. Fractional Factorial Design

Uses carefully selected subset of full factorial runs for cost reduction.

**Example:** 2^(k-1) uses half the runs of 2^k

**Trade-off:** Reduced cost vs. some interaction aliasing (confounding)

**When to use:** Screening studies, expensive experiments, large number of factors

---

## 7. Response Surface Methodology (RSM)

Systematically studies how response varies with factors using quadratic model:

$$Y = \beta_0 + \sum \beta_i x_i + \sum \beta_{ii}x_i^2 + \sum \beta_{ij}x_i x_j + \epsilon$$

**Applications:**
- Parameter tuning and optimization
- Sensitivity analysis
- Contour mapping of design space
- Finding optimal operating region

**Example:** Optimize hyperparameters (learning rate, batch size) using RSM on held-out validation performance.

---

## 8. Ablation and Sensitivity Studies

**Ablation:** Remove one component/factor at a time to measure its contribution.

**Sensitivity:** Vary factors systematically to rank importance.

**Applications:**
- ML model ablation: Drop each feature/layer; measure accuracy drop
- Algorithm analysis: Disable each component; measure performance
- Engineering: Identify critical design parameters

---

## 9. Statistical Power and Sample Size

**Power** = P(reject H₀ | H₀ is false) = 1 - β

**Determines:** How many replicates needed to detect effect size δ with significance α and power (1-β)

**Typical target:** Power = 0.80 (80% chance of detecting real effect)

**Main drivers:**
- Effect size: Larger δ requires fewer samples
- Variance σ²: Higher variance requires more samples
- Significance level α: Stricter α (0.01 vs 0.05) requires more samples

---

## 10. Quality Control Charts

Monitor process stability over time.

**Shewhart control limits:**
- UCL = μ + 3σ (upper control limit)
- LCL = μ - 3σ (lower control limit)

**Signals:**
- Point outside control limits
- Trend (increasing or decreasing)
- Cyclic pattern

**Applications:**
- Manufacturing process monitoring
- ML training stability (loss curves)
- Sensor drift detection
- Hardware temperature monitoring

---

## 11. Measurement System Quality

Measurement reliability ensures data validity.

**Key properties:**
- **Precision:** Repeatability of measurements
- **Accuracy:** Closeness to true value
- **Calibration:** Regular reference checks
- **Stability:** Consistent over time

**Impact:** Poor measurement invalidates even perfect designs

---

## 12. Reproducibility and Reporting

Complete methodology reporting:
- Sample/run size and replication
- Factor levels and experimental units
- Random seed and software versions
- Hardware specifications
- Confidence intervals and p-values
- Effect sizes
- Data preprocessing steps
- Code and data availability

**Enables:** Independent verification and meta-analyses

---

## Key Takeaways

1. **Randomization** prevents bias and confounding in treatment assignment and run order.

2. **Replication** enables estimation of variability and statistical inference (CI, SE, power).

3. **Blocking** reduces noise by accounting for known nuisance factors.

4. **Factorial designs** systematically explore factor combinations; full factorial complete, fractional efficient.

5. **RSM** optimizes responses through quadratic modeling and contour mapping.

6. **Ablation/sensitivity** ranks component importance; critical for AI system understanding.

7. **Power analysis** ensures sufficient sample size before experiment; prevents underpowered studies.

8. **Control charts** monitor process stability; detect drift and anomalies.

9. **Complete reporting** including seeds, versions, CIs enables reproducibility and external validation.

---

## Connection to Next Modules

Experimental design feeds into Lecture 13 (Bayesian Statistics) for sequential design optimization. Uncertainty quantification (Lecture 14) uses design insights for robust intervals. Finally, causal inference (Lecture 15) extends design principles to isolate causal effects.
