# Structured Summary of "On the rewards of self-adaptive IoT honeypots"

**Full Title:** On the rewards of self-adaptive IoT honeypots
**Authors:** Adrian Pauna, Ion Bica, Florin Pop, Aniello Castiglione

## I. Structured Summary

### Objective
The paper introduces a self-adaptive IoT honeypot system that uses reinforcement learning (RL) to interact with attackers. The primary goal is to address a key challenge in self-adaptive honeypots: the subjective and often suboptimal nature of manually defined reward functions. The research proposes using Inverse Reinforcement Learning (IRL) to automatically derive near-optimal reward functions by observing and learning from expert-defined behaviors.

### Methodology
The authors developed **IRASSH-T**, a self-adaptive honeypot that extends QRASSH (a DQN-based honeypot). The methodology is divided into three main phases:
1.  **Manual Training (Expert Demonstration):** An expert manually interacts with the honeypot to demonstrate a desired behavior. This creates a set of "expert trajectories" (sequences of states/commands). For this study, two behaviors were defined:
    *   **Download Behavior:** Encouraging the attacker to download as much malicious code as possible.
    *   **Retain Behavior:** Maximizing the time an attacker spends in the honeypot by delaying command execution.
2.  **Inverse Reinforcement Learning (IRL):** The system uses an Apprenticeship Learning (AL) via IRL algorithm. This algorithm takes the expert trajectories and infers the underlying reward function that must have guided the expert's actions. It does this by calculating a weight vector for a set of predefined environmental features (e.g., `is_download_command`, `is_exit_command`).
3.  **Testing and Validation:** The reward function, derived from the calculated weights, is then implemented in the RL agent (the honeypot). The honeypot is exposed to real-world attack scenarios (using a dataset from Mirai botnet analysis) to validate whether it successfully reproduces the desired expert behavior.

### Core Concepts
*   **Self-Adaptive Honeypot:** A honeypot that uses machine learning (specifically RL) to interact dynamically with attackers, rather than being static. The agent can take actions like `allow`, `block`, `delay`, `fake`, or `insult` in response to attacker commands.
*   **Reinforcement Learning (RL):** An agent learns an optimal policy by taking actions in an environment to maximize a cumulative reward. In this context, the honeypot is the agent, commands are the environment, and the policy dictates how to respond to those commands.
*   **Inverse Reinforcement Learning (IRL):** Instead of being given a reward function to find a policy, IRL observes an expert's policy (behavior) and infers the reward function that best explains that behavior. This overcomes the challenge of manually engineering an effective reward function.

### Main Findings
*   The study successfully demonstrated that IRL can be used to generate effective, non-obvious reward functions for a self-adaptive honeypot.
*   The reward functions derived from the "download" and "retain" expert behaviors successfully induced the corresponding behaviors in the honeypot agent when tested.
*   The analysis of the learned weights provided insights into the reward structure. For example, the `download_command` feature received a high positive weight for the download behavior, while the `is_delay` feature received a high positive weight for the retain behavior. The `exit_command` consistently received a high negative weight, as keeping the attacker engaged was crucial for both behaviors.
*   The research proves it is possible to overcome the subjectivity and difficulty of manually designing reward functions, a key issue that limits the performance of adaptive honeypots.

## II. Relevance to Thesis (ADLAH)

The findings of Pauna et al. offer a direct and highly relevant critique and a constructive path forward for the reward function design within the ADLAH framework. The ADLAH thesis explicitly identifies the simplistic, quantity-based nature of its current reward function as a major limitation, and this paper provides a robust, empirically validated methodology for addressing precisely that problem.

1.  **Critique of ADLAH's Current Reward Function:** The ADLAH thesis currently proposes a reward function as a future enhancement: `R = (1 + omega * A_score) * N_logs - lambda * C`. Pauna et al.'s work directly validates the concern outlined in the ADLAH paper that a simple, log-quantity-based metric (`N_logs`) is insufficient and can be gamed. Their research demonstrates that focusing on the *quality* and *type* of interaction (e.g., encouraging downloads, increasing session time) yields more strategically valuable results than simply counting logs.

2.  **Informing the Design of ADLAH's Reward Function:** The methodology of Pauna et al. provides a blueprint for how ADLAH's reward function can evolve.
    *   **Feature-Based Reward Structure:** Instead of relying on a monolithic `A_score`, ADLAH could adopt a feature-based reward function similar to the one used in IRASSH-T. The "sensors" described by Pauna et al. (`is_download_command`, `is_hacking_command`, etc.) can be directly translated into features for ADLAH's post-interaction analysis. The anomaly score (`A_score`) from ADLAH's autoencoder can be one such feature, but it should be complemented by others that capture different dimensions of interaction quality, such as:
        *   `session_duration` (from "retain" behavior)
        *   `unique_command_count`
        *   `payload_transfer_volume` (from "download" behavior)
        *   `use_of_novel_tools`
    *   **Using IRL to Find the Weights (`omega`):** The core challenge in ADLAH's proposed reward function is determining the value of the weighting hyperparameter `omega`. Pauna et al.'s use of IRL provides a direct solution. By defining an "expert policy" for ADLAH (e.g., an analyst manually flagging a session as high-value), the IRL algorithm could be used to automatically learn the optimal weights for each feature in the reward function, effectively determining the `omega` for not just the anomaly score but for a whole vector of quality-based features.

3.  **Addressing the Quality vs. Quantity Trade-off:** The ADLAH thesis correctly identifies the risk of an agent valuing a high volume of noise over a stealthy, significant interaction. Pauna et al.'s work offers a practical solution by separating behaviors ("download" vs. "retain"). This suggests that ADLAH might benefit from having multiple, specialized reward functions or a single, complex one that can balance competing objectives. The paper's conclusion that learned weights are not always intuitive underscores the value of an automated, data-driven approach like IRL over manual tuning. By learning from expert demonstrations, the system can discover the subtle trade-offs between different signals that lead to the desired intelligence-gathering outcomes.

In conclusion, the Pauna et al. paper is not merely related work; it provides a direct, actionable roadmap for addressing one of the most critical future work items identified in the ADLAH thesis: the design of a sophisticated, quality-driven reward function for its DQN agent. It moves the discussion from a conceptual formula (`R = ...`) to a practical, proven methodology for its implementation.