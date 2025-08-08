# Summary of "Q-Learning" by Watkins and Dayan (1992)

## 1. Introduction
- **Q-learning** is a model-free reinforcement learning algorithm.
- It allows an agent to learn an optimal policy in a controlled Markovian domain without needing a model of the environment.
- The learning process is incremental and similar to temporal difference (TD) learning.
- The agent learns from the consequences of its actions, based on immediate rewards and its estimate of the value of the next state.

## 2. The Task for Q-Learning
- The agent operates in a discrete, finite world (a controlled Markov process).
- At each step, the agent observes the state `x`, chooses an action `a`, receives a reward `r`, and transitions to a new state `y`.
- The goal is to find an optimal policy `π*` that maximizes the total discounted expected reward.
- **Q-values** `Q(x, a)` represent the expected discounted reward for taking action `a` in state `x` and following a specific policy thereafter.
- The optimal Q-values, `Q*(x, a)`, are unique and allow the agent to determine the optimal policy by simply choosing the action with the maximum Q-value in each state.

## 3. The Q-Learning Algorithm
- The agent's experience is a sequence of episodes: `(xn, an, yn, rn)`.
- The Q-values are updated iteratively using the following rule:
  `Qn(x, a) = (1 - αn) * Qn-1(x, a) + αn * (rn + γ * max_b(Qn-1(yn, b)))`
- `αn` is the learning rate.
- `γ` is the discount factor.
- The update is only applied to the Q-value for the state-action pair that was just experienced.

## 4. Convergence Proof
- The paper provides a detailed proof that Q-learning converges to the optimal Q-values with probability 1.
- **Key Conditions for Convergence:**
    - Bounded rewards.
    - The learning rates `αn` must satisfy certain conditions (`Σαn = ∞`, `Σαn^2 < ∞`).
    - All state-action pairs must be visited infinitely often.
- The proof introduces an **Action-Replay Process (ARP)**, an artificial controlled Markov process constructed from the sequence of learning episodes.
- **Lemma A:** The Q-values `Qn(x, a)` at step `n` are the optimal action values for the ARP at level `n`.
- **Lemma B:** The ARP converges to the real process as `n` approaches infinity.
- By combining these lemmas, the paper shows that the optimal Q-values of the ARP (which are the agent's learned Q-values) converge to the optimal Q-values of the real process.

## 5. Extensions and Discussion
- The convergence proof can be extended to:
    - Non-discounted (`γ = 1`) problems with absorbing goal states.
    - Cases where multiple Q-values are updated in each iteration.
- The paper discusses the relationship between Q-learning and certainty-equivalence methods, framing Q-learning as a form of incremental dynamic programming.
- The proof does not cover the case of multi-step updates (like Sutton's `TD(λ)`), which would require different proof techniques.

## 6. Significance
- This paper provides the first formal proof of convergence for the Q-learning algorithm.
- It established Q-learning as a foundational and provably correct method for reinforcement learning, paving the way for many subsequent developments in the field.