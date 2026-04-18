# Uncertainty Quantification

Uncertainty Quantification (UQ) is the statistical framework for **identifying, representing, propagating, and interpreting uncertainty in data, models, and predictions**. The central question: **How uncertain are our conclusions, predictions, or decisions?**

Unlike Bayesian statistics (which focuses on belief updating), UQ focuses on **how uncertainty flows through a system**—from inputs through models to outputs.

This module is essential for:
- Predictive modeling with confidence bounds
- Simulation reliability and robustness
- Robust machine learning under uncertainty
- Engineering decision support
- Risk analysis and safety-critical systems
- Digital twins with uncertainty quantification
- Probabilistic forecasting

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Distinguish** aleatoric (irreducible) vs epistemic (reducible) uncertainty
2. **Propagate uncertainty** through models using linear approximation and Monte Carlo
3. **Construct** confidence and prediction intervals for predictions
4. **Perform sensitivity analysis** to identify dominant uncertainty sources
5. **Quantify model uncertainty** via ensemble methods
6. **Assess parameter uncertainty** from regression covariance
7. **Make decisions under uncertainty** using expected utility

---

## 1. Foundations of Uncertainty

For a predictive system Y = f(X), UQ quantifies how uncertainty in X affects Y:

**Two types of uncertainty:**
- **Aleatoric:** Irreducible natural randomness (sensor noise, environmental variability)
- **Epistemic:** Reducible lack of knowledge (limited data, imperfect models, unknown parameters)

Key insight: Not all uncertainty can be reduced the same way—epistemic reduces with more/better data; aleatoric persists.

**Check for Understanding:**
- Is modeling error aleatoric or epistemic? Can it be reduced?

---

## 2. Uncertainty Propagation

The fundamental UQ task: Given uncertain inputs X ~ P(X), determine output uncertainty for Y = f(X).

**Linear approximation (first-order):**
$$\text{Var}(Y) \approx \left(\frac{df}{dX}\right)^2 \text{Var}(X)$$

**Multivariate:**
$$\text{Var}(Y) \approx J \Sigma_X J^T$$

where J is Jacobian, Σ_X is input covariance.

---

## 3. Monte Carlo Uncertainty Propagation

**Most practical method:**
1. Sample inputs: X^(i) ~ P(X)
2. Evaluate model: Y^(i) = f(X^(i))
3. Compute output statistics: E[Y], Var(Y), percentiles

$$E[Y] \approx \frac{1}{N}\sum_{i=1}^{N}Y^{(i)}$$

---

## 4. Confidence and Prediction Intervals

**Confidence interval** (for estimated mean):
$$\bar{y} \pm 1.96\frac{s}{\sqrt{n}}$$

**Prediction interval** (for new observation):
$$\hat{y} \pm 1.96 \cdot \sigma_{\text{pred}}$$

Prediction intervals are wider (account for residual variance); essential for practical predictions.

---

## 5. Sensitivity Analysis

Determines which uncertain inputs dominate output variance:

**Variance-based (Sobol):**
$$S_i = \frac{\text{Var}(E[Y|X_i])}{\text{Var}(Y)}$$

Guides resource allocation: reduce uncertainty in high-sensitivity inputs first.

---

## 6. Model Uncertainty

Different models predict differently. Ensemble variance quantifies this:

$$\text{Var}_{\text{model}} = \frac{1}{M-1}\sum_{m=1}^{M}(\hat{y}_m - \bar{y})^2$$

Critical for surrogate modeling, ensemble ML, scientific computing.

---

## 7. Data Uncertainty

Sources: missing values, measurement error, label noise, sampling bias, finite sample size.

**Standard error:** SE = s/√n measures estimation uncertainty.

---

## 8. Parameter Uncertainty

Model parameters themselves are uncertain. Regression covariance:

$$\text{Cov}(\hat{\beta}) = \sigma^2(X^T X)^{-1}$$

Parameter uncertainty propagates to prediction intervals.

---

## 9. Calibration and Domain Shift

Model may be accurate on training data but highly uncertain on unseen domains:
- Calibration error: Model confidence vs true accuracy
- Domain shift: Extrapolation uncertainty
- Validation drift: Performance decay over time

---

## 10. Decision-Making Under Uncertainty

Integrate uncertainty into decisions via expected utility:

$$EU(a) = \sum P(y|a) U(y,a)$$

Enables risk-aware, robust decisions compared to ignoring uncertainty.

---

## Key Takeaways

1. **Aleatoric and epistemic** uncertainty differ in reducibility
2. **Monte Carlo propagation** is practical; linear approximation for small uncertainties
3. **Prediction intervals** broader than confidence intervals; required for practical predictions
4. **Sensitivity analysis** identifies dominant uncertainty sources
5. **Model/parameter/data** uncertainty all contribute to total prediction uncertainty
6. **Expected utility** integrates uncertainty into decision-making
7. **Robust AI systems** must quantify and communicate uncertainty

---

## Connection to Next Modules

UQ foundations connect to Lecture 15 (Causal Inference) for causal effect uncertainty, Lecture 17 (Probabilistic Machine Learning) for Bayesian neural networks, and real-world deployment where uncertainty informs safety margins and risk management.
