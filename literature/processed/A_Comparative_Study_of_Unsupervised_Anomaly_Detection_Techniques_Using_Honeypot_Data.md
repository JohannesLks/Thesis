# Summary of "A Comparative Study of Unsupervised Anomaly Detection Techniques Using Honeypot Data"

This paper conducts a comparative study of major unsupervised anomaly detection techniques using real traffic data from honeypots. The goal is to provide practical guidelines for researchers and operators on applying these techniques for intrusion detection.

## 1. Introduction

*   **Problem:** While many machine learning techniques have been applied to Intrusion Detection Systems (IDS), especially unsupervised methods capable of detecting 0-day attacks, previous evaluations have weaknesses. They often use outdated and simulated datasets (like KDD Cup 99) and focus narrowly on accuracy and false positive rates, ignoring other critical factors.
*   **Contribution:** This study addresses these gaps by:
    1.  Evaluating a broader range of unsupervised techniques, including density-based methods like LOF and DBScan.
    2.  Using **real traffic data** collected from honeypots deployed in a university network.
    3.  Investigating the influence of different **similarity measurements**.
    4.  Comparing performance based on training data size, overall performance, ability to detect unknown attacks, and **time complexity**.

### Key Findings Preview

*   **Similarity Measures:** Euclidean and Chebyshev distances, and the RBF kernel for One-Class SVM, are most effective.
*   **Technique Suitability:** Clustering approaches are generally more suitable for intrusion detection than outlier detection methods.
*   **Data Quality:** Most techniques are sensitive to noisy data.
*   **Time Complexity:** DBScan and LOF have long training times, and outlier detection methods have long testing times, making them less practical for real-time applications.

## 2. Unsupervised Anomaly Detection Techniques Evaluated

The paper evaluates three main categories of unsupervised techniques:

### 2.1. Outlier Detection

These techniques assign an anomaly score to each data instance.
*   **k-Nearest Neighbor (Kappa):** Score is based on the distance to the *k*-th nearest neighbor.
*   **Gamma:** Score is the *average* distance to the *k* nearest neighbors.
*   **Local Outlier Factor (LOF):** Score is based on the local density of data points, identifying outliers relative to their local neighborhood.

### 2.2. Clustering

These algorithms group data, and clusters are labeled as normal or attack, typically based on size (smaller clusters are anomalous).
*   **K-Means:** A standard partitioning algorithm.
*   **Song:** An improved K-Means method with a different labeling heuristic based on the distance between final and initial cluster centers.
*   **Single Linkage:** A hierarchical clustering method.
*   **DBScan:** A density-based algorithm that can find arbitrarily shaped clusters and is robust to noise.

### 2.3. One-Class SVM

This algorithm learns a boundary (hypersphere) around the majority of normal data. Instances falling outside this boundary are classified as anomalies.

## 3. Experimental Setup and Analysis

### 3.1. Data Collection

*   **Data Source:** Traffic data was collected from various honeypots (Windows, Solaris, proactive systems joining botnets) deployed inside and outside Kyoto University.
*   **Labeling:** Honeypot traffic was considered attack data. Normal data was generated from a dedicated mail server.
*   **Dataset Creation:**
    1.  Raw traffic was summarized into session data using Bro.
    2.  Sessions were converted into connection records with 14 statistical features (selected as the most significant from the original KDD Cup 99 set).
    3.  The attack ratio in the training data was adjusted to 1% to mimic realistic network conditions.
*   **Training Data:** Two sets were used: 1 day (58k instances) and 3 days (209k instances).
*   **Testing Data:** 10 days of general traffic and 5 specific days containing known "unknown" (0-day) attacks.

### 3.2. Evaluation of Similarity Measurement

*   **Key Finding:** The choice of similarity measure significantly impacts performance.
*   **Results:**
    *   For distance-based methods, **Euclidean and Chebyshev distances** performed best. They effectively penalize large differences in any single feature dimension. Canberra distance was unsuitable.
    *   For One-Class SVM, the **RBF kernel** was clearly superior to Polynomial and Sigmoid kernels.

### 3.3. Performance Evaluation

#### Performance by Training Data Size

*   **Observation:** Performance for most algorithms (except LOF) **decreased** when the training data size was increased from 1 day to 3 days.
*   **Reasoning:** Larger datasets contained more noise, which negatively impacted the models. This highlights the need for noise-robust algorithms. Although LOF's performance improved, it remained low overall.

#### Overall Performance

*   **Key Finding:** **Clustering-based approaches outperform outlier-based approaches**, with One-Class SVM being intermediate.
*   **Reasoning:** Outlier detection methods are highly sensitive to individual false positives/negatives in the training data. In contrast, clustering methods are more robust because their cluster centers are averaged over many members, mitigating the effect of a few misclassified instances.
*   **Comparison to Past Research:** Previous studies using the (less noisy) KDD Cup 99 dataset found outlier methods to be more competitive. This study shows that on real, noisy data, their performance degrades significantly.

#### Performance for Unknown Attacks

*   The results for detecting unknown attacks were consistent with the overall performance findings, with clustering methods performing better.

### 3.4. Evaluation of Time Complexity

*   **Training Time:**
    *   **DBScan and LOF** were extremely slow (hours), making them impractical for frequent retraining.
    *   Other methods had relatively short training times.
*   **Testing Time:**
    *   The three **outlier detection approaches were very slow** because they must compare each test instance to all training instances.
    *   Clustering and One-Class SVM were much faster, making them more suitable for real-time detection.

## 4. Conclusion and Future Work

### Summary of Key Guidelines

1.  **Similarity Measure Choice is Crucial:** Use Euclidean or Chebyshev distance for distance-based methods and the RBF kernel for One-Class SVM.
2.  **Clustering is More Robust:** Clustering-based approaches are generally better suited for intrusion detection on real-world data than outlier detection methods due to better performance and lower time complexity. One-Class SVM is a viable middle ground.
3.  **Beware of Noisy Data:** Most current unsupervised techniques are not robust to noise, which is prevalent in real traffic.
4.  **Practicality Matters:** The high time complexity (both for training and testing) of algorithms like LOF, DBScan, and k-NN-style outlier detectors makes them impractical for many real-world IDS deployments.

### Future Work

*   Verify performance on even larger and more varied datasets.
*   Investigate the sensitivity of these algorithms to their configuration parameters.
*   Evaluate performance with different attack-to-normal data ratios in the training set.
