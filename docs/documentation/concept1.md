# Unsupervised Multi-Layer Anomaly Detection and Attack Chain Extraction in Unstructured Honeypot Logs
*A Hybrid Deep Learning Framework for Modeling Complex Attacker Behavior on Unlabeled Production Data*

---
This concept is created with help from Generative AI (ChatGpt o1 + 4o)
---

## 1. Related Work and Positioning
This framework builds upon and addresses gaps identified in state-of-the-art research, including:

- **An Unsupervised Deep Learning Model for Early Network Traffic Anomaly Detection (2020)**: CNN combined with autoencoders for early anomaly detection on IoT traffic, using only the first bytes of flows for fast detection.  
- **Anomaly Detection from Log Files Using Unsupervised Deep Learning (2020)**: LSTM autoencoder applied to raw, unstructured log data without preprocessing; relevant for modeling temporal rarity signals.  
- **DeepLog (2017)**: LSTM-based modeling of structured system log sequences, with incremental updates but limited to single-session and structured data.  
- **AutoLog (2021)**: Template-free deep autoencoder using entropy scoring, effective on heterogeneous system logs, but lacking relational modeling across sessions.  
- **Raw Packet Data Ingestion with Transformers (2023)**: Byte-level transformer ingestion of raw packet data; demonstrates feasibility but faces large infrastructure demands.  
- **Unsupervised Machine Learning Techniques for Network Intrusion Detection (2020)**: Comparison of PCA, Isolation Forest, One-Class SVM, and autoencoders; highlights autoencoders' superiority for zero-day detection and real-time efficiency.  
- **A Deep Learning Approach to Network Intrusion Detection (2017)**: Stacked non-symmetric autoencoders with random forest classifiers; foundational for hybrid feature extraction and classification.  
- **An LSTM-Based Deep Learning Approach for Packet-Level Detection (2020)**: Embedding of packet header fields for sequence-based anomaly detection, pointing out limitations in generalizing to payloads.  
- **UNADA (2015)**: Clustering-based unsupervised anomaly detection in honeypot traffic with automated signature generation; limited to NetFlow data.  
- **FedNIDS (2025)**: Federated supervised learning on packet data; excellent scalability, but lacks unsupervised adaptability for unknown attacks.  
- **DeepFed (2023)**: Federated, unsupervised deep anomaly detection across unlabeled data streams, relevant for future decentralized security models.  
- **Graph Neural Networks for Anomaly Detection in Dynamic Graphs (2022)**: Reviews scalability strategies and temporal GNN frameworks applicable to dynamic honeypot graph construction.  
- **Online and Adaptive Graph Construction for Anomaly Detection (2022)**: Introduces dynamic graph edge adaptation mechanisms based on anomaly scores and time decay.  
- **Explainable AI for Anomaly Detection in Cybersecurity (2023)**: Provides guidance on integrating GNN explainability techniques like GNNExplainer and interpretability for security analysts.

---

## 2. Introduction
The detection of anomalies and the reconstruction of attack chains in unstructured honeypot logs represent a complex challenge. Existing literature provides valuable inspiration but also exposes clear gaps—particularly in handling massive, unlabeled datasets and detecting multi-session or long-tail attacks. This document proposes a critically designed architecture and theoretical framework that leverages the strengths and addresses the weaknesses identified across recent state-of-the-art research.

Given the large-scale nature of production honeypot data, this paper primarily focuses on an unlabeled 500 GB dataset provided by the BSI (German Federal Office for Information Security). Its considerable size and lack of any explicit ground truth motivate a fully unsupervised approach. For further validation and partial ground-truth checks, classical intrusion datasets such as **KDD Cup 99** and **NSL-KDD** will be leveraged. This combination enables comprehensive benchmarking: the BSI dataset tests scalability and real-world complexity, while the smaller labeled sets confirm detection accuracy on known attack classes.

---

## 3. Proposed Architecture: Enhanced Conceptual Design

### 3.1 Key Innovations
(1) **Line-Level Modeling**  
   Use a lightweight distilled transformer (e.g., DistilByT5) for byte-level embeddings of log lines. To ensure scalability under high-throughput conditions, introduce a fallback denoising sequence autoencoder. This autoencoder can replicate or approximate the transformer-based embeddings in resource-constrained production environments.

