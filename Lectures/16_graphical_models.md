# Graphical Models

Graphical models provide a mathematical framework for **representing complex probabilistic relationships using graphs**. Instead of writing full joint distributions directly, encode variable relationships through **nodes (variables) and edges (dependencies)**.

This module is essential for:
- Probabilistic reasoning and inference
- Bayesian networks and causal modeling
- Latent variable discovery
- Structured machine learning
- Uncertainty-aware AI systems
- Efficient computation via factorization

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Understand graph-based probability factorization** for joint distributions
2. **Apply directed vs undirected graphical models** (Bayesian networks vs MRFs)
3. **Identify conditional independence** using d-separation in DAGs
4. **Perform inference** in graphical models (marginal, conditional, MAP)
5. **Explain Hidden Markov Models** for sequential data
6. **Connect graphical structures** to causal reasoning

---

## 1. Foundations of Graphical Models

Graphical models represent variables X₁, ..., X_n as nodes with probabilistic relationships as edges.

**Main advantage:** Factorize complex joint distribution P(X₁,...,X_n) into smaller local terms:

**Directed:** P(X₁,...,X_n) = ∏ᵢ P(Xᵢ|Pa(Xᵢ))
**Undirected:** P(X) = (1/Z) ∏_c φ_c(X_c)

Dramatically reduces parameters and enables efficient inference.

---

## 2. Bayesian Networks (Directed Graphical Models)

Directed Acyclic Graphs (DAGs) where each node depends only on parents:

**Factorization:**
$$P(X_1,...,X_n) = \prod_{i=1}^{n}P(X_i|Pa(X_i))$$

**Example:** A → B → C gives P(A,B,C) = P(A)P(B|A)P(C|B)

**Applications:**
- Causal reasoning
- Diagnostic systems
- Decision support
- Scientific modeling

---

## 3. Conditional Independence

**Most important concept:** Two variables X ⊥ Y | Z (conditionally independent) if knowing Z makes X and Y independent.

**Graph representation:** Encodes conditional independence structure. Reduces model complexity, speeds inference.

---

## 4. d-Separation in DAGs

Determines conditional independence from graph structure:

**Chain:** X → Z → Y; conditioning on Z blocks path
**Fork:** Z → X, Z → Y; conditioning on Z blocks common cause
**Collider:** X → Z ← Y; conditioning on Z **opens** dependence (creates association)

These patterns generalize to larger networks via d-separation algorithm.

---

## 5. Markov Random Fields (Undirected Graphical Models)

Represent **symmetric** dependence relationships:

$$P(X) = \frac{1}{Z}\prod_{c \in C}\phi_c(X_c)$$

where φ_c are clique potentials, Z is partition function.

**Applications:**
- Image segmentation and labeling
- Spatial statistics
- Structured prediction
- Energy-based models

---

## 6. Factor Graphs

Separate variable nodes from factor nodes for explicit factorization representation:

$$P(X) = \frac{1}{Z}\prod_{k}f_k(X_k)$$

Enable message-passing algorithms, belief propagation, structured inference.

---

## 7. Inference in Graphical Models

Three main inference tasks:

1. **Marginal inference:** P(Xᵢ) - what's the marginal probability?
2. **Conditional inference:** P(Xᵢ|evidence) - belief given observations
3. **MAP inference:** argmax P(X|evidence) - most likely assignment

---

## 8. Exact Inference Methods

**Variable elimination:** Repeatedly sum out hidden variables

**Belief propagation:** Message passing on trees for efficient computation

Efficient on sparse graphs; exponential on dense graphs.

---

## 9. Approximate Inference

For large graphs, use approximate methods:
- **Monte Carlo sampling:** Generate samples from posterior
- **Gibbs sampling:** Iterative sampling updates
- **Variational inference:** Optimize approximating distribution
- **Loopy belief propagation:** Approximate message passing

---

## 10. Hidden Markov Models (HMMs)

Sequential graphical model: Z_t (hidden state) → X_t (observed)

**Markov assumption:** P(Z_t|Z_{t-1})
**Observation model:** P(X_t|Z_t)

**Applications:**
- Speech recognition
- Sequence labeling
- Anomaly detection in time series
- State estimation in dynamic systems

---

## 11. Graphical Models and Causality

Bayesian networks are natural for causal reasoning. DAGs encode:
- Causal assumptions (arrows show direct causation)
- Confounding structure (common causes)
- Mediation pathways
- Intervention effects

---

## Key Takeaways

1. **Factorization** reduces parameters and computational complexity
2. **Conditional independence** (d-separation) encodes in graph structure
3. **Directed models** (Bayesian networks) suitable for causal reasoning
4. **Undirected models** (MRFs) for symmetric local interactions
5. **Inference** answers questions about unknown variables given evidence
6. **HMMs** extend to sequential probabilistic reasoning
7. **Graphical models** form backbone of modern probabilistic AI

---

## Connection to Next Modules

Graphical models connect to Lecture 17 (Probabilistic Machine Learning) for deep generative models, and Lecture 18 (Reinforcement Learning) where graphical structures appear in POMDP representations and value function approximation.
