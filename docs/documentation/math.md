## 4. Mathematical Background  

### 4.1 Problem Setup  

We consider honeypot log data consisting of a set of log lines:
$$
L = \{l_1, l_2, \dots, l_n\}
$$  
These lines are grouped into sessions:
$$
S = \{s_1, s_2, \dots, s_m\}
$$  
Each log line is embedded into a continuous vector space:
$$
e_{l_i} \in \mathbb{R}^d
$$  
A session embedding is derived via a Transformer-LSTM encoder:
$$
e_{s_i} = \text{TransformerLSTM}(s_i)
$$  

---

### 4.2 Cross-Session Anomaly Cohesion Score (CSACS)  

We define the **Cross-Session Anomaly Cohesion Score (CSACS)** for a community \( C \subseteq S \) as:
$$
CSACS(C) = \frac{ \sum_{(s_i, s_j) \in E_C} \left( \lambda_1 \cdot \frac{1}{D(s_i, s_j) + \epsilon} + \lambda_2 \cdot \frac{1}{T(s_i, s_j) + \delta} \right) \cdot \min(A(s_i), A(s_j)) }{ |E_C| }
$$  
Where:  
- \(E_C\) is the edge set induced by \(C.\)  
- \(D(s_i, s_j)\) is the Euclidean distance between session embeddings.  
- \(T(s_i, s_j)\) is the temporal gap between sessions.  
- \(A(s_i)\) is the anomaly score of session \(s_i.\)  
- \(\lambda_1, \lambda_2, \epsilon, \delta > 0\) are weighting and stability parameters.  

---

### 4.3 Optimization Objective  

The objective in identifying clusters is to maximize:
$$
\max_{C} CSACS(C) - \gamma \cdot |C|
$$
with \(\gamma > 0\) serving as a size penalty to avoid trivial large clusters.  

---

### 4.4 Statistical Stability Under a Null Model  

#### 4.4.1 Null Model Setup  

We analyze the probability that a randomly formed cluster achieves high cohesion under a null hypothesis in which no structured anomaly exists. We assume:  

- **Embedding noise model:**  
$$
e_{s_i} = \mu_{s_i} + \eta_{s_i}, \quad \eta_{s_i} \sim \mathcal{N}(0, \sigma^2 I_d)
$$  
with independent noise \(\eta_{s_i}.\)  

- **Temporal gaps:**  
Independent random variables with density \(f_T(t)\) bounded by \(\frac{1}{T_{\max}}\) on \([0, T_{\max}]\).  

- **Anomaly scores:**  
Independent, bounded random variables in \([0, A_{\max}]\), independent of embeddings and temporal gaps.  

- **Clusters:**  
Community \(C\) and edge set \(E_C\) are selected independently of all random variables described.  

> **Interpretation:** This null model forms the baseline scenario against which discovered clusters can be tested. High cohesion under this model is highly unlikely; observed deviations indicate structure.  

---

#### 4.4.2 Lemma: Dominance of Exponential Tails  

**Lemma 1 (Tail Dominance):**  
Let \(X = \left( \lambda_1 \cdot \frac{1}{D + \epsilon} + \lambda_2 \cdot \frac{1}{T + \delta} \right) \cdot A,\) where:  
- \(D^2 \sim 2 \sigma^2 \chi^2_d,\)  
- \(T \sim f_T\) bounded by \(\frac{1}{T_{\max}}\),  
- \(A \in [0, A_{\max}]\) is independent.  

Then \(X\) is sub-exponential with exponential tail decay dominated by the distance inversion term. In particular, there exist constants \(c_1, c_2 > 0\) such that for large \(x\):
$$
P(X > x) \leq c_1 \exp(-c_2 x^2)
$$  

**Proof Sketch:**  
- Distance tail deviations decay exponentially in \(d.\)  
- Temporal term is polynomially decaying but multiplied by a random bounded variable and added to an exponentially decaying component.  
- By known results in convolution of distributions (Boucheron–Lugosi–Massart, Lemma 2.7), exponential tails dominate.  