(2) **Session-Level Modeling**  
   Deploy a hybrid Transformer-LSTM module with temporal attention to capture both local (sequence-based) and global (contextual) session behavior. This layer aggregates line embeddings into a coherent session representation, accounting for potential long-range dependencies and ordering.

(3) **Temporal Relationship Modeling**  
   Construct dynamic session graphs in which edges reflect semantic distance and temporal adjacency, decaying exponentially over time. This approach helps identify how different sessions are interlinked, uncovering multi-session attack sequences or correlated anomalies.

(4) **Graph Neural Network Layer**  
   Integrate a scalable GNN using subgraph sampling techniques (e.g., GraphSAGE or Cluster-GCN) to handle large, evolving graphs without overwhelming memory. This layer consolidates session embeddings and uncovers global patterns indicative of distributed or advanced persistent threats (APTs).

(5) **Fusion Layer**  
   Design an attention-based fusion mechanism that combines: 

  - **Line-level anomaly scores** (e.g., reconstruction/entropy error),  
  - **Session-level anomaly indicators**,  
  - **GNN-based cluster embeddings**.  
  
   By weighting these factors, the fusion layer seeks a comprehensive anomaly indicator reflective of local outliers and global relational structure.

(6) **Improved Attack Chain Extraction**  
   Implement a two-stage process—first, coarse temporal clustering (via DBSCAN on timestamp embeddings) to group sessions that lie close in time, followed by fine-grained community detection (e.g., Leiden algorithm) to identify subgroups with high anomaly cohesion. This helps reconstruct potentially large, multi-step attack chains spanning many sessions.

(7) **Explainability**  
   Incorporate GNNExplainer for subgraph-level interpretability and heatmaps showing the most significant lines or sessions in an anomalous cluster. By mapping the contributing features back to raw log lines, security analysts can trace suspicious activity with minimal manual overhead.

### 3.2 Updated Architecture Diagram (Conceptual Overview)
```mermaid
graph TD
  Input["Unlabeled Honeypot Logs (500 GB BSI)"] --> EmbeddingPath["Line Embedding Generation"]
  EmbeddingPath -->|Transformer (ByT5)| LineEmbeddings["Line Embeddings"]
  EmbeddingPath -->|Fallback Autoencoder| LineEmbeddingsAE["Line Embeddings (Autoencoder)"] 
  EmbeddingPath -->|Autoencoder Reconstruction Error| LineAnomalyScores["Line-Level Anomaly Scores"]
  
  LineEmbeddings --> SessionModel["Session-Level Hybrid Transformer-LSTM"]
  LineEmbeddingsAE --> SessionModel
  
  SessionModel --> SessionEmbeddings["Session Embeddings"]
  SessionEmbeddings --> TemporalGraph["Dynamic Temporal Session Graph"]
  TemporalGraph --> GNN["Scalable GNN with Subgraph Sampling"]
  
  GNN --> Fusion["Attention-Based Fusion Layer"]
  LineAnomalyScores --> Fusion
  
  Fusion --> AttackChains["Two-Stage Attack Chain Extraction"]
  AttackChains --> Output["Attack Chain Reports & Explanations"]
  Fusion -->|Feedback Loop| TemporalGraph
  AttackChains -->|Continuous Update| SessionModel

```

---

## 4. Mathematical Background

### 4.1 Problem Setup
- **Honeypot Log Lines**  
  $$
    L = \{l_1, l_2, \dots, l_n\}
  $$  
- **Grouped into Sessions**  
  $$
    S = \{s_1, s_2, \dots, s_m\}
  $$  
- **Per-Line Embeddings**  
  $$
    e_{l_i} \in \mathbb{R}^d
  $$  
  Each log line is mapped to a continuous vector representation.  
- **Session Embeddings**  
  Derived from the final state of the Transformer-LSTM stack:  
  $$
    e_{s_i} = \text{TransformerLSTM}(s_i)
  $$

