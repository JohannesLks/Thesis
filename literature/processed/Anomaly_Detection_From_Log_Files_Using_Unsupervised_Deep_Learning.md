# Summary of "Anomaly Detection From Log Files Using Unsupervised Deep Learning"

## 1. Introduction

The paper addresses the challenge of detecting malfunctions in complex computer systems by analyzing voluminous log files. Manual inspection is no longer feasible due to the complexity and scale of modern systems which can generate gigabytes of logs per hour. This necessitates automated log anomaly detection.

Existing solutions often depend on hand-crafted features, require significant preprocessing of raw logs, or use supervised learning models which need labeled datasets that are not always available.

The authors propose a novel two-part deep autoencoder model using LSTM (Long Short-Term Memory) units. This model is designed to work directly on raw log text without requiring hand-crafted features or extensive preprocessing. It outputs an anomaly score for each log entry, reflecting the rarity of the event based on both its content and its temporal context.

The model was tested on a large HDFS (Hadoop Distributed File System) log dataset. While it doesn't match the performance of supervised classifiers, it is presented as a potentially useful tool for filtering logs for manual inspection when labeled data is unavailable.

## 2. Related Work

The paper reviews traditional and recent approaches to log anomaly detection, which typically involve three steps:
1.  **Log Parsing:** Converting unstructured text into structured data. Approaches are divided into clustering-based (calculating distances between logs) and heuristic-based (counting word occurrences).
2.  **Feature Extraction:** Transforming the structured text into numerical feature vectors.
3.  **Anomaly Detection:** Applying a machine learning algorithm.

-   **Traditional ML:** Supervised models like Logistic Regression, Decision Trees, and SVMs have been used, with SVMs generally performing best. Unsupervised models include PCA and clustering, with invariant mining showing superior performance among them. Supervised methods consistently outperform unsupervised ones.
-   **Deep Learning:**
    -   **Supervised:** Du et al. used an LSTM network to treat system logs as a natural language sequence, automatically learning patterns from normal execution to detect deviations. This approach outperformed non-deep learning methods.
    -   **Unsupervised:** Tuor et al. developed an online unsupervised deep learning model (DNN with LSTM units) to detect anomalous network activity in real-time from system logs, outperforming PCA, SVM, and isolation forests. Others have used deep autoencoders that identify anomalies by measuring input reconstruction errors, applying them to HPC systems and industrial time series data.

## 3. Proposed Method

### 3.1. Model Architecture

The core of the proposed method is a two-part model designed for minimal preprocessing and generality across different log types.

1.  **Part 1: Log Message Embedding Autoencoder:**
    *   This is an LSTM-based sequence-to-sequence autoencoder inspired by machine translation models.
    *   It learns to map a variable-length log message (raw text, excluding timestamp) to a fixed-length vector representation (embedding).
    *   After training, the decoder part of this autoencoder is discarded, and only the encoder is used to generate embeddings for log messages.

2.  **Part 2: Anomaly Detection Autoencoder:**
    *   This is another LSTM autoencoder.
    *   Its input consists of two components: the fixed-length embedding of the log message from Part 1, and a numerical representation of the message's timestamp.
    *   The timestamp is transformed using a cosine function (`cos(2π * t / 86400)`) to normalize the time of day and focus on short-term anomalies.
    *   This autoencoder is trained to reconstruct its inputs. The hypothesis is that it will learn to accurately reconstruct common (normal) log patterns it has seen during training, but will have a higher reconstruction error for rare (anomalous) patterns.
    *   An anomaly score is calculated as the L1 distance between the input to this autoencoder and its reconstructed output. An input is flagged as anomalous if this score exceeds a chosen threshold.

The main innovation is the combination of these two components, which allows the model to work on raw log data without needing specific parsing rules or feature engineering, making it more general.

### 3.2. Implementation and Training

-   **Dataset:** The model was trained and evaluated on a publicly available HDFS log dataset containing 11 million labeled lines. A subset of 2 million lines was used (1 million for training, 1 million for testing), with approximately 3% of the data being anomalous.
-   **Preprocessing:**
    -   Date and time are separated from the message.
    -   Non-alphanumeric characters are replaced with whitespace.
    -   Multi-digit numbers are split into single digits (e.g., "67108864" becomes "6 7 1 0 8 8 6 4"). This prevents the vocabulary from exploding with unique numerical values and allows the model to handle unseen numbers.
-   **Training:**
    -   The text embedding autoencoder was trained for 4 epochs with a word embedding size of 200 and 100 LSTM units.
    -   The anomaly detection autoencoder was trained for 4 epochs with 64 LSTM units. The cost function was Mean Squared Error (MSE).

## 4. Experimental Results

-   The ROC (Receiver Operating Characteristic) curve for the model on the test dataset yielded an AUC (Area Under Curve) of **0.59**, indicating poor overall classification performance and weak separation between normal and anomalous classes.
-   Analysis of precision, recall, and F-score revealed that the optimal F-score was achieved at a threshold of 62. Beyond this threshold, precision rises sharply while recall drops rapidly.
-   A key finding is the consistently high number of false negatives, meaning the model fails to detect a large portion of the actual anomalies. This is reflected in the low recall values. The paper suggests this might be because some anomalous events are actually quite common in the dataset.
-   However, at higher thresholds, the number of false positives decreases faster than true positives. This leads to **high precision** at these high thresholds.
-   **Conclusion on Performance:** Although most anomalies are missed, the ones that *are* detected (those with the highest anomaly scores) are very likely to be true anomalies.

## 5. Conclusions

The paper introduced a two-part unsupervised deep learning model for anomaly detection in system logs, notable for its ability to work on raw text without feature engineering.

-   **Strengths:** The model is generic and does not require preprocessing, making it applicable to various types of log files.
-   **Weaknesses:** The experimental results show low recall, meaning it misses a majority of anomalies. Its overall discriminative power (AUC=0.59) is low.
-   **Potential Use Case:** Due to its high precision at high thresholds, the model is proposed as a **coarse filter for human inspection**. In environments with high-frequency log generation, the model can be used to flag a small number of events that are highly likely to be anomalous, reducing the workload for human analysts, even though it won't catch all problems.

The authors state that this work is a preliminary proof of concept and plan more exhaustive experiments in the future.