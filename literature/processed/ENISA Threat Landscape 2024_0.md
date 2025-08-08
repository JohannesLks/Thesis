# Analysis of ENISA Threat Landscape 2024

This document provides a summary of the top cybersecurity threats identified in the ENISA Threat Landscape 2024 report, covering the period from July 2023 to June 2024. It also outlines the relevance of these threats to the ADLAH (Adaptive Multi-Layered Honeynet Architecture) thesis.

## Top 8 Prime Threats and Key Trends

The ENISA report identifies eight prime threats that have dominated the cybersecurity landscape. Below is a summary of each threat and its associated key trends.

1.  **Ransomware**: Attacks where threat actors seize control of assets and demand a ransom for their return or to prevent data exposure.
    *   **Key Trends**: Ransomware attacks have stabilized at high numbers, with operators increasingly using "Living Off The Land" (LOTL) techniques to evade detection and weaponizing disclosure requirements to pressure victims into paying.

2.  **Malware**: Malicious software or firmware designed to perform unauthorized processes that compromise the confidentiality, integrity, or availability of a system.
    *   **Key Trends**: Malware-as-a-Service (MaaS) offerings are evolving rapidly, and a recent surge in mobile banking trojans with complex attack vectors has been observed.

3.  **Social Engineering**: A broad range of activities that exploit human error or behavior to gain access to information or services.
    *   **Key Trends**: There has been a sharp increase in Business Email Compromise (BEC) incidents, and supply chain compromises are now emerging through sophisticated social engineering campaigns.

4.  **Threats Against Data**: Incidents leading to the accidental or unlawful destruction, loss, alteration, or unauthorized disclosure of or access to data.
    *   **Key Trends**: Data compromises increased in 2023-2024, and data leak sites are becoming unreliable, with many posts being duplicates or misattributed following law enforcement disruptions.

5.  **Threats Against Availability (Denial of Service)**: Attacks that prevent legitimate users from accessing data, services, or other resources, often by overwhelming the target system.
    *   **Key Trends**: DDoS attacks, alongside ransomware, ranked as the top threats for another year. The availability of DDoS-for-Hire services allows unskilled users to launch large-scale attacks.

6.  **Information Manipulation & Interference**: A pattern of behavior that threatens or negatively impacts values, procedures, and political processes through intentional and coordinated manipulation.
    *   **Key Trends**: Information manipulation remains a key element in geopolitical conflicts, with a notable increase in its use in reaction to specific news and major events like elections. The threat of AI-enabled information manipulation is present and evolving.

7.  **Supply Chain Attacks**: Attacks that target less-secure elements in an organization's supply chain to compromise the primary target.
    *   **Key Trends**: Compromises through social engineering are emerging as a significant vector, as seen in the XZ Utils backdoor incident, highlighting the vulnerability of open-source projects.

8.  **Vulnerabilities**: Weaknesses in software or hardware that can be exploited by threat actors to compromise a system.
    *   **Key Trends**: In the reporting period, 19,754 new vulnerabilities were identified, with 9.3% rated as 'critical'. Threat actors, particularly state-nexus groups, continue to focus on exploiting vulnerabilities in edge devices and public-facing applications.

### Relevance to Thesis

The ADLAH (Adaptive Multi-Layered Honeynet Architecture) provides a robust and dynamic platform for studying and mitigating the prime threats identified in the ENISA 2024 report. Its capabilities are directly applicable to understanding the Tactics, Techniques, and Procedures (TTPs) of modern threat actors.

*   **Ransomware**: ADLAH's isolated and instrumented honeypot environments are ideal for safely detonating ransomware payloads. This allows for in-depth analysis of their behavior, including encryption processes, lateral movement across a simulated network, C2 communication, and data exfiltration techniques, without risking an actual production environment.

*   **Malware**: The adaptive nature of ADLAH is a direct counter to polymorphic and evolving malware. By dynamically changing the honeypot's characteristics and services (e.g., OS, open ports, application versions), ADLAH can present a moving target, forcing malware to reveal more of its capabilities. The comprehensive data collection from various layers (network, host, application) facilitates detailed reverse engineering and signature development.

*   **Social Engineering**: While ADLAH cannot prevent initial social engineering attempts like phishing, it excels at studying the post-exploitation phase. Honeypots mimicking employee workstations or specific internal services can be used to observe how attackers who have gained initial access via social engineering attempt to escalate privileges, move laterally, and identify valuable assets within a network.

*   **Threats Against Data**: ADLAH can deploy honeypots that emulate sensitive data stores, such as databases, file servers, or cloud storage buckets. These "honey-data" environments can be used to study the TTPs of data thieves, logging their query patterns, access methods, and data exfiltration routes. This provides invaluable intelligence for protecting real data assets.

*   **Threats Against Availability (DDoS)**: By emulating critical services, ADLAH's honeypots can be used as sensors to attract and analyze DDoS attacks. They can capture attack traffic for forensic analysis, helping to identify attack vectors (e.g., volumetric, protocol-based), botnet characteristics, and source IP addresses. This intelligence can be used to refine DDoS mitigation strategies for production systems.

*   **Supply Chain Attacks & Vulnerabilities**: ADLAH can simulate components of a software supply chain or deploy honeypots running specific versions of vulnerable software (e.g., vulnerable libraries, outdated services). This allows researchers to study how attackers exploit these weaknesses in a controlled environment, providing insights into zero-day exploits and the TTPs used in supply chain compromises.