# Resampling and Validation Statistics

Resampling and validation statistics provide the framework for **estimating model generalization, uncertainty, robustness, and statistical stability from limited data**.

These methods repeatedly reuse available samples to answer critical questions:

- **Will the model generalize to unseen data?** Test generalization without new data
- **How stable are estimated metrics?** Quantify metric uncertainty
- **How sensitive are results to data splits?** Assess split-dependency
- **What is the uncertainty of performance estimates?** Compute confidence intervals
- **Is the model overfitting?** Compare train vs validation performance
- **How does hyperparameter choice affect results?** Unbiased model selection

This module is foundational for:
- Machine learning model validation and comparison
- Surrogate model assessment and uncertainty
- Uncertainty-aware AI systems
- Scientific reproducibility in publications
- Benchmark robustness and stability
- Limited-data engineering workflows
- Model selection and hyperparameter optimization
- Statistical confidence in predictive pipelines

For your AI + engineering + surrogate modeling workflows, this is one of the most practical modules.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Explain** why resampling is necessary and what it estimates (generalization error)
2. **Implement** train-validation-test splits appropriately for model development
3. **Conduct k-fold cross-validation** and interpret its results
4. **Use bootstrap resampling** to estimate uncertainty and construct confidence intervals
5. **Apply nested cross-validation** for unbiased hyperparameter tuning and evaluation
6. **Design permutation tests** to validate whether model performance is statistically significant
7. **Report metrics with uncertainty** (means, standard deviations, confidence intervals)

---

## 1. Foundations of Resampling

### 1.1 The Generalization Problem

Given a dataset:
$$D = \{(x_i,y_i)\}_{i=1}^{n}$$

The core question is: **How well does the trained model perform on unseen data?**

**Cannot answer by:**
- Testing on training data (biased; model has memorized training set)
- Using labeled test data from original experiment (only approximates generalization)

**Solution:** Resampling methods repeatedly partition data to estimate:
$$\mathbb{E}[L(f,D_{new})]$$

where L is loss/error metric, f is trained model, D_new is hypothetical unseen data.

### 1.2 Data Partitioning Strategy

Data is split into three disjoint sets:
- **Training set:** Learn model parameters
- **Validation set:** Tune hyperparameters, detect overfitting
- **Test set:** Final unbiased performance evaluation

**Typical splits:**
- 70% train / 15% validation / 15% test
- 80% train / 10% validation / 10% test  
- 60% train / 20% validation / 20% test

**Critical rule:** Test set is used only once, at the very end, after all development is complete.

**Check for Understanding:**
- Why is it wrong to tune hyperparameters using test set performance?

---

## 2. Holdout Validation

The simplest validation method: single train-test split.

### 2.1 Procedure

1. **Randomly split** data into training and test portions
2. **Train model** on training portion
3. **Evaluate** on held-out test portion

**Error estimate:**
$$\hat{E}_{holdout}=\frac{1}{n_{test}}\sum_{i \in \text{test}}L(y_i,\hat{y}_i)$$

### 2.2 Advantages and Disadvantages

**Advantages:**
- Simple and fast
- Low computational cost
- Clear train-test separation

**Disadvantages:**
- **Split-sensitive:** Different random splits can give different estimates
- **Unstable on small data:** If only 20% for testing, high variance in estimate
- **Wastes data:** Smaller training set reduces learning

---

## 3. k-Fold Cross-Validation

The **most important resampling method**. Dataset is divided into k equal folds. Each fold is used once as validation, remaining k-1 as training.

### 3.1 Procedure

For fold j = 1, ..., k:
1. **Use fold j as validation set**
2. **Use remaining k-1 folds as training set**
3. **Train model; evaluate on fold j; record error E_j**

**CV estimate:**
$$CV_k = \frac{1}{k}\sum_{j=1}^{k}E_j$$

### 3.2 Typical Choices

- **5-fold CV:** Good balance; commonly used
- **10-fold CV:** Standard in ML; good statistical properties
- **Leave-one-out (LOO):** k = n (each sample its own fold); rarely used (expensive)

### 3.3 Advantages

- **Stable:** Uses all data; less variable than holdout
- **Reproducible:** Deterministic given random seed
- **Data-efficient:** More training data per iteration than holdout
- **Standard:** Accepted in ML publications and benchmarks

**Example:** For 500 FEM samples, 10-fold CV trains on 450 samples, validates on 50, repeats 10 times; final estimate is average of 10 fold errors.

### 3.4 Stratified k-Fold

When data has **class imbalance**, preserve class proportions in each fold.

**Important for:**
- Defect detection (rare defects)
- Anomaly detection (rare anomalies)
- Damage classification (imbalanced damage states)

**Check for Understanding:**
- For 100 samples with 10% defect rate, what are the defect counts in 5-fold CV? In stratified 5-fold CV?

---

## 4. Leave-One-Out Cross-Validation (LOOCV)

