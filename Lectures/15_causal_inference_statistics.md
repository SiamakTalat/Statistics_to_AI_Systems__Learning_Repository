# Causal Inference Statistics

Causal inference is the statistical framework for **estimating cause-effect relationships from data**. Unlike predictive statistics ("what is associated with Y?"), causal inference asks: **What would happen if we intervene on X?**

This module is essential for:
- Evidence-based decision making
- Policy evaluation and impact assessment
- Medical and social studies
- AI fairness and debiasing analysis
- Treatment effect estimation
- Intervention design
- Scientific causal reasoning

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Distinguish correlation from causation** and explain why it matters
2. **Apply the potential outcomes framework** for causal effect definition
3. **Estimate Average Treatment Effects (ATE)** from randomized and observational data
4. **Identify confounders** using Directed Acyclic Graphs (DAGs)
5. **Apply backdoor adjustment** for confounder control
6. **Use propensity scores** for covariate balance in observational studies
7. **Interpret Instrumental Variables** for hidden confounding

---

## 1. Foundations of Causal Reasoning

Causal reasoning asks: Y = f(X, U), what happens to Y if we **change X** (intervention)?

**Key contrast with association:**
- **Association:** Are X and Y related?
- **Causation:** Does changing X change Y?

Example: Doctor and patient go together (associated) but doctor doesn't cause patient's illness.

---

## 2. Potential Outcomes Framework

The Rubin Causal Model defines two potential outcomes for each unit:
- Y(1) = outcome if treated
- Y(0) = outcome if untreated

Individual causal effect: τ = Y(1) - Y(0)

**The Fundamental Problem of Causal Inference:** We never observe both Y(1) and Y(0) for the same unit—only one is realized.

---

## 3. Average Treatment Effect (ATE)

The key estimand:

$$\text{ATE} = E[Y(1) - Y(0)]$$

Measures average causal effect across population. Used in clinical trials, A/B testing, policy evaluation.

---

## 4. Randomized Controlled Experiments

Gold standard for causality: Randomized assignment ensures treatment is independent of potential outcomes.

If T is randomized:
$$T \perp (Y(1), Y(0))$$

Simple estimator:
$$\hat{\text{ATE}} = \bar{Y}_{\text{treated}} - \bar{Y}_{\text{control}}$$

No confounding bias.

---

## 5. Confounding and Bias

A confounder Z affects both treatment and outcome:
- Z → X
- Z → Y

If ignored, treatment effect estimates are **biased** (mix treatment effect with Z's effect).

---

## 6. Directed Acyclic Graphs (DAGs)

Visual language for causal assumptions. Shows:
- Direct causal relationships (arrows)
- Confounders (common causes)
- Mediators (intermediate variables)
- Colliders (common effects)

DAGs help identify valid adjustment sets for causal estimation.

---

## 7. Backdoor Adjustment

To remove confounding, adjust for variables satisfying the backdoor criterion:

$$P(Y | \text{do}(X)) = \sum_z P(Y|X,z)P(z)$$

Converts observational estimates into causal estimates under assumptions.

---

## 8. Propensity Score Methods

Propensity score: e(X) = P(T=1|X) = probability of receiving treatment

Used for:
- **Matching:** Pair treated/control units with similar propensity
- **Weighting:** Reweight to simulate randomization
- **Stratification:** Create balanced strata

---

## 9. Instrumental Variables (IV)

For hidden confounding, use instrument Z:
- Z affects treatment X
- Z doesn't directly affect outcome Y
- Z independent of hidden confounders

$$\beta_{\text{IV}} = \frac{\text{Cov}(Z,Y)}{\text{Cov}(Z,X)}$$

---

## 10. Mediation Analysis

When treatment acts through intermediate variable M:
- X → M → Y (indirect/mediated effect)
- X → Y (direct effect, not through M)

Separates mechanism understanding.

---

## 11. Causal Validity Assumptions

Causal conclusions require strong assumptions:
- **Exchangeability:** No hidden confounders
- **Positivity:** All units can receive both treatments
- **Consistency:** Stable treatment unit value
- **No hidden confounding**
- **Correct model specification**

---

## Key Takeaways

1. **Causation ≠ correlation;** intervention thinking required
2. **Potential outcomes** define individual and average causal effects
3. **Randomization** removes confounding; most valid design
4. **DAGs** map causal assumptions; identify confounders
5. **Backdoor adjustment/propensity scores** handle observational confounding
6. **Instrumental variables** address hidden confounding
7. **Causal claims** only as strong as assumptions; always state them

---

## Connection to Next Modules

Causal graphs (DAGs) directly connect to Lecture 16 (Graphical Models), where probabilistic structure reasoning extends to directed acyclic structures. Causal inference principles underpin responsible AI (fairness, debiasing) and evidence-based decision systems.
