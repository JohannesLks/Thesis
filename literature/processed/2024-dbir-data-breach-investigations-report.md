# Summary of the 2024 Verizon Data Breach Investigations Report (DBIR)

The 2024 DBIR provides a comprehensive analysis of 30,458 security incidents and 10,626 confirmed data breaches from November 1, 2022, to October 31, 2023. This year's report highlights significant shifts in the threat landscape, particularly the dramatic rise in vulnerability exploitation and the persistent threat of social engineering.

## Key Findings & High-Level Summary

-   **Exploitation of Vulnerabilities:** There was a **180% increase** in breaches initiated by exploiting vulnerabilities, largely driven by the mass exploitation of zero-day flaws like the one in MOVEit. This vector has become a primary pathway for ransomware and extortion attacks.
-   **Ransomware & Extortion:** Ransomware and other extortion techniques were involved in **roughly one-third (32%) of all breaches**. While traditional ransomware saw a slight decline to 23% of breaches, pure extortion attacks (stealing data and threatening to leak it without encryption) rose to 9%.
-   **The Human Element:** The human element was a factor in **68% of breaches**. This figure now excludes malicious insider misuse to better reflect the impact of security awareness. The median time for a user to fall for a phishing email is **less than 60 seconds** (21 seconds to click the link and another 28 seconds to enter data).
-   **Third-Party Breaches:** Breaches involving a third party (including supply chain issues and vulnerabilities in third-party software) accounted for **15% of all breaches**, a 68% increase from the previous year, primarily fueled by the MOVEit incident.
-   **Financially Motivated Attacks:** The majority of attacks remain financially motivated.
    -   The median loss for ransomware/extortion breaches was **$46,000**.
    -   The median transaction amount for a Business Email Compromise (BEC) was **$50,000**.
-   **Pretexting on the Rise:** Pretexting attacks, often leading to BEC, are now more common in breaches than traditional phishing.

## Detailed Analysis: The 4 A's (Actors, Actions, Assets, Attributes)

### Actors (Who?)

-   **External Actors** were responsible for the majority of breaches (65%).
-   **Internal Actors** were involved in 35% of breaches, a significant increase from 20% last year. However, 73% of these were non-malicious errors, a trend made more visible by mandatory breach disclosure laws.
-   **Motivations:** Financial motives overwhelmingly dominate (95%), with Espionage being a distant second (7%).
-   **Generative AI:** The report found no significant evidence of GenAI being used to launch attacks in the wild. Criminal forum discussions are low, and current attack methods (phishing, ransomware) do not appear to need a "sophistication" boost to be effective.

### Actions (How?)

-   **Top Breach Actions:**
    1.  **Use of Stolen Credentials (24%)**: Still the top action, but its lead has narrowed.
    2.  **Ransomware (23%)**: Remains a dominant threat.
    3.  **Extortion (9%)**: Growing rapidly as a tactic by ransomware groups.
    4.  **Exploitation of Vulnerabilities (Exploit vuln)**: Doubled from last year, with web applications being the primary vector.
-   **Initial Entry (Ways-In):** The three primary paths for external attackers into a network are:
    1.  Use of Stolen Credentials
    2.  Exploitation of Vulnerabilities
    3.  Phishing
-   **Vulnerability Exploitation Timeline:** The gap between vulnerability disclosure and exploitation is shrinking. For critical vulnerabilities in the CISA KEV catalog, the median time to the first scan is just **5 days**, while organizations take a median of **55 days** to remediate 50% of these vulnerabilities.

### Assets (What?)

-   **Top Compromised Assets:**
    1.  **Servers:** The most targeted asset, especially file servers (due to MOVEit), web application servers, and mail servers.
    2.  **Person:** The "person" as an asset has grown in involvement due to the rise of social engineering actions like extortion.
    3.  **Media:** Grew due to an increase in Misdelivery errors involving physical documents and faxes.
-   **Compromised Data:**
    -   **Personal Data** is compromised in the vast majority of breaches, often due to disclosure regulations and its ubiquity.
    -   **Credentials** remain a key target (compromised in 50% of Social Engineering breaches and 71% of Basic Web Application Attacks).

### Attributes (CIA Impact)

-   **Confidentiality** was compromised in roughly a third of all incidents.
-   **Availability** was impacted primarily by Denim of Service (DoS) attacks, which remain the top *incident* pattern (but rarely a breach).
-   **Integrity** saw a significant bump in the "Alter behavior" variety, a consequence of successful social engineering where victims are manipulated into taking actions like sending money.

## Incident Classification Patterns

-   **System Intrusion (36% of breaches):** Remains the top pattern, encompassing complex attacks that combine hacking and malware, with ransomware being the primary driver (70% of incidents in this pattern). The MOVEit incident blurred the lines by combining vulnerability exploitation (System Intrusion) with extortion (Social Engineering).
-   **Social Engineering (28% of breaches):** Rose significantly. Pretexting (leading to BEC) and Phishing are the dominant tactics. The MOVEit case contributed a large number of Extortion actions to this pattern.
-   **Basic Web Application Attacks (8% of breaches):** Dropped dramatically from 25% last year. These are simple attacks, usually leveraging stolen credentials against cloud email and collaboration platforms.
-   **Miscellaneous Errors (25% of breaches):** Saw a five-fold increase, largely due to better reporting from new data contributors. **Misdelivery** (sending something to the wrong person) accounts for over 50% of these errors.

## Conclusion & Recommendations

The 2024 DBIR underscores a strategic shift by attackers towards exploiting vulnerabilities in public-facing applications as a highly efficient means to deploy ransomware and extortion campaigns. While technical defenses against these exploits are crucial, the persistent success of social engineering highlights the non-negotiable need for robust security awareness training.

Key defensive strategies include:
-   **Patch Management:** Prioritize patching of known exploited vulnerabilities (KEV catalog) and internet-facing systems.
-   **Multifactor Authentication (MFA):** Implement MFA wherever possible to mitigate the risk of stolen credentials.
-   **Security Awareness:** Train users to identify and report phishing and pretexting attempts. Develop clear procedures for handling financial transactions and extortion demands.
-   **Vendor Risk Management:** Scrutinize the security posture of third-party vendors and software providers, as supply chain vulnerabilities represent a growing threat.