
# Theoretical Framework: Mathematical Formulation (Conceptual Proposal)

## Problem Formalization (Proposed Idea)

As part of this thesis, I propose a theoretical framework to describe anomaly detection and attack chain extraction in unstructured honeypot logs. The following mathematical formulations are intended to strengthen the theoretical rigor of the paper, though they are not yet tested or validated and should be viewed critically as conceptual building blocks for future work.

We consider a set of honeypot log lines:
$$
L = \{l_1, l_2, \dots, l_n\}
$$

Grouped into sessions:
$$
S = \{s_1, s_2, \dots, s_m\}
$$

Each session $s_i$ is modeled as a time-ordered sequence of log lines. Each log line $l_i$ is mapped to an embedding vector:
$$
\mathbf{e}_{l_i} \in \mathbb{R}^d
$$

The session-level embedding $\mathbf{e}_{s_i}$ is defined as the final hidden state of an LSTM that processes the sequence of embeddings for that session.

---

## Cross-Session Anomaly Cohesion Score (CSACS) — Conceptual Formulation

To theoretically capture the cohesion of anomalies across sessions, I propose the following formula for a cross-session anomaly cohesion score (CSACS), which is meant to express the relationship between temporal proximity, embedding similarity, and anomaly strength within clusters:
$$
\text{CSACS}(C) = \frac{ \sum_{(s_i, s_j) \in E_C} \left( \lambda_1 \cdot \frac{1}{D(s_i, s_j) + \epsilon} + \lambda_2 \cdot \frac{1}{T(s_i, s_j) + \delta} \right) \cdot \min(A(s_i), A(s_j)) }{ |E_C| }
$$

Where:  
- $E_C$ is the set of edges between sessions in candidate cluster $C$  
- $D(s_i, s_j)$ represents the embedding distance between sessions  
- $T(s_i, s_j)$ is the temporal gap between sessions  
- $A(s_i)$ is the anomaly score assigned to session $s_i$  
- $\lambda_1, \lambda_2$ are weighting parameters that would need to be empirically tuned  
- $\epsilon, \delta$ are small smoothing constants to avoid division by zero  

> **Critical Note:**  
> This score is currently a theoretical construct. It has not been validated, and parameter weighting would require future optimization. The functional form may require adjustment after empirical testing.

---

## Optimization Problem Formulation (Proposed)

The attack chain extraction process could be framed as the following optimization problem:
$$
\max_{C} \text{CSACS}(C) - \gamma \cdot |C|
$$

Where $\gamma$ is a regularization term to penalize overly large or trivial clusters.  

> **Critical Reflection:**  
> This optimization objective is purely theoretical at this stage. It does not account for noise sensitivity, scalability constraints, or potential bias introduced by overly strong temporal weighting.

---

## Hypothetical Stability Theorem (to be explored)

**Proposed Theorem (Cluster Stability under Noise):**  
Let $C$ be a cluster determined by maximizing the CSACS score with a given threshold $\theta$. Under the assumption of Gaussian noise and uniform temporal distribution, the probability that a random cluster achieves $\text{CSACS}(C) \geq \theta$ decreases exponentially with the number of edges:
$$
P(\text{CSACS}(C) \geq \theta) \leq \exp\left( -\alpha \cdot |E_C| \cdot \theta \right)
$$

Where $\alpha > 0$ is determined by noise variance properties.  

> **Disclaimer:** This theorem is presented as a conceptual idea only and is not mathematically proven or validated in this work. It is intended as a theoretical proposal for future investigation.
