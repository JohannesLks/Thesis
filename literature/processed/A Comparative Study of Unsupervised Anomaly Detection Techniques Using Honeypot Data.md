# Summary: A Comparative Study of Unsupervised Anomaly Detection Techniques Using Honeypot Data

This paper conducts a comparative study of major unsupervised anomaly detection techniques for Intrusion Detection Systems (IDS) using real traffic data from honeypots. The study evaluates performance based on various criteria, including similarity measurements, training data size, overall performance, detection of unknown attacks, and time complexity.

## 1. Introduction

- **Problem:** While many machine learning techniques have been applied to IDSs, previous comparative studies suffer from two main weaknesses:
    1.  Reliance on the outdated and simulated KDD Cup 99 dataset.
    2.  Evaluation focus is too narrow, primarily on detection accuracy and false positive rates, ignoring other critical factors like similarity metrics and training data size.
- **Contribution:** This paper addresses these gaps by:
    1.  Evaluating a broad range of unsupervised techniques: 3 outlier detection algorithms, 4 clustering algorithms, and one-class SVM.
    2.  Using **real traffic data** collected from honeypots deployed in a university network.
    3.  Investigating the influence of different **similarity measurements**.
    4.  Analyzing performance against the **size of training data**, overall effectiveness, ability to detect **0-day attacks**, and **time complexity**.

## 2. Unsupervised Anomaly Detection Techniques Evaluated

The study categorizes the techniques into three main groups:

### 2.1. Outlier Detection

These techniques assign an "outlierness" score to each data instance.
- **k-Nearest Neighbor (Kappa):** Score is based on the distance to the *k*-th nearest neighbor.
- **Gamma:** Score is the *average distance* to the *k* nearest neighbors.
- **Local Outlier Factor (LOF):** Score is based on the local density of data instances, identifying outliers relative to their local neighborhood.

### 2.2. Clustering

Algorithms group data into clusters, which are then labeled as normal or attack, typically based on size (smaller clusters are anomalous).
- **K-Means:** A partitioning algorithm that groups data into a pre-defined *k* number of clusters.
- **Song:** A modified K-means approach with an improved labeling heuristic that considers the distance between initial and final cluster centers.
- **Single Linkage:** A hierarchical clustering method where instances are added to the closest cluster if within a distance threshold; otherwise, a new cluster is formed.
- **DBScan:** A density-based algorithm that can find arbitrarily shaped clusters and is robust to noise.

### 2.3. One-Class SVM

- This method learns a boundary (hypersphere) that encompasses the majority of the training data (normal instances). Any data point falling outside this boundary is classified as an anomaly.

## 3. Experimental Setup

- **Data Source:** Real-world traffic data was collected from various honeypots (Windows, Solaris, etc.) and a mail server (for normal traffic) deployed at Kyoto University.
- **Data Preprocessing:**
    1.  Raw traffic was summarized into session data using Bro.
    2.  Sessions were converted into connection records with 14 statistical features (selected based on prior research).
    3.  The attack-to-normal ratio in the training data was adjusted to 1% to mimic realistic network conditions.
- **Training Data:** Two datasets were used: 1 day of traffic and 3 days of traffic.
- **Testing Data:** 10 days of general traffic and 5 specific days containing known "unknown" (0-day) attacks.

## 4. Key Findings and Results

### 4.1. Evaluation of Similarity Measurement

- For distance-based methods, **Euclidean and Chebyshev distances** performed best. These metrics give more weight to dimensions with large differences, which is effective for intrusion detection.
- **Canberra distance** was found to be unsuitable.
- For One-Class SVM, **RBF (Radial Basis Function) kernel** was clearly superior to Polynomial and Sigmoid kernels.

### 4.2. Performance Evaluation

- **Impact of Training Data Size:**
    - The performance of most algorithms (except LOF) **decreased** when trained on more data (3 days vs. 1 day).
    - This is attributed to the fact that larger real-world datasets contain more **noise**, making it harder for the models to distinguish anomalies.
    - LOF's performance improved slightly but remained low overall.

- **Overall Performance:**
    - **Clustering-based approaches consistently outperformed outlier detection methods.** They are more robust to noise because their cluster centers are an average of many data points, mitigating the effect of individual false positives/negatives.
    - **One-Class SVM** showed intermediate performance.
    - These results contradict earlier studies that used the KDD Cup 99 dataset, which is shown to be much less noisy and complex than real-world data.

- **Detection of Unknown Attacks:**
    - The performance trend for detecting unknown attacks was similar to the overall performance, with clustering methods performing the best.

### 4.3. Evaluation of Time Complexity

- **Training Time:**
    - **DBScan and LOF** had extremely long training times (hours), making them impractical for real-time environments.
    - The other algorithms had relatively short training times.
- **Testing Time:**
    - The three **outlier detection approaches** (Kappa, Gamma, LOF) were significantly slower during testing because they must compare each new instance to the entire training set.
    - Clustering methods and One-Class SVM were much faster at detection.

## 5. Conclusion and Future Work

The study provides practical guidelines for applying unsupervised anomaly detection techniques in real-world IDS.

- **Key Takeaways:**
    - **Clustering approaches are more suitable** for intrusion detection than outlier detection methods due to their superior performance, robustness to noise, and better time complexity.
    - The choice of **similarity metric is crucial** (Euclidean/Chebyshev for distance, RBF for SVM).
    - Most techniques are **sensitive to noisy data**, which is prevalent in real-world traffic.
    - Algorithms with high time complexity like **DBScan and LOF are not practical** for real-time intrusion detection.

- **Future Work:**
    - Verify performance on more varied and larger datasets.
    - Investigate the sensitivity of algorithms to their parameters (e.g., *k* in K-Means).
    - Evaluate performance with different attack-to-normal data ratios.