# Summary of "An Anomaly Detection Method to Detect Web Attacks Using Stacked Auto-Encoder"

## 1. Introduction

The paper addresses the critical issue of web security and the increasing threat of network-borne attacks. While traditional security measures like firewalls and Intrusion Detection Systems (IDS) exist, they often fail to inspect application-layer traffic, specifically HTTP. Web Application Firewalls (WAFs) are essential for protecting against attacks like XSS and SQL injection.

The authors highlight two main approaches for IDS:
*   **Signature-based:** Effective for known attacks but fails against new, unknown threats.
*   **Anomaly-based:** Uses machine learning to identify deviations from normal behavior, making it capable of detecting novel attacks.

The paper proposes an anomaly detection method for WAFs that leverages deep learning for feature extraction.

## 2. Proposed Methodology

The proposed system consists of several stages:

1.  **Feature Construction:**
    *   The system analyzes HTTP requests from the WAF server.
    *   It uses an **n-gram model** (character-based) to create feature vectors from HTTP logs. The frequency of each n-gram becomes a feature.
    *   1-gram and 2-gram models are used. The 2-gram model results in a very high-dimensional feature space (over 3600 dimensions initially, reduced to 2572 by filtering frequent items).

2.  **Feature Learning (Dimensionality Reduction):**
    *   To handle the high dimensionality ("curse of dimensionality") from the n-gram model, a **Stacked Auto-Encoder (SAE)** is employed.
    *   An autoencoder is an unsupervised neural network that learns a compressed representation of the input data.
    *   By stacking autoencoders, the model can learn hierarchical, abstract features from the raw n-gram data.
    *   The paper experiments with different SAE configurations, including various activation functions (Sigmoid, Softplus) and optimization algorithms (SGD, Adam, Adagrad, RmsProp).

3.  **Anomaly Detection:**
    *   The final stage uses an **Isolation Forest** classifier.
    *   Isolation Forest is a one-class classifier, well-suited for anomaly detection, as it can be trained only on normal data.
    *   It works by randomly partitioning the data until each data point is isolated. Anomalies are typically isolated with fewer partitions than normal points.
    *   The features learned by the SAE are fed into the Isolation Forest for classification.

## 3. Experiments and Results

*   **Dataset:** The experiments were conducted on the **CSIC 2010 dataset**.
    *   Training set: 28,000 normal requests.
    *   Test set: 28,000 normal and 15,000 abnormal requests.
*   **Implementation:** Python 3.6 with TensorFlow.
*   **Evaluation Metrics:** Accuracy, Detection Rate (Recall), Precision, Specificity, and F1-score.
*   **Key Findings:**
    *   The models using SAE for feature extraction consistently outperformed the models that used the raw n-gram features directly. This demonstrates the effectiveness of deep learning for learning relevant features.
    *   The **Sigmoid activation function** in the SAE neurons yielded significantly better results than Softplus.
    *   The **Adam and RmsProp** optimization algorithms performed best, achieving high accuracy and detection rates.
    *   The best performing model (SAE with Sigmoid and Adam) achieved an accuracy of **88.32%**, a detection rate of **88.34%**, and an F1-score of **84.12%**.
    *   The results indicate that the choice of activation function was more critical than the choice of optimization algorithm for this specific problem.

## 4. Conclusion

The study successfully demonstrates that using a Stacked Auto-Encoder (SAE) for feature extraction significantly improves the performance of anomaly detection in WAFs. The combination of SAE for learning complex data representations and Isolation Forest for one-class classification proves to be an effective approach.

The authors suggest that future work could explore other deep learning architectures and adapt the method for real-time data streams.

## 5. Key Concepts

*   **Web Application Firewall (WAF):** A security tool to protect web applications by filtering and monitoring HTTP traffic.
*   **Stacked Auto-Encoder (SAE):** A deep neural network composed of multiple layers of autoencoders, used for unsupervised feature learning and dimensionality reduction.
*   **Isolation Forest:** An anomaly detection algorithm that works on the principle of isolating anomalies. It is efficient and effective, especially with high-dimensional data.
*   **n-gram:** A contiguous sequence of n items from a given sample of text or speech. In this paper, character-based n-grams are used to featurize HTTP requests.