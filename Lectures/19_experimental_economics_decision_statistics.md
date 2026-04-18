# Experimental Economics and Decision Statistics

Experimental economics and decision statistics study how **individuals, groups, and intelligent agents make choices under uncertainty, incentives, and strategic interaction**.

Unlike standard predictive statistics, this asks: **How do incentives, information, and risk change decisions?**

This module is essential for:
- Behavioral data science and decision modeling
- A/B testing and product experimentation
- Mechanism design and market design
- Recommender system evaluation
- Pricing experiments and elasticity
- Auction systems
- Policy evaluation under behavioral effects
- Human-AI interaction and trust

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Design controlled experiments** to test decision behavior under risk and uncertainty
2. **Model preference and choice behavior** using utility functions
3. **Estimate demand and elasticity** from experimental data
4. **Analyze auction mechanisms** for efficiency and incentive compatibility
5. **Evaluate mechanism designs** for truthfulness and robustness
6. **Understand behavioral anomalies** (loss aversion, framing effects)
7. **Test strategic behavior** in multi-agent settings

---

## 1. Foundations of Decision Experiments

A decision experiment includes:
- **Decision maker** (individual or agent)
- **Choice set** (available options)
- **Payoff/utility** (consequences of choices)
- **Information structure** (what agents know)
- **Risk/uncertainty** (stochasticity)
- **Observed outcome** (actual choice)

---

## 2. Utility Functions and Expected Utility

**Expected utility hypothesis:**

$$U(act) = E[u(outcome|act)]$$

Agents choose action maximizing expected utility.

**Von Neumann-Morgenstern:** Axioms for rational preferences lead to expected utility representation.

---

## 3. Risk Preferences

**Risk aversion:** Prefer certain payoff over uncertain lottery with same expected value

**Loss aversion:** Losing $100 hurts more than gaining $100 feels good

**Probability weighting:** Rare events overweighted; common events underweighted

---

## 4. Choice Modeling

**Logit model:** Probability of choosing option i:

$$P(i|choice set) = \frac{e^{u_i}}{sum_j e^{u_j}}$$

Captures observed choice variability; β reflects noise or heterogeneity.

---

## 5. A/B Testing and Experimentation

**Randomized experiments:** Assign treatments randomly to measure causal effects

**Power calculation:** Determine sample size to detect effect of interest

**Multiple comparisons:** Correct for multiple hypothesis tests

Critical for product decisions, policy evaluation, recommendation systems.

---

## 6. Demand Estimation

**Demand function:** Q = f(P, income, other factors)

**Price elasticity:** ε = ∂log(Q)/∂log(P) measures price sensitivity

**Estimation from experiments:** Vary price; measure quantity demanded

---

## 7. Auction Mechanisms

**Sealed-bid first-price auction:** Winner pays highest bid

**English auction:** Open outcry; ascending prices

**Vickrey auction:** Sealed-bid second-price; incentive compatible (truthful bidding optimal)

**Analysis:** Which mechanism generates highest revenue? Encourages truthfulness?

---

## 8. Mechanism Design

Design mechanisms (rules, incentive structures) achieving goals:
- **Truthfulness:** Agents motivated to reveal true information
- **Efficiency:** Maximize total value created
- **Individual rationality:** No agent worse off than not participating

---

## 9. Behavioral Anomalies

Violations of rational choice:
- **Framing effects:** Different presentations of same choice give different decisions
- **Status quo bias:** Preference for current state
- **Endowment effect:** Overvalue what we own
- **Present bias:** Discount future too heavily

---

## 10. Strategic Interaction

In multi-agent settings, agents must anticipate others' decisions:
- **Nash equilibrium:** No agent wants to unilaterally deviate
- **Game theory:** Analyze strategic interdependencies
- **Learning in games:** How agents converge to equilibrium

---

## Key Takeaways

1. **Expected utility** framework models rational decision-making under uncertainty
2. **Experiments** enable causal inference; critical for product/policy decisions
3. **Demand estimation** reveals price sensitivity; enables optimal pricing
4. **Auction mechanisms** vary in revenue, efficiency, and incentive properties
5. **Mechanism design** aligns incentives with desired outcomes
6. **Behavioral anomalies** show systematic departures from rationality
7. **Strategic interaction** requires game-theoretic reasoning

---

## Connection to Next Modules

Decision statistics and experimental design connect to Lecture 20 (Statistical Thinking for AI Systems) for end-to-end system evaluation, trustworthy AI deployment, and monitoring decision system performance in real-world settings.
