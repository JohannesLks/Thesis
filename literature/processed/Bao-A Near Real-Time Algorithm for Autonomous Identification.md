# Summary of "A Near Real-Time Algorithm for Autonomous Identification and Characterization of Honeypot Attacks"

**Authors:** Philippe Owezarski (misidentified as Bao in the prompt, the author is Owezarski et al.)

**Source:** ASIA CCS’15, April 14–17, 2015, Singapore.

## 1. Core Contribution

The paper presents **UNADA (Unsupervised Network Anomaly Detection Algorithm)**, a framework designed for the autonomous, near real-time identification and characterization of attacks captured by honeypots. Its primary contribution is a completely unsupervised methodology that requires no a-priori knowledge, such as attack signatures, labeled training data, or human intervention. This positions UNADA as a significant step towards fully autonomous security systems that can adapt to the evolving threat landscape. The algorithm not only identifies attacks but also automatically generates easy-to-interpret filtering rules that can be used to configure defense mechanisms like firewalls and routers.

## 2. Methodology: The UNADA Algorithm

UNADA operates on network traffic data (specifically NetFlows from a university honeypot network) and employs a multi-level, multi-stage process to isolate and characterize illicit activities. The core assumption is that all traffic to a honeypot is illegitimate, simplifying the problem from "detecting anomalies" to "classifying different types of malicious traffic."

The methodology can be broken down into the following key stages:

### a. Multi-Level Traffic Aggregation & Sub-Space Clustering
- **Traffic Aggregation:** The algorithm first aggregates traffic flows at nine different levels of granularity (e.g., by source IP, destination IP, source/destination network prefixes like /24, /16, /8). This allows it to detect a wide range of attacks, from single-source DoS to large-scale distributed attacks (DDoS).
- **Sub-Space Clustering (SSC):** Instead of clustering data in a high-dimensional feature space (which is prone to noise and the "curse of dimensionality"), UNADA performs clustering on numerous low-dimensional (2D) sub-spaces. Each sub-space is a combination of two traffic features out of a possible nine (e.g., `packet rate`, `fraction of SYN packets`, `number of source ports`). This approach is more robust, faster, and better at finding clusters in sparse data.

### b. Combining Clustering Results (Evidence Accumulation)
- **Problem:** Running clustering on many sub-spaces yields multiple, independent partitions of the data. The challenge is to combine this evidence into a single, cohesive result.
- **Solution:** The paper proposes two methods:
    1.  **Evidence Accumulation (EA):** Builds a similarity matrix between all flow pairs based on how frequently they are clustered together in the various sub-spaces.
    2.  **Inter-Clustering Result Association (ICRA):** A more robust method that measures similarity between the *clusters* themselves (not the individual flows) across different sub-spaces. It uses a graph-based approach to find cliques of highly similar clusters, effectively grouping different views of the same underlying attack.

### c. Correlation of Anomalous Classes
- To consolidate findings, the algorithm correlates the identified traffic classes across different aggregation levels and time windows. It uses a similarity function based on shared source/destination IP addresses and temporal proximity to ensure that long-separated events are not incorrectly merged. This allows it to build a complete picture of a single, complex attack that might manifest differently at various aggregation levels.

### d. Automatic Characterization and Rule Generation
- Once anomalous traffic classes are confidently identified, UNADA automatically generates a "signature" for each one.
- It identifies the most discriminant features from the sub-space analysis and creates a set of filtering rules (e.g., `nSyn/nPkts == 1`, `nDiffDestAddr < 9`). These rules are compact, human-readable, and can be directly implemented in security appliances.

## 3. Results and Performance

- The algorithm was evaluated on real honeypot traffic from the University of Maryland.
- It was shown to successfully identify and characterize various attacks, including different types of DoS (SYN floods, RST attacks) and more complex TCP flooding.
- In a comparative evaluation (ROC curve analysis), UNADA significantly outperformed other unsupervised methods like standard DBSCAN, k-means, and PCA-based outlier detection, achieving a much higher True Positive Rate (TPR) for a given False Positive Rate (FPR).
- The paper also highlights the algorithm's suitability for near real-time operation, as the sub-space clustering approach is highly parallelizable, allowing computational time to be drastically reduced on multi-core systems.

---

### Relevance to Thesis

This paper is highly relevant to the ADLAH (An Adaptive Multi-Layered Honeynet Architecture for Deception-based Cyber Threat Intelligence) thesis, as it provides a foundational example of an **unsupervised, autonomous system for analyzing honeypot data**—a core theme of the thesis. While ADLAH proposes a Reinforcement Learning (RL) approach for orchestration, Owezarski's work on UNADA serves as a perfect counterpart from the domain of classical Machine Learning, enriching the "Related Work" section.

Specifically, its relevance can be articulated as follows:

1.  **Alignment with ADLAH's Core Goals:** The central goal of UNADA—to autonomously identify, characterize, and generate countermeasures for attacks from honeypot data without human intervention—is directly aligned with the objectives of the ADLAH framework. It provides a concrete, non-RL-based example of how such autonomy can be achieved.

2.  **Unsupervised Learning Paradigm:** UNADA's strictly unsupervised nature is its most critical link to ADLAH. The thesis argues for adaptive systems that do not rely on pre-existing attack signatures. UNADA exemplifies this principle using a statistical, clustering-based approach, which contrasts with ADLAH's experience-driven, trial-and-error RL paradigm. This contrast is valuable for highlighting the different philosophical approaches to achieving autonomy in cybersecurity.

3.  **Classical ML Counterpart to RL Orchestration:** The thesis positions ADLAH as a novel approach using RL for dynamic defense orchestration. UNADA serves as a state-of-the-art example of a *non-RL* autonomous system. Its methodology—**sub-space clustering** combined with evidence accumulation—is a classical, data-centric ML technique. This allows the thesis to frame ADLAH's contribution more clearly: moving beyond static analysis of captured data (like UNADA) towards a dynamic, policy-based agent that learns to configure the honeynet environment itself in response to attacker interactions.

4.  **Justification for Multi-Layered Analysis:** UNADA's use of multi-level traffic aggregation validates ADLAH's multi-layered architectural concept. UNADA demonstrates that analyzing data at different granularities (IP, /24, /16) is essential for detecting a diverse range of attacks. This finding supports ADLAH's premise that an effective defense architecture must also operate and adapt across multiple layers of the kill chain and network topology.

In summary, this paper acts as a cornerstone of the related work, illustrating a powerful, alternative method for achieving autonomous honeypot analysis. It helps to contextualize ADLAH's contribution by showcasing what is possible with classical unsupervised ML and providing a clear point of departure for the thesis's proposed RL-based orchestration model.