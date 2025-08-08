# Summary of "A Machine Learning Approach to Classify Network Traffic"

This document provides a summary and contextual analysis of the paper "A Machine Learning Approach to Classify Network Traffic" by Jadav et al.

### **Objective**

The primary objective of this research was to conduct a comparative analysis of various machine learning models for the task of classifying network traffic. The study aimed to distinguish between benign (normal) traffic and malicious traffic originating from anonymous networks like Tor and VPN, often referred to as "darknet" traffic. The goal was to identify the most effective classifiers for this binary classification problem, which could then be integrated into detection systems to enhance their efficiency and accuracy.

### **Models Compared**

The paper evaluated a wide array of supervised machine learning algorithms, which were grouped into several categories:

*   **Ensemble Methods:**
    *   AdaBoost
    *   Bagging
    *   Extra Trees Classifier
    *   Gradient Boosting
    *   Random Forest Classifier
*   **Tree-Based Classifiers:**
    *   Decision Tree Classifier
*   **Logistic/Linear Models:**
    *   Logistic Regression CV
    *   Perceptron
    *   Passive Aggressive Classifier
    *   SGD (Stochastic Gradient Descent) Classifier
    *   Ridge Classifier CV
*   **Bayesian Models:**
    *   Gaussian Naive Bayes (NB)
    *   Bernoulli Naive Bayes (NB)
*   **Discriminant Analysis:**
    *   Quadratic Discriminant Analysis
    *   Linear Discriminant Analysis

The study notably excluded K-Nearest Neighbors (KNN) and Support Vector Machine (SVM) from the final results because they caused kernel crashes during the model fitting phase on the given dataset.

### **Dataset and Features**

*   **Dataset:** The study utilized the **CIC-Darknet 2020** dataset from the Canadian Institute for Cybersecurity (CIC). This dataset is a composite, created by combining two other public datasets (ISCXTor2016 and ISCXVPN2016) to provide a rich collection of both benign and darknet (Tor and VPN) traffic. The original dataset was imbalanced, with a significantly larger number of benign samples than darknet samples.
*   **Data Preprocessing:** To address the class imbalance, the authors applied the **Synthetic Minority Over-sampling Technique (SMOTE)**. To handle the high dimensionality (85 initial features), they used **Principal Component Analysis (PCA)**, reducing the feature space to approximately 25 principal components that collectively explained about 90% of the data's variance.
*   **Features:** The features used for training were statistical, time-based, and payload-based characteristics derived from network flows. Key features included:
    *   Flow duration
    *   Total forward/backward packets
    *   Total length of forward/backward packets
    *   Packet length statistics (min, max, mean, std deviation)
    *   Flow IAT (Inter-Arrival Time) statistics (min, max, mean, std deviation)
    *   Flow bytes/s and packets/s
    *   Basic identifiers like source/destination IP, port, and protocol.

### **Key Findings**

The main results of the comparative evaluation were:

*   **Best Performing Models:** The **Extra Trees Classifier** and the **Decision Tree Classifier** significantly outperformed all other models. They both achieved perfect (100%) training accuracy and nearly perfect (99%) test accuracy.
*   **Evaluation Metrics:** The performance was evaluated using multiple metrics:
    *   **Accuracy, Precision, Recall, and F1-Score:** The top models scored 0.99 across all these metrics on the test set.
    *   **Matthews Correlation Coefficient (MCC):** The Extra Tree and Decision Tree classifiers also achieved the highest MCC scores (0.99), indicating a very strong positive correlation between predictions and actual classes and confirming their robustness on the binary classification task.
    *   **Log Loss Score:** These two models also had the lowest log loss scores (0.028 and 0.030, respectively), signifying high confidence in their correct predictions.
    *   **AUC-ROC:** The ROC curves for the Extra Trees and Decision Tree models showed an area under the curve (AUC) of 0.99, demonstrating excellent separability between the benign and darknet classes.
*   **Performance of Other Models:** While other ensemble methods like Bagging and Random Forest also performed well (99% accuracy, 0.98 MCC), the logistic, Bayesian, and discriminant analysis models showed significantly poorer performance, with accuracies ranging from 39% to 79% and some showing signs of overfitting.

The study concluded that tree-based ensemble methods, particularly Extra Trees and Decision Trees, are highly effective and competent for classifying network traffic, even in complex scenarios involving anonymized services.

---

### **Relevance to Thesis**

This comparative study provides critical insights that directly inform the design and implementation of the **ADLAH (Adaptive Multi-Layered Honeynet Architecture)**, especially its analytical components.

#### **Informing Model Selection**

The paper's rigorous comparison of 15 different classifiers offers a valuable data-driven foundation for selecting algorithms for ADLAH's `Post-Interaction Analysis Layer`. The standout performance of the **Extra Trees** and **Decision Tree** models suggests that these should be primary candidates for the initial implementation of ADLAH's classification engine. This is particularly relevant because these models are not only highly accurate but also generally less computationally intensive than more complex deep learning models, which aligns with the need for a real-time, responsive system. The study demonstrates that for well-defined classification tasks with engineered features, simpler, more interpretable models can achieve state-of-the-art performance.

#### **Feature Engineering**

The research underscores the critical importance of preprocessing and feature engineering. The use of **PCA** to reduce 85 features to 25 and **SMOTE** to balance the dataset were key steps in achieving high accuracy. This directly informs the ADLAH architecture: data collected by the `Sensor Layer` cannot be used raw. It must first pass through a preprocessing pipeline that performs:
1.  **Feature Extraction:** Generating meaningful statistical features from raw packet data and logs.
2.  **Dimensionality Reduction:** Techniques like PCA can be used to create a more efficient and effective feature set, reducing training time and the risk of overfitting.
3.  **Data Balancing:** Ensuring that the models are not biased towards the majority class (e.g., benign or low-level scanning traffic).

#### **Performance Benchmarks**

The performance metrics reported in this paper—particularly the **99% accuracy, 0.99 F1-Score, and 0.99 MCC** achieved by the top models on the CIC-Darknet 2020 dataset—provide an excellent **performance benchmark** for ADLAH. As custom classifiers are developed for ADLAH using data collected from its own honeypots, these results serve as a concrete target. If ADLAH's classifiers can achieve similar or better performance on its specific task (e.g., classifying attacker intent or malware families), it will validate the effectiveness of its design.

#### **Trade-offs (Speed vs. Accuracy)**

While the paper does not explicitly measure training or inference time, the choice of models implicitly addresses the trade-off between complexity and performance. Decision Trees and Extra Trees classifiers are known for being relatively fast to train and quick for inference compared to deep learning architectures or even some other ensemble methods like Gradient Boosting. This is a critical consideration for ADLAH, which is designed to be an *adaptive* system. Fast model retraining and real-time classification are essential for the feedback loop where the `Post-Interaction Analysis Layer` informs the `Orchestrator Layer` to reconfigure the honeynet. This paper provides evidence that high accuracy does not have to be sacrificed for the sake of performance, making these models a practical choice for a real-time system.