# Reinforcement Learning Statistics

Reinforcement Learning (RL) statistics provides the mathematical foundation for **learning from sequential interaction, delayed rewards, and uncertain environments**. Unlike supervised learning with labeled data, RL learns through trial, feedback, and long-term reward maximization.

The core question: **How can an agent learn optimal actions through interaction?**

This module is essential for:
- Sequential decision-making
- Adaptive control and robotics
- Recommendation systems
- Game-playing AI
- Bandit optimization and exploration
- Online learning
- Autonomous systems

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Formulate** Markov Decision Processes (MDPs) for sequential decision problems
2. **Estimate value functions** V(s) and Q(s,a) from experience
3. **Balance exploration vs exploitation** in bandit problems
4. **Apply temporal-difference learning** for efficient value estimation
5. **Understand policy gradient methods** for direct policy optimization
6. **Quantify uncertainty** in value estimates and decisions
7. **Design reward functions** and understand reward shaping

---

## 1. Foundations of Reinforcement Learning

RL system: Agent interacts with environment over time:
- **State:** s_t (situation description)
- **Action:** a_t (agent decision)
- **Reward:** r_t (immediate feedback)
- **Transition:** P(s_{t+1}|s_t,a_t) (environment dynamics)

**Goal:** Learn policy π(a|s) maximizing cumulative reward:

$$J = E\left[\sum_{t=0}^{T}\gamma^t r_t\right]$$

where γ is discount factor.

---

## 2. Markov Decision Processes (MDPs)

Formal framework: (S, A, P, R, γ)
- S: State space
- A: Action space
- P(s'|s,a): Transition probabilities
- R(s,a): Reward function
- γ: Discount factor

**Markov property:** Future only depends on current state, not history.

---

## 3. Value Functions

**State value:** V(s) = Expected cumulative reward starting from s

**Action value:** Q(s,a) = Expected cumulative reward starting from s and taking a

**Bellman equations:**
$$V(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)[R(s,a) + \gamma V(s')]$$

$$Q(s,a) = \sum_{s'} P(s'|s,a)[R(s,a) + \gamma \max_{a'} Q(s',a')]$$

---

## 4. Exploration vs Exploitation

Fundamental trade-off:
- **Exploitation:** Choose action with highest estimated value
- **Exploration:** Try other actions to learn better value estimates

**ε-greedy:** With prob ε try random action; with prob (1-ε) exploit best

**Upper Confidence Bound (UCB):** Explore actions with high uncertainty

---

## 5. Temporal-Difference (TD) Learning

Learn value function from experience without full environment model:

$$V(s) \leftarrow V(s) + \alpha [r + \gamma V(s') - V(s)]$$

TD error: δ = r + γV(s') - V(s) measures prediction error.

**Advantage:** Sample-efficient; works online; combines MC and DP benefits.

---

## 6. Q-Learning

Off-policy algorithm for action values:

$$Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma \max_{a'} Q(s',a') - Q(s,a)]$$

Learns optimal Q even if following exploratory policy.

---

## 7. Policy Gradient Methods

Directly optimize policy π_θ(a|s) via gradient ascent:

$$\nabla J(\theta) = E[\nabla_\theta \log \pi_\theta(a|s) Q(s,a)]$$

Actor-critic combines policy gradient with value function baseline for variance reduction.

---

## 8. Value Uncertainty

Uncertainty in value estimates drives exploration:
- **Upper Confidence:** Select actions with high upper confidence bound on Q(s,a)
- **Thompson Sampling:** Sample from posterior over values; act optimally under sample
- **Posterior Inference:** Use Bayesian methods to model Q-value uncertainty

---

## 9. Off-Policy vs On-Policy Learning

**On-policy:** Learn value of policy being executed

**Off-policy:** Learn optimal policy while executing different exploratory policy

Trade-off: On-policy has high variance, off-policy has high bias.

---

## 10. Reward Design and Shaping

**Reward function** critically affects learning. Common issues:
- **Sparse rewards:** Few positive signals; hard to learn
- **Reward hacking:** Agent exploits reward function, not intended behavior
- **Credit assignment:** Hard to know which actions led to reward

**Reward shaping:** Add auxiliary rewards (domain knowledge) to guide learning.

---

## Key Takeaways

1. **MDPs** formalize sequential decision-making with Bellman equations
2. **Value functions** V(s) and Q(s,a) estimate long-term consequences
3. **Temporal-difference learning** is sample-efficient and practical
4. **Exploration vs exploitation** trade-off is fundamental; address via ε-greedy, UCB, Thompson sampling
5. **Policy gradients** directly optimize action selection
6. **Value uncertainty** drives principled exploration
7. **Reward design** critically affects learning success

---

## Connection to Next Modules

RL foundations connect to Lecture 19 (Decision Statistics) for mechanism design and incentives, and Lecture 20 (Statistical Thinking for AI Systems) for end-to-end RL system monitoring and safety validation.
