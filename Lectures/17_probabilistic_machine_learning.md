# Probabilistic Machine Learning

Probabilistic machine learning extends classical ML by representing **predictions, parameters, and latent structure as probability distributions** rather than fixed values.

Instead of only asking "What is the prediction?", also ask: **"How certain is the prediction?"**

This module is essential for:
- Uncertainty-aware prediction
- Latent variable modeling (clustering, topic models)
- Probabilistic clustering and density estimation
- Robust forecasting with confidence bounds
- Missing-data inference
- Trustworthy and explainable AI
- Decision support systems

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Learn distributions** P(Y|X) instead of point predictions
2. **Apply Gaussian Processes** for nonparametric regression with uncertainty
3. **Use Variational Autoencoders (VAEs)** for latent representation learning
4. **Implement Probabilistic PCA** for uncertainty-aware dimensionality reduction
5. **Understand mixture models** (Gaussian mixtures, Latent Dirichlet Allocation)
6. **Quantify prediction uncertainty** and model confidence
7. **Build Bayesian Neural Networks** for weight uncertainty

---

## 1. Probabilistic vs Deterministic Learning

**Deterministic:** ŷ = f(x) - single prediction

**Probabilistic:** P(Y|X) - full distribution over outputs

Enables:
- Confidence intervals on predictions
- Risk-aware decisions
- Uncertainty-driven exploration
- Principled missing-data handling

---

## 2. Gaussian Processes

Nonparametric Bayesian regression treating functions as distributions.

**GP prior:** P(f) - distribution over smooth functions

**Posterior:** P(f|data) - updated after observations

**Prediction:** P(y*|x*,data) - predictive distribution at new x*

Provides mean prediction + confidence bounds naturally. Computationally expensive for large n.

---

## 3. Variational Autoencoders (VAEs)

Learn latent representations via:
- **Encoder:** q(z|x) maps data to latent distribution
- **Decoder:** p(x|z) reconstructs from latents
- **Latent prior:** p(z) = N(0,I)

Enables:
- Generative modeling
- Unsupervised representation learning
- Anomaly detection via reconstruction error

---

## 4. Mixture Models

**Gaussian Mixture Model:** Data generated from mixture of K Gaussians:

$$P(x) = \sum_{k=1}^{K} \pi_k N(x|\mu_k, \Sigma_k)$$

EM algorithm estimates mixing proportions π_k and component parameters.

**Applications:**
- Clustering with soft assignments
- Density estimation
- Model selection via BIC

---

## 5. Latent Variable Models

Learn hidden structure Z explaining observed X:

$$P(X) = \int P(X|Z)P(Z)dZ$$

**Factor Analysis, PCA, topic models** are special cases.

Enables:
- Interpretable latent factors
- Dimension reduction
- Sparse representations

---

## 6. Probabilistic PCA

PCA with explicit probability model:

$$x_i = Wz_i + μ + ε$$

where W is loading matrix, z_i are latent factors.

**Benefits:**
- Handles missing data
- Principled model selection (MLE)
- Posterior uncertainty on reconstructions

---

## 7. Bayesian Neural Networks

Neural networks with weight distributions rather than point estimates:

**Prior:** P(W) - distribution over weights

**Posterior:** P(W|data) - updated after training

**Predictions:** Integrate over weight uncertainty for robust predictions

---

## 8. Uncertainty Quantification in Neural Networks

Practical approaches:
- **Dropout as uncertainty:** Dropout at test time samples weights
- **Ensemble methods:** Multiple independent models capture uncertainty
- **Temperature scaling:** Calibrate network confidence

---

## 9. Density Estimation

Learn probability density of data distribution:

$$\hat{p}(x) = \frac{1}{N}\sum_{i=1}^{N} K_h(x-x_i)$$

Applications:
- Anomaly detection (low density → anomaly)
- Generative modeling
- Importance sampling

---

## 10. Expectation-Maximization (EM) Algorithm

Iterative algorithm for latent variable models:

1. **E-step:** Compute posterior over latents P(Z|X,θ)
2. **M-step:** Maximize likelihood with respect to θ

Converges to local maximum of likelihood.

---

## Key Takeaways

1. **Probabilistic models** learn distributions, enabling uncertainty quantification
2. **Gaussian Processes** provide non-parametric Bayesian regression with natural uncertainty
3. **VAEs** learn interpretable latent representations
4. **Mixture models** provide soft clustering and density estimation
5. **Bayesian neural networks** quantify weight uncertainty
6. **EM algorithm** enables efficient latent variable learning
7. **Probabilistic methods** are essential for trustworthy, robust AI

---

## Connection to Next Modules

Probabilistic foundations connect directly to Lecture 18 (Reinforcement Learning Statistics) for value uncertainty and exploration, and Lecture 20 (Statistical Thinking for AI Systems) for end-to-end uncertainty integration.
