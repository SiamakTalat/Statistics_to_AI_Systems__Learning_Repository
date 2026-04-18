# Bayesian Statistics

Bayesian statistics provides a framework for **updating beliefs using observed data**. Unlike classical frequentist statistics, Bayesian methods treat unknown parameters as **random variables with probability distributions**, enabling probability to serve as a language of belief updating.

This module is fundamental for:
- Uncertainty-aware machine learning
- Probabilistic inference and parameter estimation
- Decision-making under uncertainty
- Adaptive learning systems
- Robust prediction with confidence bounds
- Scientific reasoning from evidence

The central idea: **Prior Belief + Data Evidence = Posterior Knowledge**

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Explain** Bayes' theorem and its components (prior, likelihood, posterior, evidence)
2. **Apply conjugate prior models** for analytical Bayesian learning
3. **Compute posterior summaries** (mean, median, credible intervals, MAP estimates)
4. **Construct Bayesian regression models** with coefficient uncertainty
5. **Use posterior predictive distributions** for probabilistic forecasting
6. **Implement MCMC sampling** for complex posteriors
7. **Compare models** using Bayes factors

---

## 1. Foundations of Bayesian Thinking

Bayesian inference treats unknown parameters as uncertain, quantified by probability distributions.

**Prior:** P(θ) = Belief before observing data
**Likelihood:** P(D|θ) = Probability of data given parameter  
**Posterior:** P(θ|D) = Updated belief after data

**Intuition:** Start with prior → see data → update to posterior

---

## 2. Bayes' Theorem

The central equation of Bayesian statistics:

$$P(\theta|D) = \frac{P(D|\theta)P(\theta)}{P(D)}$$

where:
- **P(θ):** Prior
- **P(D|θ):** Likelihood
- **P(D):** Evidence/marginal likelihood
- **P(θ|D):** Posterior

**Simplified:** Posterior ∝ Likelihood × Prior

**Check for Understanding:**
- If you have a weak prior (high uncertainty) and strong likelihood, what dominates the posterior?

---

## 3. Prior Distributions

Priors encode domain knowledge before data observation.

**Common families:**
- **Beta:** For probability parameters (p)
- **Normal:** For means (μ)
- **Gamma:** For rates/variances
- **Dirichlet:** For probability vectors

**Benefits of priors:**
- Incorporate expert knowledge
- Regularize learning (like Ridge regression)
- Enable learning from limited data
- Avoid overfitting

---

## 4. Likelihood Function

Likelihood measures how probable observed data is for given parameter:

$$L(\theta) = P(D|\theta) = \prod_{i=1}^{n} P(x_i|\theta)$$

**Bridges data and parameter belief updating.** High likelihood at some θ means that θ explains data well.

---

## 5. Posterior Distribution

Posterior combines prior and data:

$$P(\theta|D) \propto P(D|\theta)P(\theta)$$

From posterior we compute:
- Posterior mean/median
- Credible intervals
- Predictive distributions
- Decision probabilities

---

## 6. Conjugate Priors

Conjugate priors produce posteriors in same distribution family (analytical tractability).

**Beta-Binomial:** p ~ Beta(α,β), data ~ Binomial(n,p) → posterior: p|data ~ Beta(α+x, β+n-x)

**Normal-Normal:** μ ~ N(μ₀,σ₀²), data normal → posterior: μ|data ~ N(..., ...)

---

## 7. Posterior Estimates

**Posterior mean:** E[θ|D] = Optimal under squared loss
**MAP (Maximum a posteriori):** argmax P(θ|D) = Bayesian regularization  
**Posterior median:** 50th percentile = Robust to outliers

---

## 8. Credible Intervals

95% credible interval [a,b] satisfies: P(a ≤ θ ≤ b|D) = 0.95

**Intuitive interpretation:** 95% probability parameter is in interval (vs frequentist "if we repeated sampling...")

---

## 9. Posterior Predictive Distribution

Prediction integrating over parameter uncertainty:

$$P(x_{new}|D) = \int P(x_{new}|\theta)P(\theta|D)d\theta$$

Provides probabilistic forecasts with uncertainty, critical for robust decision-making.

---

## 10. Bayesian Regression

Linear regression with uncertain coefficients:

$$y = X\beta + \epsilon, \quad \beta \sim N(0, \sigma^2 I)$$

Posterior: P(β|X,y) provides coefficient uncertainty, credible intervals on predictions.

---

## 11. MCMC Sampling

For complex posteriors without closed forms, use sampling (Metropolis-Hastings, Gibbs, Hamiltonian MC):

Generate θ^(1), ..., θ^(N) from posterior; approximate expectations via:

$$E[f(\theta)] \approx \frac{1}{N}\sum_{i=1}^{N} f(\theta^{(i)})$$

---

## 12. Bayesian Model Comparison

**Bayes Factor:** BF₁₂ = P(D|M₁)/P(D|M₂)  
- BF > 1: Favors M₁  
- BF < 1: Favors M₂

Principled model selection incorporating model complexity.

---

## Key Takeaways

1. **Prior + Likelihood = Posterior** via Bayes' theorem
2. **Credible intervals** provide intuitive probability statements about parameters
3. **Conjugate priors** enable analytical Bayesian learning
4. **Posterior predictive** incorporates parameter uncertainty into predictions
5. **MCMC** samples complex posteriors computationally
6. **Bayes factors** compare models principledl

y
7. **Bayesian methods** naturally quantify uncertainty (critical for AI safety)

---

## Connection to Next Modules

Bayesian foundations connect to Lecture 14 (Uncertainty Quantification) for rigorous uncertainty propagation, Lecture 17 (Probabilistic Machine Learning) for deep Bayesian models, and Lecture 18 (Reinforcement Learning) for Bayesian optimization and exploration.
