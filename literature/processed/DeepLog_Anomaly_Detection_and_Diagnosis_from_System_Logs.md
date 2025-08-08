# Summary and Analysis of "DeepLog: Anomaly Detection and Diagnosis from System Logs"

This document provides a summary of the paper "DeepLog: Anomaly Detection and Diagnosis from System Logs" by Du et al. and discusses its relevance to the ADLAH (Adaptive Multi-Layered Honeynet Architecture) thesis.

## Paper Summary

### Problem
The primary challenge DeepLog addresses is the difficulty of performing timely and effective anomaly detection in large, unstructured system logs. Traditional methods often rely on domain-specific rules, require offline processing of the entire dataset, or fail to capture the complex, sequential nature of log events, especially in concurrent systems. The goal is to create a general-purpose, online system that can learn normal system behavior from logs and detect unknown anomalies in a streaming fashion without prior knowledge of attack signatures.

### Methodology
DeepLog treats system log analysis as a natural language processing problem. Its methodology consists of two main components:

1.  **Log Parsing (Log Keying):** Raw, unstructured log messages are first parsed into structured events. Each log entry is converted into a **log key**, which is a constant string template representing the message type (e.g., `Took * seconds to build instance.`), and a corresponding **parameter value vector** that captures the variable parts of the message (e.g., timestamps, metrics, IDs). This structures the data while retaining important quantitative information.

2.  **LSTM-based Modeling:** DeepLog uses a Long Short-Term Memory (LSTM) network, a type of Recurrent Neural Network (RNN), to model the sequence of log keys. By training on log sequences from normal system operations, the LSTM learns the "grammar" and patterns of valid execution paths. It implicitly captures complex, non-linear dependencies between log events over time. A separate LSTM model is also trained for the parameter value vectors associated with each log key to detect performance-related anomalies.

### Anomaly Detection
Anomaly detection in DeepLog is a predictive process:

1.  **Execution Path Anomaly:** Given a recent history of log keys (a window of events), the trained LSTM model predicts a probability distribution for the *next* expected log key. If the actual log key that appears is not among the top-`k` most probable predictions (where `k` is a configurable threshold), it is flagged as an anomaly. This indicates a deviation from the learned normal execution flow.

2.  **Parameter Value Anomaly:** If a log key is deemed normal, its corresponding parameter vector is then checked. A separate LSTM model predicts the expected parameter values based on the historical sequence for that key. If the Mean Squared Error (MSE) between the predicted and observed vectors exceeds a statistically determined threshold, it is flagged as a performance anomaly.

An event is considered anomalous if either its key or its parameter vector is flagged.

### Evaluation
The authors evaluated DeepLog against other methods (PCA, Invariant Mining) using large, real-world datasets, including the **HDFS** (Hadoop Distributed File System) logs and logs from an **OpenStack** cloud environment. The evaluation demonstrated that DeepLog significantly outperformed existing methods, achieving high precision and recall (F-measure of 96% on HDFS). It proved effective at detecting various anomalies, including those caused by missing or extra log keys, and performance issues reflected in parameter values. The evaluation also highlighted its ability to work in an online, streaming fashion and adapt to new patterns via incremental updates from user feedback.

---

### Relevance to Thesis
The DeepLog methodology is highly relevant and directly applicable to the post-interaction analysis layer of the **ADLAH (Adaptive Multi-Layered Honeynet Architecture)**. Its unsupervised, sequence-based approach to anomaly detection offers a sophisticated mechanism for analyzing attacker behavior within high-interaction honeypots.

*   **Modeling Attacker Behavior:** Attacker activity within a honeypot, whether captured as a sequence of shell commands or system call traces, is inherently sequential. DeepLog's LSTM-based model can be trained on corpora of "benign" or known activity to learn normal administrative sequences. When an attacker interacts with the honeypot, their sequence of commands can be fed into the model. Deviations from the learned "normal" grammar of system interaction would be flagged as anomalies, effectively identifying malicious or novel behavioral patterns.

*   **Sophisticated Anomaly Detection:** This approach moves beyond traditional signature-based IDS, which can only detect known attack patterns, and simple statistical methods that might miss the significance of the *order* of operations. An attacker might use legitimate commands, but the sequence in which they are used (e.g., `whoami` -> `uname -a` -> `cat /etc/passwd` -> `wget ...`) constitutes a strong indicator of compromise. DeepLog is designed to detect precisely these kinds of sequential anomalies, which are characteristic of evolving attack strategies that traditional methods would miss.

*   **High-Fidelity Signal for Reinforcement Learning:** The detection of an anomalous sequence of actions by a DeepLog-inspired model can serve as a high-fidelity signal for the ADLAH's Reinforcement Learning (RL) agent. When the model flags a previously unseen, low-probability sequence of commands, it strongly indicates a novel or sophisticated threat that has successfully bypassed lower-level defenses. This signal provides the RL agent with crucial, context-rich information, enabling it to trigger an adaptive response—such as modifying the honeynet configuration, migrating the attacker to a more instrumented environment, or terminating the session—based on the inferred novelty and potential risk of the attacker's behavior.