---

#### 4.4.3 Theorem: Statistical Stability of CSACS  

**Theorem (Statistical Stability Under the Null):**  
Under the null model described, let \(C\) be a random community with \(|E_C|\) edges. Suppose \(\epsilon > c_1 \sigma,\ \delta > c_2 / T_{\max}.\) Then for any threshold \(\theta > 0,\)
$$
P\bigl(CSACS(C) \geq \theta \bigr) \leq \exp\bigl(- \alpha \cdot |E_C| \cdot \theta^2 \bigr),
$$
where \(\alpha > 0\) depends on \(\sigma^2, d, \lambda_1, \lambda_2, A_{\max}, c_1, c_2, T_{\max}.\)  

> **Corollary:** As dimension \(d\) increases, \(\alpha\) scales proportionally with \(d,\) reinforcing stability in high-dimensional embeddings.  

---

#### 4.4.4 Proof (Outline)  

1. **Individual edge contributions** follow sub-exponential tails due to Lemma 1.  
2. **Sums of independent sub-exponential random variables** concentrate according to Vershynin (2018), Theorem 2.7.1:
$$
P\left( \sum_{(i,j) \in E_C} X_{(i,j)} \geq |E_C| \theta \right) \leq \exp\left(-c |E_C| \cdot \min\left(\frac{\theta^2}{v^2}, \frac{\theta}{M}\right) \right)
$$  
where \(v^2\) is total variance and \(M\) bounds the tails.  
3. All constants can be expressed in terms of \(\sigma, d, A_{\max}, T_{\max}, \lambda_1, \lambda_2.\)  

---

#### 4.4.5 Explicit Constant  
An explicit lower bound on \(\alpha\) is given by:
$$
\alpha = c \cdot \min \left( \frac{d \lambda_1^2}{\sigma^2}, \frac{\lambda_2^2 \delta^2}{T_{\max}^2}, \frac{1}{A_{\max}^2} \right)
$$
with universal constant \(c > 0.\)  

---

#### 4.4.6 Hypothesis Testing Interpretation  

> The stability theorem provides a null hypothesis threshold: if an observed cluster exceeds
$$
CSACS(C) > \theta_{\text{crit}}
$$
with \(\theta_{\text{crit}}\) chosen such that
$$
\exp\left(- \alpha \cdot |E_C| \cdot \theta_{\text{crit}}^2 \right) < \delta
$$
(for some desired significance level \(\delta\)), we can reject the null hypothesis that the cluster is random.  

> This reframes CSACS detection as a hypothesis testing procedure rather than relying on strong assumptions about the clustering algorithm.  

---

#### 4.4.7 Search Space Correction  

If the clustering algorithm searches over a candidate set \(\mathcal{C}\) of communities with \(|\mathcal{C}|\) polynomial in \(m,\) a union bound corrects for search:
$$
P\left( \exists C \in \mathcal{C}: CSACS(C) \geq \theta \right) \leq |\mathcal{C}| \cdot \exp\bigl(- \alpha \cdot |E_C| \cdot \theta^2 \bigr)
$$
For polynomial-sized \(|\mathcal{C}|\), the exponential decay dominates for large thresholds.  

---

#### 4.4.8 Limitations and Future Work  

- The result applies under independence assumptions. In practice, embeddings are correlated outputs of deep models.  
- Extending stability results to dependent embeddings and data-conditioned cluster search remains open. This will likely require bounding the complexity of clustering algorithms (e.g., via VC dimension or covering number arguments).  
- Heavier-tailed temporal distributions and bursty behavior are not covered here; extending to sub-exponential temporal gap distributions is part of ongoing theoretical work.  

---

## References  

- Vershynin, R. (2018). *High-Dimensional Probability: An Introduction with Applications in Data Science.* Cambridge University Press.  
- Boucheron, S., Lugosi, G., & Massart, P. (2013). *Concentration Inequalities: A Nonasymptotic Theory of Independence.* Oxford University Press.  
