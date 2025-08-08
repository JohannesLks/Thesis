# Summary of "An Unsupervised Deep Learning Model for Early Network Traffic Anomaly Detection"

## 1. Paper Details

*   **Title**: An Unsupervised Deep Learning Model for Early Network Traffic Anomaly Detection
*   **Authors**: Ren-Hung Hwang, Min-Chun Peng, Chien-Wei Huang, Po-Ching Lin, and Van-Linh Nguyen
*   **Publication**: IEEE Access, 2020
*   **DOI**: 10.1109/ACCESS.2020.2973023

## 2. Abstract Summary

The paper introduces **D-PACK**, an effective anomaly detection mechanism for network traffic, designed to counter threats in the Internet of Things (IoT) landscape. D-PACK utilizes a **Convolutional Neural Network (CNN)** for automatic traffic feature profiling and an **unsupervised autoencoder** to filter abnormal traffic. A key feature of D-PACK is its ability to perform early detection by inspecting only the first few bytes of the first few packets in each network flow. The experimental results demonstrate nearly **100% accuracy** with a very low false-positive rate (e.g., 0.83%) by examining just the first two packets of each flow. This approach aims to inspire online anomaly detection systems that reduce the volume of processed data and enable timely blocking of malicious flows.

## 3. Introduction

The paper highlights the growing threat of large-scale Distributed Denial-of-Service (DDoS) attacks, often launched from compromised IoT devices like the Mirai botnet. It points out the limitations of existing defense systems, such as signature-based detection, which struggle to keep up with new attack variants. Anomaly detection offers a more adaptive alternative but often suffers from high false-positive rates. The authors propose that Deep Learning (DL) can overcome these challenges by automatically building traffic profiles from raw data.

The core idea is to sample only the first few packets of each flow, which significantly reduces the computational load and enables faster, online detection. This contrasts with traditional methods that analyze entire flows or rely on manually defined statistical features.

## 4. Proposed System: D-PACK

The D-PACK system consists of two main components:

1.  **CNN Module**: Automatically learns features directly from raw packet data.
2.  **Autoencoder Module**: An unsupervised DL model that builds a profile of benign traffic and classifies flows as normal or anomalous.

### Key Contributions:

*   A novel approach for auto-learning traffic profiles directly from raw traffic using only the first few packets per flow.
*   Implementation and evaluation of the system using credible public datasets and a self-collected realistic dataset.
*   Demonstration of high accuracy and precision, aiming to inspire faster online DL-based anomaly detection systems.

## 5. Learning Strategy

### A. Sampling Network Flows

*   **Goal**: To reduce the amount of data needed for analysis while preserving representative characteristics for classification.
*   **Method**:
    *   Extract the first **`n`** packets of each flow.
    *   Trim each packet to a fixed length of **`l`** bytes from the header.
    *   Pad with zeros if a packet is shorter than `l` or a flow has fewer than `n` packets.
    *   The `n` trimmed packets are concatenated to form a single one-dimensional vector (`n * l` elements), which serves as the input for the CNN.
*   **Rationale**: Internet traffic often exhibits a heavy-tailed distribution, meaning a small portion of the flow (the beginning) can be sufficient for identification. This dramatically reduces processing for long flows.

### B. Auto-Building the Traffic Profile

*   **Model**: A **1D-Convolutional Neural Network (1D-CNN)** is used, as it is well-suited for sequential data. This is more appropriate than 2D-CNN because adjacent bytes within a packet are semantically related, but bytes between different packets (e.g., end of packet 1 and start of packet 2) are not.
*   **Process**:
    1.  The 1D-CNN applies filters to the input vector to extract features.
    2.  Multiple convolution and max-pooling layers are used to learn high-level features.
    3.  A fully connected dense layer outputs a probability distribution for different types of benign traffic (e.g., Gmail, Skype).
    4.  The hidden layers of the CNN are directly connected to an **autoencoder**.
*   **Autoencoder for Anomaly Detection**:
    *   The autoencoder is trained **only on benign traffic**.
    *   It learns to reconstruct the feature representation of normal flows with low error.
    *   During detection, if a flow's reconstruction error (measured by Mean Squared Error Loss - **MSELoss**) is above a dynamically calculated threshold, it is classified as an anomaly.

## 6. Datasets Used for Evaluation

The study uses three datasets to ensure the credibility and robustness of its results:

1.  **USTC-TFC2016**: A public dataset containing 10 types of benign traffic and 10 types of malware traffic.
2.  **Mirai-RGU**: A public dataset from Robert Gordon University containing Mirai botnet traffic (scan, infect, attack) and normal IP camera traffic.
3.  **Mirai-CCU**: A self-collected dataset created in a testbed at National Chung Cheng University, featuring four types of Mirai DDoS attacks (SYN, UDP, ACK, HTTP floods).

## 7. Evaluation and Results

The model was implemented using TensorFlow/Keras and evaluated on a GPU server. The performance was measured using standard metrics: Accuracy, Precision, Recall, F1-Score, False Alarm Rate (FAR/FPR), and False Negative Rate (FNR).

### Key Findings:

*   **High Accuracy**: D-PACK achieved nearly **100% accuracy** on the USTC-TFC2016 and Mirai-CCU datasets, even when inspecting only **2 packets per flow** and **50 bytes per packet**.
*   **Low Error Rates**: False Positive and False Negative rates were consistently very low, often close to 0%.
*   **Optimal Configuration**: The experiments showed that inspecting more bytes per packet was generally more beneficial than inspecting more packets per flow. The configuration of **`n=2` packets** and **`l=80` bytes** was found to be ideal for balancing performance and efficiency across datasets.
*   **Time Efficiency**: The detection system could classify **hundreds of thousands of flows per second**, making it suitable for online monitoring in medium-sized networks. The pre-processing and training are done offline.

### Performance Scenarios:

*   **Scenario 1 (USTC-TFC benign, Mirai-CCU malicious)**: Achieved 100% accuracy with `n=2`, `l=50`.
*   **Scenario 2 (USTC-TFC benign and malicious)**: Achieved near-perfect results, again with low `n` and `l` values.
*   **Scenario 3 (Mirai-RGU dataset)**: Accuracy was slightly lower but still very high (over 99.7%) with `n=2`, `l=80`.

## 8. Discussion and Conclusion

### Trade-offs:

*   There is a trade-off between the trimmed length/number of packets and classification performance. Reducing `n` and `l` improves speed but can impact accuracy if cut too much. The ideal values depend on the specific traffic and attack types.

### Future Work:
*   Developing an algorithm to automatically determine the optimal `n` and `l` values in a live environment.
*   Investigating data poisoning attacks against DL-based classifiers and developing robust defense mechanisms.

### Conclusion:

D-PACK is a novel and effective framework for **early malicious traffic detection**. By sampling a small portion of each flow and using a combined CNN-autoencoder model, it achieves high accuracy, low error rates, and fast detection speeds. The approach significantly reduces the data processing overhead, making it practical for real-time, online anomaly detection systems.