### 4.2 Cross-Session Anomaly Cohesion Score (CSACS)
To quantify how anomalous and tightly coupled a community \( C \) of sessions is, we propose:

$$
  CSACS(C) = \frac{ \sum_{(s_i, s_j) \in E_C} \left( \lambda_1 \cdot \frac{1}{D(s_i, s_j) + \epsilon} + \lambda_2 \cdot \frac{1}{T(s_i, s_j) + \delta} \right) \cdot \min(A(s_i), A(s_j)) }{ |E_C| }
$$

**Where**:

- \( E_C \): set of edges among sessions in community \( C \)  
- \( D(s_i, s_j) \): distance between session embeddings \( e_{s_i} \) and \( e_{s_j} \)  
- \( T(s_i, s_j) \): temporal gap between sessions  
- \( A(s_i) \): anomaly score of session \( s_i \)  
- \( \lambda_1, \lambda_2 \): weighting parameters  
- \( \epsilon, \delta \): small constants for numerical stability  

### 4.3 Optimization Objective
$$
  \max_{C} \; CSACS(C) \;-\; \gamma \cdot |C|
$$
Balancing cluster compactness and size through a regularization term \(\gamma\).

### 4.4 Hypothetical Stability Theorem (Conceptual Proposal)
> **Theorem:** Under Gaussian noise and uniform temporal distribution assumptions, the probability that a random cluster exceeds threshold \(\theta\) decays exponentially with the number of edges:  
> $$
>   P(CSACS(C) \geq \theta) \;\leq\; \exp\Bigl(-\alpha \cdot |E_C| \cdot \theta\Bigr),
> $$  
> where \(\alpha > 0\) depends on the noise variance.  
> **Note:** A full proof is out of scope for this paper, but future work will involve both theoretical and empirical analysis to validate or refine this statement.

---

## 5. Data Strategy, Implementation, and Evaluation Plan

### 5.1 Datasets

1. **Unlabeled 500 GB BSI Honeypot Dataset**  

    - Large-scale, real-world, unstructured data.  
    - Ideal for testing scalability and unsupervised capabilities on production-level volumes.  
    - We will measure detection coverage, throughput (logs/sec), and resource usage (CPU/GPU memory) in near real-time pipelines.

2. **KDD Cup 99 / NSL-KDD**  

    - Classic labeled intrusion datasets, albeit somewhat dated.  
    - Useful for establishing performance baselines (Precision, Recall, F1).  
    - Provides partial ground truth and confirms whether the system can detect well-known attacks.

### 5.2 Ablation Studies and Comparative Experiments
We propose **five** key ablations to rigorously test each major architectural component:

1. **Transformer vs. Autoencoder (Line-Level Embedding)**  

    - **Hypothesis:** Byte-level transformer embeddings yield more nuanced semantic features but require more computation.  
    - **Metrics:** Reconstruction error, classification metrics (where labels exist), ingestion throughput.

2. **Full Session Model vs. No Session Model**  

    - **Setup:** Compare a pipeline that (a) aggregates log lines into sessions vs. (b) treats every line independently before the GNN.  
    - **Aim:** Check if ignoring session context severely diminishes anomaly detection for multi-step attacks.

3. **Static vs. Dynamic Graph Construction**  

    - **Variant A:** Build a static graph for a fixed time window.  
    - **Variant B:** Continuously update edges with an exponential time decay.  
    - **Goal:** Evaluate whether dynamic adjacency provides better chain detection for persistent or slow-moving attacks.

4. **GNN vs. Simpler Aggregator**  

    - **Comparison:** Replace the GNN with a basic aggregator (e.g., an MLP on session embeddings or a clustering method like k-Means).  
    - **Question:** Is the complexity of a GNN justified by significantly better correlation of anomalies across sessions?

5. **Fusion vs. Single Anomaly Score**  

    - **Approach:** Compare using a single anomaly score (e.g., from the session model only) vs. combining line-level, session-level, and GNN-based signals in the fusion layer.  
    - **Objective:** Demonstrate whether multi-level fusion substantially improves detection robustness and reduces false positives.

