# Analysis of "Framework for Improving Critical Infrastructure Cybersecurity"

### 1. Structured Summary

*   **Title:** Framework for Improving Critical Infrastructure Cybersecurity, Version 1.1
*   **Author:** This document is the result of a collaborative effort led by the National Institute of Standards and Technology (NIST), involving industry, academia, and government stakeholders.
*   **Publication Date:** April 16, 2018

#### Objective
The main objective of the NIST Cybersecurity Framework is to provide a voluntary, risk-based approach for organizations, particularly those managing critical infrastructure, to identify, assess, and manage cybersecurity risks. It aims to offer a common language and structure for cybersecurity activities, enabling communication between technical and executive stakeholders. The framework is designed to be flexible, repeatable, performance-based, and cost-effective, helping organizations improve their security and resilience without imposing new regulatory requirements. It guides organizations in aligning their cybersecurity activities with their business needs, risk tolerances, and resources.

#### Methodology/Framework
The NIST Cybersecurity Framework is composed of three main parts:

1.  **Framework Core:** A set of common cybersecurity activities, desired outcomes, and informative references. It is organized into five continuous and concurrent **Functions**:
    *   **Identify:** Understand the business context, the resources that support critical functions, and the related cybersecurity risks.
    *   **Protect:** Implement appropriate safeguards to ensure the delivery of critical services and limit the impact of potential events.
    *   **Detect:** Implement activities to identify the occurrence of a cybersecurity event in a timely manner.
    *   **Respond:** Take appropriate action after a cybersecurity incident has been detected.
    *   **Recover:** Maintain resilience and restore any capabilities or services that were impaired due to an incident.

2.  **Implementation Tiers:** These describe the degree of rigor and sophistication of an organization's cybersecurity risk management practices, ranging from **Tier 1 (Partial)** to **Tier 4 (Adaptive)**. Tiers provide context on how an organization views and manages cybersecurity risk
3.  **Framework Profiles:** These represent the alignment of the Functions, Categories, and Subcategories with an organization's specific business requirements, risk tolerance, and resources. Organizations create a "Current Profile" (as-is state) and a "Target Profile" (to-be state) to identify gaps and create a roadmap for improvement.

#### Key Concepts
*   **Risk-Based Approach:** The framework emphasizes that cybersecurity decisions should be driven by an understanding of organizational risk, including threats, vulnerabilities, and potential impacts.
*   **Common Language:** It provides a shared taxonomy for internal and external stakeholders to communicate about cybersecurity posture, requirements, and risks.
*   **Adaptability and Flexibility:** The framework is technology-neutral and not a one-size-fits-all solution. It is designed to be customized by organizations of any size or sector to fit their unique needs.
*   **Cyber Supply Chain Risk Management (SCRM):** Version 1.1 adds significant focus on managing risks associated with external parties, including suppliers of technology and non-technology products and services.
*   **Continuous Improvement:** The framework is presented as a "living document" and encourages a continuous cycle of assessment, action, and reassessment.

#### Main Findings/Recommendations
*   Organizations should integrate cybersecurity risk management into their overall risk management processes, viewing it alongside financial and reputational risks.
*   The use of **Framework Profiles** is recommended to establish a clear baseline (Current Profile) and a goal (Target Profile) for cybersecurity posture.
*   Organizations should use the **Implementation Tiers** to self-assess their risk management processes and determine a target state that aligns with their business objectives and threat environment.
*   The framework strongly encourages communication and collaboration among stakeholders, both internally (from the executive level to operations) and externally with partners throughout the supply chain.
*   Effective cyber SCRM is critical and involves determining requirements for suppliers, formalizing them in contracts, and verifying that those requirements are met.

### 2. Relevance to Thesis (ADLAH)

The NIST Cybersecurity Framework provides a foundational and authoritative context for the ADLAH (Adaptive Deep Learning Anomaly Detection Honeynet) project. While the ADLAH thesis focuses on a specific, technical implementation of an adaptive defense system, the NIST framework offers a high-level, strategic justification for its existence and a structure for its integration into a broader security program.

1.  **Alignment with Core Functions:** The ADLAH system directly supports several of the NIST Framework's core functions:
    *   **Protect (`PR.PT`):** ADLAH serves as a "Protective Technology" by implementing an advanced cyber deception mechanism. It actively works to limit the impact of an event by engaging adversaries in a controlled environment.
    *   **Detect (`DE.AE`], [`DE.CM`):** The entire purpose of ADLAH is to improve the detection of cybersecurity events. Its use of machine learning for first-flight analysis addresses "Anomalies and Events," and its ability to escalate sessions to high-interaction honeypots is a form of "Security Continuous Monitoring."
    *   **Respond (`RS.AN`], [`RS.MI`):** By capturing high-fidelity intelligence on adversary TTPs, ADLAH directly supports "Analysis" and "Mitigation." The intelligence gathered can inform an organization on how to contain and eradicate a threat.
    *   **Identify (`ID.RA`):** The threat intelligence generated by ADLAH is a critical input for "Risk Assessment." It helps the organization understand current threats, which is a key component of the Identify function.

2.  **Justification for an Adaptive Approach:** The NIST Framework's **Implementation Tiers** provide a clear rationale for a system like ADLAH. An organization aiming for a **Tier 4 (Adaptive)** posture is expected to adapt its cybersecurity practices based on lessons learned and predictive indicators, and to respond effectively "to evolving, sophisticated threats." ADLAH's RL-driven orchestration is a prime example of such an adaptive capability, moving beyond the static defenses that would characterize lower Tiers.

3.  **Addressing Supply Chain Risks ([`ID.SC`]):** The NIST Framework highlights the importance of understanding and managing Cyber Supply Chain Risk Management. While ADLAH is a defensive tool, the intelligence it gathers on novel TTPs can help an organization understand the threats posed by external actors, including those who may be targeting suppliers or partners, thus informing the organization's overall SCRM strategy.

4.  **Improving Risk Management and Decision Making:** The ADLAH architecture, which uses reinforcement learning to make resource-allocation decisions (i.e., when to deploy a high-interaction honeypot), is a practical implementation of the NIST framework's core principle: making risk-based decisions to prioritize resources effectively. Instead of wasting resources on low-level scans, ADLAH aims to focus on high-value threats, maximizing the impact of security expenditures.

In summary, the NIST Cybersecurity Framework provides the "why" and "what" for an organization's security program, while the ADLAH project provides a specific, advanced implementation of the "how." The ADLAH system can be presented as a technical solution that enables an organization to mature its cybersecurity capabilities and advance to a higher Implementation Tier within the respected and widely adopted NIST model.