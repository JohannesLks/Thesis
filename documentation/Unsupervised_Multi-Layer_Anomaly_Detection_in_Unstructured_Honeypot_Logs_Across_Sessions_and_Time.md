# Unsupervised Multi-Layer Anomaly Detection in Unstructured Honeypot Logs Across Sessions and Time
*A Hybrid Deep Learning Framework for Modeling Complex Attacker Behavior*

## 1. Introduction

Honeypot systems are critical tools in cybersecurity research and network defense, designed to attract and record attacker behavior. These environments produce vast amounts of unstructured, noisy, and heterogeneous log data. Detecting anomalies within these logs is essential to uncover stealthy attacks, zero-day exploits, and previously unknown attack patterns.

However, existing anomaly detection methods for logs typically rely on structured log templates, assume continuous linear sequences, or operate on isolated session data. Real-world attacker behavior does not adhere to such assumptions: attackers frequently distribute their actions across multiple sessions, switch IP addresses, and exhibit complex temporal patterns that go undetected by single-level models.

This paper proposes a novel, unsupervised multi-layer anomaly detection framework that captures attacker behavior across individual log entries, sequences within sessions, and patterns that span multiple sessions over time.

---

## 2. Related Work and Research Gap

- **DeepLog (2017)** introduced LSTM-based sequence modeling for structured log templates but does not generalize to unstructured honeypot logs or multi-session activity.
- **AutoLog (2021)** uses autoencoders for entropy-based scoring of log chunks but lacks temporal modeling and cross-session awareness.
- **LogBERT (2021)** applies transformers for log sequences but requires predefined templates and does not address heterogeneous or session-spanning attacker behavior.
- Clustering approaches (e.g., UNADA) detect honeypot traffic anomalies but rely on NetFlow features and do not utilize deep learning on raw log content.

> **Research Gap**:  
To date, no unsupervised anomaly detection framework exists that:
- Operates on unstructured honeypot logs,
- Models line-level anomalies, intra-session sequences, and cross-session attacker strategies in combination,
- Provides interpretable, real-time anomaly detection.

---

## 3. Research Objectives

- Develop a hybrid deep learning model combining:
  - Autoencoders for log-line content anomaly detection,
  - LSTM-based sequential models for intra-session anomaly detection,
  - A cross-session attention mechanism to capture distributed attacker patterns.
- Design an unsupervised detection pipeline without reliance on structured templates or manual feature engineering.
- Evaluate on real honeypot datasets, enriched with synthetic attacker behaviors, demonstrating detection accuracy, false-positive rate, and scalability.
- Provide interpretability through attention heatmaps and temporal anomaly visualization.

---

## 4. Proposed Framework Architecture

```mermaid
graph TD
    AE["Line-level Autoencoder (per line)"] --> AE_Out["AE anomaly scores & embeddings"]
    AE_Out --> LSTM["Session-level LSTM (per session)"]
    LSTM --> SessEmb["Session embeddings"]
    SessEmb --> Attention["Cross-session Attention Layer"]
    AE_Out --> Fusion["Fusion Layer"]
    LSTM --> Fusion
    Attention --> Fusion
    Fusion --> Output["Unified Anomaly Score"]


