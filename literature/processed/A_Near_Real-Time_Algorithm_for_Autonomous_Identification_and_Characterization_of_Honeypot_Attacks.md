# Summary of "A Near Real-Time Algorithm for Autonomous Identification and Characterization of Honeypot Attacks"

This paper by Philippe Owezarski presents an unsupervised algorithm called UNADA (Unsupervised Network Anomaly Detection Algorithm) for identifying and characterizing security-related anomalies and attacks in honeypot traffic.

## Key Contributions

*   **Unsupervised Approach**: UNADA does not require any attack signature database, learning phase, or labeled traffic, making it a significant step towards autonomous security systems.
*   **Honeypot Specific**: The algorithm is specifically adapted to analyze honeypot traffic, which is assumed to be entirely illegitimate. This simplifies the characterization and identification process.
*   **Robust Clustering**: It uses a combination of sub-space clustering, density-based clustering, and evidence accumulation to classify flows into traffic classes with easily understandable signatures.
*   **Correlation of Anomalies**: The paper introduces a method to correlate anomalous traffic classes detected in different sub-spaces and aggregation levels, which helps in identifying all parts of a single, complex attack.
*   **Automatic Characterization**: The algorithm can automatically generate filtering rules from the identified anomalies, which can be used to configure network security devices like routers and firewalls.
*   **Risk Ranking**: A method for ranking anomalies based on the risk they represent is proposed, helping security experts to prioritize their responses.
*   **Performance**: The paper demonstrates UNADA's high accuracy and low latency, especially when parallelized, using real-world honeypot traffic data from the University of Maryland.

## Algorithm Details

1.  **Traffic Aggregation**: The algorithm analyzes traffic in consecutive time-slots, aggregating it at nine different flow levels (e.g., source/destination IPs and network prefixes).
2.  **Unsupervised Anomaly Detection**:
    *   It uses a set of traffic features (e.g., number of source/destination IPs, packet rate, fraction of SYN packets) to describe each flow.
    *   To overcome the limitations of single clustering algorithms, it employs a **Sub-Space Clustering (SSC)** approach, applying a density-based clustering algorithm to multiple 2-dimensional sub-spaces of the feature space.
    *   The results from the different sub-spaces are combined using two methods:
        *   **Evidence Accumulation (EA)**: Builds similarity measures between flows to detect small clusters and outliers.
        *   **Inter-Clustering Result Association (ICRA)**: A more robust method that correlates clusters and outliers across different sub-spaces to identify consistent anomalous flow sets.
3.  **Correlation of Anomalous Traffic Classes**:
    *   To group together different manifestations of the same attack, the algorithm correlates the identified traffic classes.
    *   This correlation considers not only the source and destination IP addresses but also the timing of the traffic to avoid incorrectly linking unrelated events that are far apart in time.
4.  **Automatic Characterization and Rule Generation**:
    *   For each identified and correlated anomaly, the algorithm generates a signature in the form of filtering rules.
    *   These rules can be **absolute** (e.g., `nICMP/nPkts == 1`) or **relative** (e.g., `nDiffDestAddr < 9`) and are selected based on how well they separate the anomalous traffic from normal traffic.
5.  **Risk Ranking**:
    *   The risk of an anomaly is calculated based on:
        *   The number of sub-spaces in which it appears.
        *   The number of packets exchanged.
        *   The number of bytes exchanged.
        *   The duration of the communication.

## Evaluation

*   The algorithm was tested on honeypot traffic from the University of Maryland.
*   It successfully identified and characterized various attacks, including DoS attacks (e.g., SYN flooding, RST attacks) and more complex TCP flooding attacks.
*   When compared to other unsupervised methods like DBSCAN-based, k-means-based, and PCA-based outlier detection, UNADA showed a significantly higher True Positive Rate for a given False Positive Rate.
*   The paper also highlights the algorithm's scalability and suitability for near real-time operation due to its parallelizable nature.

## Conclusion

UNADA represents a promising step towards fully autonomous network security. By providing an unsupervised, robust, and efficient way to identify, characterize, and generate countermeasures for attacks detected in honeypots, it can significantly reduce the workload of security experts and enable faster, more automated network defense.