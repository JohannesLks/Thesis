# Summary of "A Machine Learning Approach to Classify Network Traffic"

## Introduction

The paper addresses the challenge of classifying network traffic, particularly in distinguishing between benign and malicious activities, which has become increasingly complex due to the rise of encrypted and anonymized networks like Tor and VPNs. Traditional methods such as Deep Packet Inspection (DPI) are often insufficient as attackers use sophisticated techniques to bypass them. The authors argue that Machine Learning (ML) offers a more viable solution for effective traffic classification.

The research focuses on analyzing the **CIC-Darknet 2020 dataset** to differentiate between normal (benign) and darknet traffic.

## Key Contributions

- A thorough comparison of various ML classifiers for the task of benign vs. darknet traffic classification.
- Application of **Principal Component Analysis (PCA)** for dimensionality reduction to handle the high-dimensional dataset.
- Use of **Synthetic Minority Over-sampling Technique (SMOTE)** to address the class imbalance in the dataset.
- Detailed evaluation of the classifiers using multiple metrics: Accuracy, Precision, Recall, F1-Score, and Matthews Correlation Coefficient (MCC).

## Methodology

### 1. Dataset

- **CIC-Darknet 2020**: A dataset created by the Canadian Institute for Cybersecurity (CIC) by combining two other datasets (ISCXTor2016 and ISCXVPN2016).
- It contains **141,530 instances** and **85 features**.
- It's a binary classification problem with two labels: "Benign" and "Darknet".
- The dataset was found to be **imbalanced**, with a significantly higher number of "Benign" instances (117,219) compared to "Darknet" instances (24,311).

### 2. Pre-processing

- **Dimensionality Reduction using PCA**: Due to the high number of features (85), PCA was applied. The analysis showed that approximately **25 principal components** could explain about **90% of the variance** in the data. This reduced feature space was used for training to lower computational cost and reduce the risk of overfitting.
- **Data Balancing using SMOTE**: To counteract the class imbalance, SMOTE was used. This oversampling technique synthetically creates new minority class instances (Darknet) to balance the dataset. After SMOTE, both "Benign" and "Darknet" classes had **117,219 instances** each, resulting in a balanced dataset of 234,438 total instances.

### 3. Machine Learning Classifiers

A wide range of ML algorithms were trained and evaluated on the pre-processed dataset:

- **Ensemble Methods**:
    - AdaBoost
    - Bagging
    - Extra Trees
    - Gradient Boosting
    - Random Forest
- **Tree-based Classifier**:
    - Decision Tree
- **Logistic/Linear Models**:
    - Logistic Regression
    - Perceptron
    - Passive Aggressive Classifier
    - SGD Classifier
    - Ridge Classifier
- **Discriminant Analysis**:
    - Quadratic Discriminant Analysis (QDA)
    - Linear Discriminant Analysis (LDA)
- **Bayesian Classifier**:
    - Gaussian Naive Bayes (NB)
    - Bernoulli NB

## Results and Analysis

The performance of the classifiers was evaluated using a confusion matrix and several key metrics.

- **Top Performers**: The **Extra Trees** and **Decision Tree** classifiers emerged as the best-performing models.
    - They both achieved **100% training accuracy** and **99% testing accuracy**.
    - They also showed the highest F1-Scores (0.99) and MCC values (0.99), indicating a very strong and reliable classification performance.
    - Their Log Loss scores were the lowest (0.028 for Extra Trees, 0.030 for Decision Tree), signifying better probability predictions.
    - The ROC curves for these two models were closest to the top-left corner, with an AUC score of 0.99, demonstrating their excellent ability to distinguish between the two classes.

- **Other Ensemble Methods**: Other ensemble methods like AdaBoost, Bagging, and Random Forest also performed very well, achieving ~99% accuracy.

- **Poor Performers**: Models like the Passive Aggressive Classifier and Ridge Classifier performed poorly.

- **Overfitting**: Logistic Regression and SGD classifiers showed signs of overfitting, with test accuracy being slightly higher than train accuracy, although the use of PCA and SMOTE helped mitigate this issue for most models.

## Conclusion

The study successfully demonstrates that machine learning, particularly ensemble methods like **Extra Trees** and **Decision Tree**, can be highly effective in classifying network traffic and distinguishing benign activity from darknet traffic.

### Key Takeaways:

- Pre-processing steps like **PCA** for dimensionality reduction and **SMOTE** for handling class imbalance are crucial for building high-performance models on complex network datasets.
- **Extra Trees** and **Decision Tree** classifiers provided the most accurate and reliable results for this specific classification task.
- The authors suggest that these trained models can be integrated into detection systems to enhance their efficiency.

### Future Work

- Generate a custom dataset for further validation.
- Compare the performance of these ML algorithms with Deep Learning models.
- Develop enhanced multi-class classification algorithms to not only detect anonymous traffic but also classify its type (e.g., VPN, Tor, Proxy).