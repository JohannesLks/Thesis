
# Unsupervised Multi-Layer Anomaly Detection and Attack Chain Extraction in Unstructured Honeypot Logs  
*A Hybrid Deep Learning Framework for Modeling Complex Attacker Behavior on Unlabeled Production Data*  

## 1. Introduction  

Honeypot systems are widely deployed in cybersecurity research and defense operations to attract and record attacker behavior in controlled environments. These systems generate large volumes of highly unstructured, noisy, and heterogeneous log data. Detecting anomalies within these logs is crucial for identifying stealthy or previously unknown attacks. However, this task becomes significantly more complex when working with unlabeled production datasets, such as the one provided by the German Federal Office for Information Security (BSI), where no predefined ground truth or attack annotations are available.  

This paper presents a novel unsupervised framework designed to not only detect anomalous behavior in unstructured honeypot logs but also to autonomously reconstruct potential attack chains by grouping related sessions and log entries. Our approach addresses both detection and interpretability challenges in environments where labeling is impractical or impossible.  

## 2. Related Work and Research Gap  

- **DeepLog (2017)** models log sequences with LSTMs but relies on structured templates and does not scale to unstructured or cross-session patterns.  
- **AutoLog (2021)** uses autoencoders for anomaly detection but ignores temporal and cross-session relationships.  
- **LogBERT (2021)** applies transformer-based sequence modeling but depends on predefined templates and does not handle multi-session attacker behavior.  
- Clustering-based methods detect attack patterns but focus on static flow-level metrics and do not leverage deep embeddings or time-based behavior modeling.  

> **Identified Research Gap:**  
> To date, no unsupervised framework exists that:  
> - Operates on unstructured, unlabeled honeypot log data,  
> - Models attacker behavior across line-level, session-level, and cross-session layers,  
> - Incorporates temporal modeling across multiple sessions to detect distributed attacks,  
> - And extracts coherent attack chains from anomaly signals without ground-truth annotations.

## 3. Research Objectives  

- Develop a hybrid deep learning framework that integrates:  
    - Line-level autoencoder for anomaly detection in individual log entries,  
    - Session-level sequential modeling using LSTM to detect abnormal behavior patterns within sessions,  
    - Time-based LSTM for modeling sequential patterns across multiple sessions over time,  
    - A cross-session attention layer to highlight suspicious aggregated patterns.  

- Address the absence of labeled data by:  
    - Operating fully unsupervised,  
    - Using anomaly scores and embeddings to autonomously cluster sessions and reconstruct attack chains.  

- Enable explainability and analyst support by:  
    - Providing log line-level anomaly scores,  
    - Visualizing attention weights across sessions and time,  
    - Outputting clustered groups of logs and sessions that constitute suspected attack chains.
 

## 4. Proposed Framework Architecture  

```mermaid
graph TD
    Input["Unlabeled Honeypot Log Data"] --> AE["Line-level Autoencoder (per line)"]
    AE --> AE_Out["AE anomaly scores & embeddings"]
    AE_Out --> LSTM["Session-level LSTM (per session)"]
    LSTM --> SessEmb["Session embeddings"]
    SessEmb --> TimeLSTM["Time-based LSTM (sequence of session embeddings)"]
    TimeLSTM --> Attention["Cross-session Attention Layer"]
    AE_Out --> Fusion["Fusion Layer"]
    LSTM --> Fusion
    Attention --> Fusion
    Fusion --> Chains["Attack Chain Extraction & Clustering"]
    Chains --> Output["Unified Anomaly Score & Attack Chain Reports"]
```

### Key Components:
- **Line-level Autoencoder:** Detects content anomalies in individual log lines.  
- **Session-level LSTM:** Captures sequential behavior anomalies within each session.  
- **Time-based LSTM:** Models ordered sequences of session embeddings over time to detect distributed attack chains.  
- **Cross-session Attention Layer:** Highlights aggregated suspicious behaviors across sessions and time.  
- **Fusion Layer:** Combines anomaly signals from line-level, session-level, and time-level.  
- **Attack Chain Extraction:** Clusters session groups forming coherent attack chains.  

## 5. Future Work: Toward Adaptive Honeypots  

In the future, detected attack chains and time-based anomalies could be used to make honeypot systems adaptive. Dynamic configuration adjustments might include:  
- Extending session timeouts,  
- Changing simulated system fingerprints or banners,  
- Creating decoy assets that provoke attacker responses.  

A reinforcement learning loop could optimize these adjustments based on the novelty and richness of attacker interactions, guided by embeddings and time-based anomaly detection signals.

## 6. Conclusion  

The proposed framework extends beyond single-session analysis and incorporates time-based LSTM modeling to capture attacker activity patterns across sessions and over time. Combined with unsupervised clustering, this enables autonomous attack chain reconstruction and lays the groundwork for future adaptive honeypot behavior.
