# Thoughts and Ideas
This document serves as a structured place to record spontaneous thoughts, observations, and ideas related to the thesis.

---

## Table of Contents
- [Thoughts and Ideas](#thoughts-and-ideas)
  - [Table of Contents](#table-of-contents)
  - [General Observations](#general-observations)
  - [possible research gaps](#possible-research-gaps)
  - [Questions to Clarify](#questions-to-clarify)
  - [Potential Improvements](#potential-improvements)
  - [Literature Insights](#literature-insights)
  - [Future Research Ideas](#future-research-ideas)
  - [To Investigate](#to-investigate)

---

## General Observations
> *Date: 14.5.2025*  
-  How does my approach reacts for example to port scanners like nmap?
> *Date: 24.3.2025*  
-  Question BSI, can i expect that the dataset has less then 5% Attack logs? Have there been analysis of the logs and is there a clean log subset
> *Date: 21.3.2025*  
-  Nearly no paper is about anomaly detection in honeypots. That makes sense, because NIDS are the main usecase and there is no big difference between the both in my research context, but it should be possible to use this fact for my research gap
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
5. be able to extract maybe with the lstm the attack chain or at least parts of the attacker to have a output of the honeypot which is usefull. Adaptiveness could build up on that data.
6. the focus could also be on the prblem, that i have not a labeled dataset and thus, no clean training data

---

## Questions to Clarify
> *Date: 24.3.2025*  
-  Should i use byt5 es transformer at first step to big?
> *Date: 22.3.2025*  
-  How can i use generative AI? If i write a text based on my thoughts and with referenced literature, can i input this text, for instance, to ChatGpt and let it rewrite it so its wriiten in a academic and typical journal article manner?
> *Date: 21.3.2025*  
-  What is the BSI doing at the momant with the Honeypot Data? What analysis is happening?
> *Date: 20.3.2025*  
-  Can i make the BSI Dataset public? Could it be transfered to a cloud provider?
> *Date: 19.3.2025*  
-  How can i evaluate the performance of my model, if i do not have labeled test data? That restriction might influence the whole approach.
> *Date: 19.3.2025*  
-  A typical journal paper ranges from 5 to 20 pages, while a regular bachelor's thesis is around 40-50 pages. Would a highly focused and concentrated 15-page paper, demonstrating significant research quality and depth, be sufficient as a bachelor's thesis?
---

## Potential Improvements

---
## Literature Insights 

---

## Future Research Ideas


---

## To Investigate


