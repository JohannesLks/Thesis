# Thoughts & Ideas

This document serves as a structured place to record spontaneous thoughts, observations, and ideas related to the thesis.

---

## Table of Contents
- [Thoughts \& Ideas](#thoughts--ideas)
  - [Table of Contents](#table-of-contents)
  - [General Observations](#general-observations)
  - [possible research gaps](#possible-research-gaps)
  - [Potential Improvements](#potential-improvements)
  - [Questions to Clarify](#questions-to-clarify)
  - [Literature Insights](#literature-insights)
  - [Future Research Ideas](#future-research-ideas)
  - [To Investigate](#to-investigate)

---

## General Observations
> *Date: 19.3.2025*  
-  Metrics (Precision, Recall, ...) should be explained as formula.
> *Date: 19.3.2025*  
-  The paper should cover basic math concepts, backpropagation might be to basic but in the explanation of teh Autoencoder or lstm it would fit really well.
> *Date: 18.3.2025*  
-  The paper should mention the differences and parallels between IDS and Honeypot. The proposed solution could find application in both domains.

---

## possible research gaps
1. practical implementation of an Autoencoder with the specific BSI data. (Not enough theoratically work)
2. integrating current aproaches but developing a solution to use the classified information to make the honeypot adaptive to the attacker to gather more information of its behaviour
3. combining current approaches to one new solution (just first bytes, without preprocessing and feature extraction on raw data, system logs als language sequences, non-symmetric data dimensionality reduction, stacked NDAEs and the RF classification algorithm)
4. **Analyze logs on line, sessio and cross session level**  
    - Attacker behavior spans multiple sessions and sequences.  
    - Logs are unstructured and not always tied to a single user or continuous flow.  
    - Single-sequence modeling (like DeepLog) misses cross-session patterns.
    - **Proposed Solution:**  
      - **Line-level anomaly detection**: Autoencoder or text reconstruction for individual log entries.  
      - **Intra-session modeling**: LSTM to detect sequential anomalies within sessions.  
      - **Cross-session modeling**:  
        - LSTM or Graph-based structure over session embeddings.  
        - Capture patterns across multiple sessions over time.  
      - **Global attention layer**: Focus on key sessions or time periods.  
      - **Fusion layer**: Combine all scores into one anomaly indicator.

---

## Questions to Clarify
> *Date: 19.3.2025*  
-  How can i evaluate the performance of my modell, if i do not have labeled test data? That restriction might influence the whole approach.
> *Date: 19.3.2025*  
-  A typical journal paper ranges from 5 to 15 pages, while a regular bachelor's thesis is around 40 pages. Would a highly focused and concentrated 15-page paper, demonstrating significant research quality and depth, be sufficient as a bachelor's thesis?
---

## Potential Improvements

---
## Literature Insights 

---

## Future Research Ideas


---

## To Investigate


