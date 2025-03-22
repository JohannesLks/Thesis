
# Theoretical Framework: Mathematical Formulation

## Problem Formalization

We consider a set of honeypot log lines:  
$$
L = \{l_1, l_2, \dots, l_n\}
$$

Grouped into sessions:  
$$
S = \{s_1, s_2, \dots, s_m\}
$$

Each session $s_i$ is a time-ordered sequence of log lines.  
Each log line $l_i$ is mapped to an embedding vector:  
$$
\mathbf{e}_{l_i} \in \mathbb{R}^d
$$

The session-level embedding $\mathbf{e}_{s_i}$ is derived from the final hidden state of an LSTM processing all log embeddings within that session.

---

## Cross-Session Anomaly Cohesion Score (CSACS)

We define the CSACS for a candidate cluster $C$ of sessions as:
$$
\text{CSACS}(C) = \frac{ \sum_{(s_i, s_j) \in E_C} \left( \lambda_1 \cdot \frac{1}{D(s_i, s_j) + \epsilon} + \lambda_2 \cdot \frac{1}{T(s_i, s_j) + \delta} \right) \cdot \min(A(s_i), A(s_j)) }{ |E_C| }
$$

Where:  
- $E_C$ are edges between sessions in cluster $C$  
- $D(s_i, s_j)$ is the distance between session embeddings  
- $T(s_i, s_j)$ is the temporal gap between sessions  
- $A(s_i)$ is the anomaly score of session $s_i$  
- $\lambda_1, \lambda_2$ are weighting parameters  
- $\epsilon, \delta$ are smoothing constants

---

## Optimization Problem Formulation

We formulate attack chain extraction as:
$$
\max_{C} \text{CSACS}(C) - \gamma \cdot |C|
$$

Where $\gamma$ is a regularization parameter.

---

## Stability Theorem

**Theorem (Cluster Stability under Noise):**  
Let $C$ be a cluster detected by maximizing CSACS with threshold $\theta$. Under assumptions of Gaussian noise and uniform temporal distribution, the probability that a random cluster achieves $\text{CSACS}(C) \geq \theta$ decreases exponentially with the number of edges:
$$
P(\text{CSACS}(C) \geq \theta) \leq \exp\left( -\alpha \cdot |E_C| \cdot \theta \right)
$$

Where $\alpha > 0$ is determined by noise variance properties.
