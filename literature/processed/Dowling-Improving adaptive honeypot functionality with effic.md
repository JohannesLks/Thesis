## Analysis of "Improving adaptive honeypot functionality with efficient reinforcement learning parameters for automated malware" by Dowling et al.

This document provides a summary and analysis of the paper by Seamus Dowling, Michael Schukat, and Enda Barrett, published in the *Journal of Cyber Security Technology* in 2018.

---

### Summary of the Paper

#### **Problem**

The paper addresses the key limitations of traditional, static honeypots in the face of modern, highly automated cyber threats. Static honeypots, while useful for capturing attack data, are passive and predictable. Their inability to dynamically respond to attacker interactions means they often fail to engage attackers for extended periods. This results in smaller, less informative datasets, which limits the ability of security analysts to understand the full scope and methodology of an attack, particularly those carried out by automated malware like botnets. Previous adaptive honeypots (e.g., Heliza, RASSH) focused on prolonging interactions with *human* attackers using actions like insults and delays, which are ineffective against automated bots that constitute the vast majority of SSH-based attacks.

#### **Proposed Solution**

The authors propose an intelligent, adaptive honeypot that utilizes **Reinforcement Learning (RL)** to proactively interact with automated malware. The goal is to optimize the data collection process by maximizing the number of commands an attacker executes, thereby prolonging the interaction.

The main components of their proposed framework are:

1.  **Modified Honeypot Environment**: They use `Cowrie`, a medium- and high-interaction SSH honeypot, as the base environment. This environment is modified to interact with an RL agent.
2.  **Reinforcement Learning Agent**: A SARSA (State-Action-Reward-State-Action) learning agent is implemented using the PyBrain library. This agent learns an optimal policy for responding to attacker commands.
3.  **State-Action Space Formalism**: This is the core contribution of the paper.
    *   **State (S)**: The state is defined by the command entered by the attacker (e.g., `wget`, `sudo`, `rm`).
    *   **Action (A)**: The action space is specifically tailored for automated malware, reduced to a simple, effective set: `{allow, block, substitute}`. This contrasts with previous work that included actions like `insult` or `delay`, which are irrelevant to bots.
    *   **Reward (R)**: The reward function is simplified to encourage more interactions. A reward of `1` is given if the attacker's input is a recognized system or attacker command, and `0` otherwise. This directly incentivizes the agent to take actions that lead to more command transitions.

The agent and honeypot interact in a continuous loop: the honeypot receives a command (state), passes it to the agent, the agent selects an action based on its learned policy, the honeypot executes the action, observes the result, and receives a reward, which is then used to update the agent's policy for future interactions.

#### **Adaptation Mechanism**

The honeypot's adaptation is driven entirely by the RL agent's decision-making process.

*   **Triggers for Adaptation**: Adaptation is triggered by each command an attacker issues after compromising the honeypot. The agent treats each command as a state and must decide on the best response.
*   **Types of Changes**: Based on the agent's decision, the honeypot can:
    *   **`allow`**: Permit the execution of the attacker's command.
    *   **`block`**: Prevent the command from running, returning a standard error.
    *   **`substitute`**: Replace the command's output with a different, potentially deceptive response (e.g., returning an "incomplete command" message for a `cat` command).

The agent learns over time which of these actions, for a given command (state), is most likely to result in the attacker issuing subsequent commands, thereby maximizing the total number of interactions (command transitions) and the richness of the collected data.

#### **Evaluation/Metrics**

The paper evaluates the effectiveness of the adaptive honeypot using several key metrics and comparisons:

1.  **Learning Evolution**: The authors measure the convergence of the agent's Q-values for specific state-action pairs (e.g., for the `wget` command). Their results show that their simplified model converges on an optimal policy much faster than previous models like Heliza and RASSH.
2.  **Cumulative Command Transitions**: This is the primary metric for success. They compare the total number of commands executed on their adaptive honeypot against a standard, non-adaptive high-interaction honeypot during a live deployment. The adaptive honeypot captured four times more commands.
3.  **Transitions to Attacker Tools**: They specifically measure the number of times the attacker successfully transitioned to executing their own tools. The adaptive honeypot performed 63% better than the standard honeypot in this regard.

