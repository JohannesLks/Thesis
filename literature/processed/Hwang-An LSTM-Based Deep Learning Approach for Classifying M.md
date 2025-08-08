# An LSTM-Based Deep Learning Approach for Classifying Malicious Traffic at the Packet Level

- **Authors:** Ren-Hung Hwang, Min-Chun Peng, Van-Linh Nguyen, and Yu-Lun Chang
- **Year:** 2019
- **Keywords:** deep learning for network security; long short-term memory; malicious traffic classification

## Summary

This paper proposes a novel approach for classifying malicious traffic at the packet level using a combination of word embedding and a Long Short-Term Memory (LSTM) model. The primary motivation is to overcome the significant delays associated with traditional flow-based intrusion detection systems (IDSs), which require accumulating packets into flows before analysis. By operating at the packet level, the proposed method aims to enable real-time, online malicious traffic detection.

The core idea is to treat each packet as a "sentence" where the header fields are the "words". A novel word embedding technique is used to capture the semantic and syntactic relationships between these fields. The resulting embedded representation of the packet is then fed into a three-layer LSTM network, which learns the temporal dependencies among the header fields to classify the packet as either benign or malicious.

The authors evaluate their approach using several public datasets (ISCX2012, USTC-TFC2016, Mirai-RGU) and a self-generated Mirai botnet dataset (Mirai-CCU). The results demonstrate that the packet-level classification is highly competitive with state-of-the-art flow-based methods, achieving nearly 100% accuracy and precision in many cases.

## Key Contributions

1.  **Packet-Level Classification:** The paper introduces a deep learning approach for IDSs that operates at the packet level, significantly reducing the detection latency compared to flow-based methods.
2.  **Word Embedding for Packets:** A novel use of word embedding is proposed to represent packet headers, allowing the model to learn the semantic meaning of packet fields and their relationships.
3.  **LSTM for Temporal Analysis:** An LSTM model is employed to effectively learn the temporal patterns and dependencies within the sequence of packet header fields.
4.  **High Performance:** The proposed framework demonstrates high accuracy, precision, recall, and F1-scores across multiple datasets, proving its effectiveness in identifying malicious packets.

## Discussion and Future Work

The authors acknowledge a trade-off between word size in the embedding and classification performance. They also note that while packet-level analysis speeds up detection, it increases training time and resource usage. Future research directions include developing algorithms to auto-calculate optimal word size and balancing training time with performance. The paper also highlights the susceptibility of DL-based systems to data poisoning attacks as an area for future investigation.