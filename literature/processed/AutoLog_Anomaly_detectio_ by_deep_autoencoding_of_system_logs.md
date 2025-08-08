# Summary of "AutoLog: Anomaly detection by deep autoencoding of system logs"

**Authors:** Marta Catillo, Antonio Pecchia, Umberto Villano
**Publication:** Expert Systems With Applications, Vol. 191, 2022

## 1. Introduction & Motivation

The paper introduces **AutoLog**, an approach for detecting anomalies in system logs. The authors highlight a significant challenge in modern system administration: while logs are a rich source of information for troubleshooting and security, their analysis is often difficult. Current tools like SIEMs (Security Information and Event Management) heavily rely on predefined rules, signatures, and standardized log formats. This dependency makes them less effective for proprietary, industrial, or highly customized systems where such standards and pre-existing threat knowledge are lacking.

The core motivation for AutoLog is to provide a generalized, robust anomaly detection solution that:
*   Does not depend on the specific structure or format of the log files.
*   Does not require prior knowledge of anomalies (i.e., no need for labeled anomaly data for training).
*   Can handle heterogeneous and distributed log sources.

## 2. The AutoLog Approach

AutoLog employs a semi-supervised learning methodology based on a deep autoencoder. The process is divided into two main components: a **Scoring Component** and an **Anomaly Detector**.

### 2.1. Scoring Component

This component transforms raw, unstructured text logs into numerical feature vectors.
1.  **Sampling:** Logs from various sources (**Logging Entities**) are sampled into fixed-time windows called "chunks". This is analogous to micro-batch processing.
2.  **Parsing:** Variable parts of log messages (like specific IPs, timestamps, process IDs) are removed to retain the constant, template-like parts.
3.  **Term Weighting:** The log lines within a chunk are tokenized into terms. A numerical score is computed for each chunk using a **logarithmic entropy** weighting scheme. This score is calculated based on the frequency of terms within the current chunk compared to their frequency across a baseline database of "normative" (normal operation) chunks.
4.  **Vector Creation:** The scores from all logging entities for a given time period are aggregated into a single numerical vector.

The score quantifies how much a given chunk deviates from the established norm. A high score indicates that the chunk contains rare or unusual terms, suggesting a potential anomaly.

### 2.2. Anomaly Detection using a Deep Autoencoder

The core of the detection mechanism is a **deep autoencoder**, a type of neural network trained to reconstruct its input.
*   **Training (Semi-Supervised):** The autoencoder is trained **only on vectors of scores from normative operations**. This forces the model to learn the latent patterns and structure of what constitutes "normal" system behavior.
*   **Detection:** When a new vector of scores is fed into the trained autoencoder, the model attempts to reconstruct it.
    *   If the input vector represents **normal** behavior (similar to what it was trained on), the reconstruction error will be **low**.
    *   If the input vector represents an **anomaly**, the model will struggle to reconstruct it accurately, resulting in a **high reconstruction error**.
*   **Thresholding:** A predefined **anomaly threshold** is applied to the reconstruction error. If the error exceeds this threshold, the input is classified as an anomaly. The threshold is determined by taking the 90th percentile of reconstruction errors on a validation set of normative data.

## 3. Experimental Evaluation

AutoLog was evaluated on four diverse systems:
1.  A proprietary **industrial system** from the transportation domain.
2.  A **microservices-based system** (Clearwater IMS).
3.  A **BG/L supercomputer** (a public benchmark dataset with spontaneous failures).
4.  A **Hadoop cluster** (a public benchmark dataset with service failures).

### Findings:

*   **High Performance:** AutoLog demonstrated excellent performance, with recall ranging from 0.96 to 0.99 and precision between 0.93 and 0.98 across all datasets.
*   **Superior to Other Methods:** It was compared against several other techniques, including Isolation Forest, One-Class SVM, vanilla autoencoder, and variational autoencoder. AutoLog consistently outperformed or was highly competitive with these methods, showing its robustness across different environments.
*   **Competitive with Supervised Methods:** Notably, its performance was on par with a supervised Decision Tree classifier, despite the significant advantage of not requiring anomaly examples for training.
*   **State-of-the-Art Comparison:** On the public BG/L and Hadoop benchmarks, AutoLog's F1-score was shown to be highly competitive with other state-of-the-art deep learning methods like DeepLog and LogAnomaly.

The authors also showed that simpler techniques like PCA and clustering were ineffective on these datasets due to the high degree of intertwinement between normal and anomalous data points in the feature space, underscoring the need for a non-linear approach like the one AutoLog provides.

## 4. Relevance to Thesis

The concepts and methodologies presented in the AutoLog paper are highly relevant to the **"An Adaptive Multi-Layered Honeynet Architecture for Deception-based Threat Intelligence" (ADLAH)** thesis, particularly for the log analysis components.

### Connection to "Post-Interaction Log Analysis"

The thesis envisions a **Post-Interaction Log Analysis** layer responsible for processing logs collected from honeypot interactions. AutoLog provides a direct and proven blueprint for this layer. The challenge in a honeynet is that logs are inherently unstructured, heterogeneous, and come from various emulated services. AutoLog's core strength is its ability to handle such logs without being constrained by their format. The methodology of sampling, scoring via term-weighting, and vectorizing logs can be directly adopted to process the diverse data streams from ADLAH's sensors.

### Connection to "Adaptive Autoencoder for Log Anomaly Detection"

The ADLAH architecture explicitly includes an **Adaptive Autoencoder for Log Anomaly Detection**. The AutoLog paper serves as a foundational reference for implementing this component.

1.  **Unsupervised Nature:** A key requirement for ADLAH is to detect novel and unforeseen attack patterns. AutoLog's semi-supervised approach, which trains only on normative data (or, in the honeynet's case, on "benign" or "baseline" activity), is perfectly aligned with this goal. It allows the system to flag any deviation from the norm as a potential anomaly, which is crucial for identifying zero-day exploits or new TTPs (Tactics, Techniques, and Procedures).
2.  **Adaptability:** The paper discusses the need for re-training to handle changes in system behavior (e.g., software updates). This aligns with the "Adaptive" aspect of the ADLAH component. The baseline model can be periodically updated with new normative data, or in the context of a honeynet, the model could be retrained after each campaign or reset to adapt to changes in the deception environment.
3.  **Non-Linearity:** The paper demonstrates that simple linear methods like PCA fail where a deep autoencoder succeeds. This validates the choice of using an autoencoder in the ADLAH architecture to capture the complex, non-linear patterns of attacker behavior reflected in system logs.

In summary, AutoLog provides a robust, validated, and state-of-the-art methodology that can be integrated into the ADLAH framework to realize its unsupervised, adaptive log analysis capabilities. It solves the critical problem of how to extract meaningful intelligence from unstructured honeypot logs in a scalable and automated fashion.