The evaluation demonstrates empirically that a honeypot adapting with an RL model tailored for automated malware is significantly more effective at gathering large, high-quality datasets than both static honeypots and adaptive honeypots designed for human interaction.

---

### Relevance to Thesis

The concepts presented by Dowling et al. are highly relevant to the **ADLAH (Adaptive Multi-Layered Honeynet Architecture)** thesis, particularly in defining the mechanics of adaptation.

#### **Similarities in Goals**

Both Dowling's framework and ADLAH share the fundamental goal of using adaptation to improve honeypot/honeynet effectiveness. Key shared objectives include:

*   **Increased Attacker Engagement**: Both aim to prolong interaction time to gather more extensive and higher-quality data on attacker tactics, techniques, and procedures (TTPs).
*   **Dynamic Response**: Both move beyond static defense, creating a dynamic environment that can react to attacker behavior in real-time.
*   **Data-Driven Improvement**: Both architectures are designed to learn from interactions to optimize future defensive and deceptive actions.

#### **Differences in Decision-Making Process**

The primary difference lies in the sophistication and mechanism of the adaptation policy.

*   **Dowling's Approach**: Uses a classic, model-free RL algorithm (SARSA) with a predefined, discrete, and relatively simple state-action space. The state is the current command, and the action is one of three fixed responses. The learning is on-policy and aims to maximize a simple reward signal (command transitions).
*   **ADLAH's Approach**: Aims to implement a more advanced RL methodology. The RL agent in ADLAH would likely need to handle a much more complex and continuous state space, representing the posture of an entire honeynet, not just a single command. The decision-making process is not just about responding to a single command but about orchestrating the behavior of a distributed system of honeypots. This involves learning a sophisticated policy that can manage network topology, change service banners, deploy new honeypot instances, and potentially route traffic between different layers of the honeynet architecture.

#### **Informing ADLAH's Design**

Dowling's paper provides valuable, empirically validated concepts that can directly inform the design of ADLAH's "action space" and "state representation."

*   **Action Space Design**: The success of Dowling's simplified action set (`{allow, block, substitute}`) underscores the importance of creating a *purposeful* and *efficient* action space. For ADLAH, this suggests that the action space, while needing to be more complex, should be composed of well-defined, impactful, and non-superfluous actions. Instead of just command-level responses, ADLAH's action space could include:
    *   `deploy_honeypot(type, config)`: Spin up a new honeypot service (e.g., a web server, an IoT device).
    *   `change_service_profile(honeypot_id, profile)`: Alter the appearance of a service (e.g., change SSH banner, web server content).
    *   `migrate_attacker(from_honeypot, to_honeypot)`: Move an attacker's session from a low-interaction to a high-interaction environment.
    *   `alter_network_path(source, dest, policy)`: Modify firewall or routing rules to change network accessibility.

*   **State Representation**: Dowling's use of the attacker command as the state is simple but effective for their goal. ADLAH requires a much richer state representation. Insights from the paper suggest that the state should include features that are direct indicators of attacker behavior and intent. A potential state vector for the ADLAH RL agent could include:
    *   **Features from a single honeypot**: Inspired by Dowling, this could be a sequence of recent commands, frequency of certain command types, or file system changes.
    *   **Network-level features**: Traffic volume, port scanning patterns, connections between internal honeypots.
    *   **Session-level features**: The number of active attacker sessions, session duration, and novelty of observed TTPs.
    *   **Honeynet configuration**: The current number and type of active honeypots.

In conclusion, Dowling et al.'s work provides a foundational and practical blueprint for applying RL to a single honeypot. It validates the core concept that an adaptive approach tailored to the dominant threat (automated malware) is highly effective. For the ADLAH thesis, it serves as a successful case study and a starting point for designing a more complex, multi-layered adaptive system, guiding the crucial design choices for its state representation and action space.