Special case of k-fold where k = n.

### 4.1 Procedure

For each sample i:
1. **Leave sample i as validation**
2. **Train on remaining n-1 samples**
3. **Evaluate on sample i**

**Error:**
$$LOOCV = \frac{1}{n}\sum_{i=1}^{n}L(y_i,\hat{y}_{-i})$$

### 4.2 Properties

- **Nearly unbiased:** Excellent estimate of generalization (minimal bias)
- **Uses maximum training data:** n-1 samples per iteration
- **High variance:** Individual errors E_i are highly variable
- **Computationally expensive:** Must train n models (infeasible for n > ~1000)

### 4.3 When to Use

- Very small datasets (n < 50)
- High-value predictions (computational cost justified)
- Interpretable confidence (errors on each sample)

---

## 5. Repeated Random Subsampling

Also called **Monte Carlo cross-validation**. Repeated random train-test splits.

### 5.1 Procedure

Repeat R times:
1. Randomly split data into train/test
2. Train and evaluate
3. Record error E_r

**Error estimate:**
$$E = \frac{1}{R}\sum_{r=1}^{R}E_r$$

### 5.2 Flexibility

- Can use non-equal split sizes (e.g., 70/30, 80/20)
- Each iteration is independent
- Can parallelize easily

### 5.3 Applications

- Benchmarking robustness (repeated runs reduce variance)
- Split-sensitivity testing (see how results vary)
- Confidence intervals (resampling distribution of metric)

**Example:** Run 100 repeated random splits; compute mean accuracy and standard error; report 95% CI.

---

## 6. Bootstrap Resampling

Bootstrap samples are drawn **with replacement** from the original dataset.

### 6.1 Basic Bootstrap

**Generate bootstrap sample:**
$$D^{*(b)} = \{z_1^*,z_2^*,...,z_n^*\}$$

where each z_i^* is sampled uniformly with replacement from original data.

**Bootstrap estimate** (e.g., of accuracy):
$$\hat{\theta}_{boot}=\frac{1}{B}\sum_{b=1}^{B}\hat{\theta}^{*(b)}$$

**Bootstrap standard error:**
$$SE_{boot} = \sqrt{\frac{1}{B-1}\sum_{b=1}^{B}(\hat{\theta}^{*(b)}-\bar{\theta}^*)^2}$$

### 6.2 Confidence Intervals via Bootstrap

**Percentile method:** Use 2.5th and 97.5th percentiles of bootstrap distribution as 95% CI.

**BCa method (bias-corrected accelerated):** More accurate for skewed distributions.

### 6.3 Applications

- **Metric uncertainty:** RMSE ± confidence interval
- **Model stability:** How variable is accuracy across bootstrap samples?
- **Limited data:** Make maximum use of available samples
- **Nonparametric:** No distributional assumptions needed

**Example:** For GAN-generated data, bootstrap 1000 times; compute mean and SE of Frechet Inception Distance (FID).

### 6.4 Why Bootstrap Works

**Key insight:** Sample distribution approximates population distribution. Resampling from sample mimics sampling from population.

---

## 7. Out-of-Bag (OOB) Validation

Used in ensemble methods like Random Forests.

### 7.1 Property of Bootstrap

For each bootstrap sample D^*(b), **about 36.8% of original samples are not included** (these are "out-of-bag").

### 7.2 OOB Error Estimate

- Use out-of-bag samples as validation for each bootstrap sample
- Average error across bootstrap replications
- **Estimate of generalization error**, without separate test set

### 7.3 Advantage

- No need for separate validation set
- Uses all data for training
- Fast (validation happens as ensemble trains)

---

## 8. Nested Cross-Validation

**Gold standard** for academic ML benchmarking when hyperparameter tuning is involved.

### 8.1 Two-Level Structure

**Outer CV:** Multiple folds for unbiased evaluation
- For each outer fold:
  - **Inner CV:** Use training portion to tune hyperparameters
  - **Test portion:** Final evaluation with optimal hyperparameters from inner CV

### 8.2 Why Nested?

