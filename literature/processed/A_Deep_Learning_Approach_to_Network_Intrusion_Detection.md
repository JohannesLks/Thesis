# Summary: A Deep Learning Approach to Network Intrusion Detection

This document provides a detailed summary of the paper "A Deep Learning Approach to Network Intrusion Detection" by Nathan Shone, Tran Nguyen Ngoc, Vu Dinh Phai, and Qi Shi.

## 1. Introduction

The paper highlights the increasing challenges for Network Intrusion Detection Systems (NIDSs) due to the growth in network data volume, the need for in-depth monitoring, and the diversity of network protocols. Traditional signature-based NIDSs are becoming less effective. While machine learning techniques like Naive Bayes, Decision Trees, and Support Vector Machines (SVM) have shown improvements, they require significant human expertise, are labor-intensive, and need large amounts of training data. Deep learning is presented as a promising solution to overcome these limitations by enabling deeper analysis of network data and faster anomaly identification.

The main contributions of the paper are:
- A new **non-symmetric deep auto-encoder (NDAE)** for unsupervised feature learning.
- A novel classifier model using **stacked NDAEs** combined with the **Random Forest (RF)** algorithm.

## 2. Background

### 2.1. NIDS Challenges
- **Volume**: The massive amount of data in modern networks (e.g., 100Gbps links) makes real-time analysis extremely challenging.
- **Accuracy**: Achieving high accuracy requires more granular and context-aware monitoring.
- **Diversity**: The increasing number of protocols and devices makes it difficult to distinguish between normal and abnormal behavior.
- **Dynamics**: The dynamic nature of modern networks makes establishing a reliable baseline difficult.
- **Low-frequency attacks**: Imbalanced datasets often lead to poor detection of infrequent attacks.
- **Adaptability**: NIDSs need to adapt to new technologies like containerization, virtualization, and Software Defined Networks (SDN).

### 2.2. Deep Learning
- **Auto-encoder**: An unsupervised neural network that learns to reconstruct its input. It is used for feature extraction and dimensionality reduction. A typical auto-encoder has a symmetric encoder-decoder structure.
- **Stacked Auto-encoder**: A deep learning model composed of multiple layers of auto-encoders, where the output of each layer is the input to the next. This allows learning of hierarchical features.

## 3. Existing Work

The paper reviews existing research on applying deep learning to NIDSs. Many studies show that deep learning methods (e.g., Deep Belief Networks (DBNs), Recurrent Neural Networks (RNNs), Deep Neural Networks (DNNs)) outperform traditional machine learning techniques. However, there is still room for improvement regarding reliance on human operators, training time, and handling of imbalanced datasets.

## 4. Proposed Methodology

### 4.1. Non-symmetric Deep Auto-encoder (NDAE)
The authors propose the NDAE, which differs from traditional auto-encoders by using a non-symmetrical structure, focusing only on the encoding phase. This is intended to reduce computational overheads without significantly impacting accuracy. The NDAE is used as a hierarchical unsupervised feature extractor.

### 4.2. Stacked NDAE Classification Model
The proposed model consists of two stacked NDAEs for deep feature learning, combined with a Random Forest (RF) classifier for the final classification. RF is chosen for its robustness, low bias, and good performance in intrusion detection tasks.

The model architecture is as follows:
- **Input Layer**: 41 features (from KDD datasets).
- **NDAE 1**: 3 hidden layers (28, 14, 28 neurons).
- **NDAE 2**: 3 hidden layers (28, 14, 28 neurons).
- **Classifier**: Random Forest.

The structure was determined through 10-fold cross-validation on the NSL-KDD dataset.

## 5. Evaluation & Results

The model was implemented using GPU-enabled TensorFlow and evaluated on the KDD Cup '99 and NSL-KDD benchmark datasets. The performance was compared against a Deep Belief Network (DBN) model.

**Metrics Used**: Accuracy, Precision, Recall, F-score, False Alarm Rate.

### 5.1. KDD Cup '99 Dataset (5-Class Classification)
- The proposed stacked NDAE model achieved an average accuracy of **97.85%**, which is comparable to or better than the DBN model (97.90%).
- The model showed significantly better precision, recall, and F-score for larger classes like "Normal" and "DoS".
- Performance was weaker for smaller classes ("R2L" and "U2R") due to insufficient training data.
- The training time was drastically reduced by an average of **97.72%** compared to the DBN model.

### 5.2. NSL-KDD Dataset
#### 5.2.1. 5-Class Classification
- The model achieved an accuracy of **85.42%**, outperforming the DBN model's 80.58%.
- It also showed a lower false alarm rate (14.58% vs 19.42%).
- Again, performance was weaker for smaller classes.
- The training time was reduced by an average of **78.19%**.

#### 5.2.2. 13-Class Classification
- The model's accuracy increased to **89.22%**, supporting the claim that it performs better with more granular, complex datasets.
- The results confirmed that the model's accuracy is correlated with the size of the training data for each class.

### 5.3. Comparison with Related Works
The proposed model's performance was shown to be superior to other deep learning-based NIDSs from the literature on the same datasets.
- **vs. Tang et al. [23]**: 85.42% accuracy vs. 75.75% on NSL-KDD.
- **vs. Kim et al. [37]**: 97.85% accuracy vs. 96.93% on KDD Cup '99.
- **vs. Gao et al. [38]**: 97.85% accuracy vs. 93.49% on KDD Cup '99.


## 6. Discussion

The evaluations demonstrate that the proposed stacked NDAE model provides high accuracy, precision, and recall, especially for well-represented attack classes, while significantly reducing training time compared to DBNs. The non-symmetric design proves to be time-efficient. The model's main weakness is its performance on classes with very few training samples.

## 7. Conclusion & Future Work

The paper successfully proposes a novel deep learning model for network intrusion detection that combines a non-symmetric deep auto-encoder (NDAE) with a Random Forest classifier. The model achieves promising results, outperforming benchmark DBN models in terms of both accuracy and training time.

Future work will focus on:
- Extending the model to handle zero-day attacks.
- Evaluating the model using real-world backbone network traffic.
