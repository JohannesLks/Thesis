### **Analysis of Kristyanto et al. (2022): Evaluation of Reinforcement Learning on SSH Honeypot**

The paper by Kristyanto, Studiawan, and Pratomo evaluates the impact of a Deep Q-Network (DQN) reinforcement learning algorithm on a Cowrie SSH honeypot. The primary goal was to create an adaptive honeypot that could learn from attacker behavior to prolong interaction times, thereby gathering more intelligence.

*   **Key Information:**
    *   **Model and Algorithm:** The study integrates a **DQN algorithm** with the **Cowrie honeypot**. It compares the performance of this RL-enhanced honeypot against an unmodified (default) Cowrie instance.
    *   **Actions:** The RL agent can perform five actions in response to an attacker's command: `Allow`, `Delay`, `Block`, `Fake`, and `Insult`.
    *   **Reward Function:** The reward mechanism is designed to incentivize the agent to lead the attacker toward executing a `download` command (e.g., `wget`, `tftp`).
        *   `+500` for a download command.
        *   `+200` for a hacking tool command (e.g., `nmap`).
        *   `0` for a known, non-download command.
        *   `-200` for an unknown command.
        *   `-500` for the attacker disconnecting.
    *   **Key Findings:**
        *   **Increased Interaction Time:** The RL-enabled honeypot achieved a significantly longer average interaction duration (**5.28 seconds**) compared to the standard Cowrie honeypot (**1.57 seconds**). This demonstrates the effectiveness of the adaptive response strategy.
        *   **Attacker Command Patterns:** The paper identifies a common sequence of commands executed by attackers before attempting a download:
            1.  System information gathering (`cat /proc/cpuinfo`).
            2.  Filtering for specific hardware details (e.g., `uniq`, `nvidia-smi`).
            3.  Executing the download (`wget`).
            4.  Running a script (`perl`).
    *   **Missing Details:** The paper does **not** provide specific DQN hyperparameters (like learning rate, discount factor γ, epsilon values for exploration/exploitation) or the exact neural network architecture (number of layers, neurons per layer). It also lacks learning curves (e.g., reward/loss over time) to show model training and convergence.

<br>

### Relevance to Thesis

The information from this paper, while not as detailed as a full supplementary document might be, provides significant value to the design and justification of the **ADLAH (Adaptive Multi-Layered Honeynet Architecture)**.

*   **Implementation Details:** The defined reward function and action space (`Allow`, `Delay`, `Block`, etc.) serve as a validated, concrete starting point for ADLAH's RL agent. By adopting this proven structure, we can reduce the initial experimental phase of determining effective rewards and actions, accelerating the development of our own adaptive response module. The paper establishes a baseline for what a successful reward strategy looks like in an SSH honeypot context.

*   **Deeper Behavioral Insights:** The analysis of command sequences leading to a malware download is highly valuable. The identified pattern (`cpuinfo` -> `uniq` -> `wget` -> `perl`) provides a clear, data-driven "Tactic, Technique, and Procedure" (TTP). This insight can directly inform the design of ADLAH's state representation. Instead of just considering the current command, the state could include a history of recent commands, allowing the agent to recognize this specific TTP as it unfolds. This enables more sophisticated, context-aware responses rather than reacting to commands in isolation.

*   **Evidence of Stability and Convergence:** Although the paper omits explicit learning curves, the tripling of the average interaction time is strong empirical evidence that the DQN approach is viable and effective in a live environment. It demonstrates that an RL agent, even without perfectly tuned hyperparameters, can successfully learn a policy that outperforms a static system. This finding strengthens the core argument for ADLAH: that adaptive, RL-driven defenses are not just theoretically interesting but practically superior for engaging attackers and gathering intelligence. The stability of the outcome (longer sessions) provides confidence that this method can be reliably deployed within the ADLAH framework.