If you tune hyperparameters using the same CV that evaluates performance, you get **optimistically biased estimates** (you're selecting hyperparameters specifically to fit this data).

**Nested CV prevents this:** Inner CV is separated from outer evaluation.

### 8.3 Pseudocode

```
for each outer fold k = 1, ..., K:
    training_data = all except fold k
    test_data = fold k
    
    for each inner fold j = 1, ..., k:
        inner_train = training_data except inner fold j
        inner_val = inner fold j
        for each hyperparameter λ:
            train model on inner_train with λ
            evaluate on inner_val
        λ_optimal = best λ
    
    train final model on all training_data with λ_optimal
    evaluate on test_data
    record error E_k

final error = mean(E_1, ..., E_K)
```

### 8.4 When to Use

- Publication-quality benchmarking
- Comparing multiple algorithms fairly
- Reporting generalizable performance

---

## 9. Permutation Testing

Tests whether model performance is **statistically significant** or due to chance.

### 9.1 Procedure

1. **Train model on real labels:** Compute performance metric M_real
2. **Shuffle target labels randomly:** Permute y values
3. **Train model on shuffled labels:** Compute M_perm
4. **Repeat step 2-3 many times** (B times)
5. **Compute p-value:**

$$
p = \frac{1 + \left| \{ M_{\text{perm}} \geq M_{\text{real}} \} \right|}{1 + B}
$$

### 9.2 Interpretation

- **p < 0.05:** Real model performance is unlikely under random labels; model is learning genuine signal
- **p ≥ 0.05:** Cannot reject that performance is due to chance; possible data leakage or spurious correlations

### 9.3 Applications

- **Leakage detection:** Find if information leaks between train-test
- **Spurious correlations:** Validate that relationships are real
- **Surrogate validation:** Ensure FEM surrogate captures real physics

---

## 10. Validation Metrics Reporting

When resampling is repeated, **always report uncertainty** around point estimates.

### 10.1 Standard Reporting

**Mean metric:**
$$\bar{M}=\frac{1}{R}\sum_{r=1}^{R}M_r$$

**Standard deviation:**
$$s_M=\sqrt{\frac{1}{R-1}\sum_{r=1}^{R}(M_r-\bar{M})^2}$$

**95% Confidence interval:**
$$\bar{M} \pm 1.96\frac{s_M}{\sqrt{R}}$$

**Report as:** "Accuracy = 0.85 ± 0.03 (mean ± SE)" or "Accuracy = 0.85 [0.79, 0.91] (95% CI)"

### 10.2 Why This Matters

- Demonstrates statistical rigor
- Enables comparison between methods
- Shows stability/reproducibility of results

**Example:** Algorithm A: 0.80 ± 0.05 vs Algorithm B: 0.82 ± 0.06. With overlapping CIs, difference may not be significant.

**Check for Understanding:**
- If you report RMSE = 10.5 with SE = 0.8 from 100 CV folds, what is the approximate 95% CI?

---

## 11. Practical Guide: Choosing Validation Methods

| Scenario | Recommended Method | Reason |
|----------|-------------------|--------|
| Single large dataset (n > 1000) | 5-10 fold CV | Good balance of stability and training data |
| Small dataset (n < 100) | LOOCV or 10-fold | Maximize training data; stability |
| Very small dataset (n < 50) | LOOCV + bootstrap | Use all data; quantify uncertainty |
| Hyperparameter tuning | Nested CV | Unbiased evaluation of final model |
| Simple baseline | Holdout validation | Fast initial assessment |
| Stability assessment | Repeated random split | See how results vary |
| Uncertainty quantification | Bootstrap | Nonparametric confidence intervals |
| Significance testing | Permutation test | Check if performance is real |

---

## 12. Practical Use in AI and Engineering

### 12.1 Highest-Value Resampling Tools

1. **k-fold CV (10-fold):** Standard for surrogate model comparison
2. **Bootstrap:** Uncertainty quantification for neural network performance
3. **Nested CV:** Publication-quality benchmarking
4. **Repeated random split:** Stability testing for algorithm comparisons
5. **Stratified CV:** Class imbalance handling (damage detection)
6. **Permutation test:** Leakage detection in surrogate models

### 12.2 Applications

- **FEM surrogate validation:** 10-fold CV for model selection
- **GAN synthetic data assessment:** Bootstrap for uncertainty on quality metrics
- **Transformer training:** Early stopping via validation loss from CV
- **SHM classifiers:** Stratified CV for rare damage states
- **Structural prediction:** Bootstrap CIs on predicted displacements
- **Scientific publication:** Nested CV + permutation test for reviewer confidence

---

## Key Takeaways

1. **Resampling estimates generalization error** to unseen data; critical for model selection and reporting.

2. **k-fold cross-validation** is the standard; uses all data; balanced variance-stability trade-off.

3. **Holdout validation** is fast but wasteful and variable; use only for initial screening.

4. **Bootstrap resampling** provides nonparametric confidence intervals and uncertainty quantification.

5. **Nested CV** prevents optimistic bias when hyperparameters are tuned; use for publication-quality benchmarking.

6. **Permutation tests** validate statistical significance; detect leakage and spurious correlations.

7. **Always report uncertainty:** Mean ± SE or 95% CI, not just point estimates.

8. **Stratified CV** preserves class distributions; essential for imbalanced data.

---

## Connection to Next Modules

Resampling foundations connect to Lecture 12 (Experimental Design), where designed experiments use resampling for hypothesis testing. Lecture 14 (Uncertainty Quantification) extends bootstrap to Bayesian uncertainty. Lecture 11 (this one) is the gateway to all downstream machine learning validation and publication-quality AI research.
