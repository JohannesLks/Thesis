# Summary of the CrowdStrike 2025 Global Threat Report

This document provides a comprehensive summary of the key findings from the CrowdStrike 2025 Global Threat Report, contextualized for the ADLAH thesis. The report highlights the increasing speed, sophistication, and business-like approach of modern cyber adversaries, referred to as "the enterprising adversary."

## Key Statistics and Trends

The 2024 threat landscape was characterized by faster attacks, a surge in identity-focused campaigns, and widespread exploitation of legitimate tools.

*   **Breakout Time:**
    *   The average eCrime breakout time has fallen to a new low of **48 minutes**, down from 62 minutes in 2023.
    *   The fastest observed breakout was a mere **51 seconds**.
*   **Adversary Focus:**
    *   **79%** of all detections were **malware-free**, indicating a heavy reliance on hands-on-keyboard techniques and legitimate tools (Living-off-the-Land).
    *   **Identity-based attacks** are a primary initial access vector. Abuse of valid accounts accounted for **35%** of cloud incidents.
    *   **52%** of observed vulnerabilities were related to **initial access**.
*   **Key TTPs and Threats:**
    *   **Social Engineering:** Voice phishing (vishing) attacks grew by **442%** in the second half of 2024. Adversaries are increasingly using help desk social engineering to gain account access.
    *   **Access-as-a-Service:** Advertisements by initial access brokers increased by **50%** year-over-year, fueling the ransomware ecosystem.
    *   **Cloud Exploitation:** New and unattributed cloud intrusions increased by **26%** compared to 2023. Adversaries are pivoting to the cloud after initial on-premises compromise.
    *   **Generative AI:** Adversaries across all categories (nation-state, eCrime, hacktivist) are early adopters of GenAI for social engineering, disinformation campaigns, and script/tool development.
*   **Nation-State Activity:**
    *   **China-nexus** activity surged by **150%** across all sectors, with some industries seeing increases of 200-300%.

## Top Adversary Trends

### 1. The Business of Social Engineering and Identity Attacks

Adversaries are moving away from traditional malware delivery towards methods that exploit human trust and circumvent technical controls.

*   **Vishing and Help Desk Attacks:** Threat actors like `CURLY SPIDER` use "spam bombing" followed by a vishing call from a fake IT support agent to trick users into providing remote access via legitimate RMM tools (e.g., Microsoft Quick Assist, TeamViewer).
*   **Callback Phishing:** Adversaries like `CHATTY SPIDER` send lure emails (e.g., fake invoices) prompting the victim to call a number, where they are socially engineered.
*   **Identity Compromise:** SCATTERED SPIDER has mastered help desk social engineering, calling IT support to reset passwords and MFA for target accounts. This is often done outside business hours to prolong access.

### 2. Cloud-Conscious Adversaries

The cloud is no longer just a target but an operational platform for attackers. After gaining initial access to an on-premises system, adversaries frequently pivot to the victim's cloud environment, where security is often less mature. They exploit misconfigurations and abuse stolen credentials to escalate privileges and exfiltrate data.

---

### Relevance to Thesis

The findings of this report provide a compelling justification for the development of the **Adaptive Multi-Layered Honeynet Architecture (ADLAH)**. The data on adversary speed, tactics, and focus areas directly addresses the problems ADLAH is designed to solve.

*   **The "Breakout Time" Problem:** The report's headline statistic—an average breakout time of **48 minutes**—is the single most powerful argument for an automated defense system. This timeframe is far too short for human security teams to reliably detect, analyze, and respond. ADLAH's core purpose, using an RL agent for real-time orchestration, is to automate this OODA loop (Observe, Orient, Decide, Act) to a speed that can match and intercept adversaries *before* they can achieve lateral movement and establish persistence.

*   **Countering Modern TTPs:** The report's emphasis on malware-free, identity-driven attacks is highly significant. Traditional signature-based defenses are ineffective against these methods. ADLAH is designed to counter these TTPs directly:
    *   **Living-off-the-Land:** By deploying high-interaction honeypots, ADLAH creates a monitored environment where adversaries' use of legitimate tools (PowerShell, `net use`, etc.) can be safely observed, analyzed, and used to build behavioral threat models.
    *   **Identity Attacks:** An ADLAH-orchestrated honeypot can be configured with decoy credentials. When an attacker compromises these credentials and attempts to use them, it provides a high-fidelity alert and a clear trail of their intended lateral movement path.

*   **Threat Intelligence Goal:** The ADLAH project's primary output is high-fidelity, actionable threat intelligence. The CrowdStrike report is created from just such intelligence, gathered at scale. ADLAH acts as a force multiplier for this effort. By dynamically engaging adversaries and capturing their novel TTPs—especially their use of social engineering scripts, cloud exploitation tools, and GenAI-powered tradecraft—the system can provide a stream of unique, real-world data to the security community, helping to populate future reports and enhance collective defense.