### 5.3 Real-Time Constraints and Overhead Mitigation
- **Subgraph Sampling & Mini-batch Training**: Techniques like GraphSAGE or Cluster-GCN prevent memory overflow by processing only portions of the graph at each step.  
- **Fallback Autoencoder**: In high-throughput or hardware-limited scenarios, switch from transformer embeddings to a lighter sequence autoencoder.  
- **Asynchronous Pipelines & Approximate Queries**: Use approximate nearest neighbor (ANN) for faster edge construction among sessions and decouple embedding computation from graph updates for better parallelization.

### 5.4 Explainability Demonstration
- **Case Studies on BSI Dataset**: Highlight suspicious clusters identified by the system, visualizing subgraphs with GNNExplainer.  
- **Line-Level Heatmaps**: Provide interpretability for local anomalies, showing which tokens or segments in a log line contributed most to the anomaly score.  
- **Comparison to Baselines**: Methods like Isolation Forest or simpler autoencoders will be used to illustrate how deeper embedding and graph-based context capture more complex attack patterns.

---

## 6. Extended Critical Analysis and Validation Outlook

### 6.1 Validation on Unlabeled Data
The BSI dataset poses a classic challenge: massive volume with no explicit labels. To address the validation gap:

1. **Spot Checks by Analysts**  

    - Manually inspect a small subset of flagged sessions for confirmatory evidence of malicious or benign behavior.  
    - This yields some real “ground truth” points to estimate false positive vs. true positive rates.

2. **Threat Intelligence Feeds**  

    - Cross-reference sessions against known malicious IPs, domains, or other IOCs.  
    - Although this only detects attacks already known, it at least provides partial labeling or confidence checks.

3. **Synthetic Injection Tests** 
 
    - Inject known artificial anomalies or replay small slices of labeled attacks into the BSI data stream.  
    - Assess how well the pipeline detects these injects within large volumes of normal honeypot logs.

### 6.2 Theoretical Simulation for the Hypothetical Stability Theorem
To provide preliminary evidence for the theorem’s relevance:

1. **Synthetic Clustering Experiments**  

    - Generate data with controlled noise and inject “attack-like” clusters.  
    - Measure how frequently random subsets exceed CSACS thresholds, correlating results with the proposed exponential bound.

2. **Real-Data Subset Analysis**  

    - Partition BSI logs into smaller subsets and track how anomaly clusters form.  
    - Estimate whether random groupings seldom achieve high CSACS values, especially as the number of edges grows.

3. **Varying Noise Levels**  

    - Experiment with different noise intensities or background traffic patterns to see if the exponential decay in cluster false alarms holds across multiple scenarios.

### 6.3 Practical and Resource Considerations
- **Computational Footprint**  

  - The pipeline’s complexity (Transformer + LSTM + GNN) requires robust hardware.  
  - Fallback autoencoders can mitigate overhead, but real-time streaming at 500 GB scale demands either distributed systems or batching strategies.

- **Ablation vs. Real-World Constraints**  

  - Thorough ablation is valuable for academic demonstration. However, real deployments might choose simpler configurations (e.g., direct autoencoder + GNN) if hardware budgets are tight.

---

## 7. Refined Limitations and Future Work
1. **Hyperparameter Tuning**  

    - Selecting thresholds (\(\lambda_1, \lambda_2, \gamma\)), graph pruning levels, and fusion weights requires iterative experimentation with domain feedback.

2. **Irregular Temporal Patterns**  

    - Attacks may have dormant phases or irregular intervals. More adaptive time windows or advanced temporal modeling (e.g., gating mechanisms) could help.

3. **Large-Scale Real-Time** 

    - True real-time processing at sustained ingestion rates remains challenging. Distributed graph-building and streaming-based GNN updates could be the next milestone.

4. **Partial Labeling & Ground Truth**  

    - While the BSI dataset is unlabeled, curated subsets (possibly with known malicious IPs or events) could anchor a semi-supervised approach, boosting reliability.

5. **Federated and Adaptive Extensions**
  
    - Inspired by *FedNIDS (2025)* and *DeepFed (2023)*, the proposed methods could be adapted for federated learning, allowing multiple honeypot locations to collaborate without sharing raw logs.

---
