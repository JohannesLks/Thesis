# Summary of IOCTA 2025: Steal, Deal and Repeat

This document summarizes the key findings of the Europol Internet Organised Crime Threat Assessment (IOCTA) 2025, focusing on the trade and exploitation of stolen data by cybercriminals.

## Key Findings

*   **Data as a Core Commodity**: Data theft is a primary threat. Compromised data is a valuable asset for a wide range of criminals, used both as a commodity and as a tool for further criminal activities.
*   **Methods of Attack**: Cybercriminals use a mix of social engineering and system vulnerability exploitation to steal data. Social engineering is a particularly common technique.
*   **Impact of AI**: The use of Large Language Models (LLMs) and other AI technologies is making social engineering attacks more effective and sophisticated.
*   **Initial Access Brokers (IABs)**: A specialized criminal market has emerged for selling access to compromised systems and accounts, with IABs advertising their services on criminal platforms.
*   **Data Broker Operations**: Data brokers are diversifying their operations across multiple platforms, including end-to-end encrypted (E2EE) communication apps, to increase resilience against law enforcement.

## How Data is Exploited

*   **Data as a Target**: Data is the primary goal of many cyber-attacks, used for extortion (ransomware) or to gain unauthorized access to financial and sensitive information.
*   **Data as a Means**: Personal data is used to profile targets for fraud, child sexual exploitation (CSE), and to create more convincing social engineering campaigns.
*   **Data as a Commodity**: Stolen data is sold on criminal platforms to other cybercriminals, who then use it for their own operations.

## How Data is Acquired

*   **Social Engineering**: Techniques like phishing, vishing, and the use of infostealers are used to trick users into revealing credentials or installing malware.
*   **System Vulnerabilities**: Criminals exploit software vulnerabilities, misconfigurations, and weak security to gain access to systems and data.

## Criminal Actors and Marketplaces

*   **Diverse Actors**: The report identifies various criminal actors, from opportunistic individuals to sophisticated groups and hybrid threat actors.
*   **Underground Ecosystem**: A complex ecosystem of criminal marketplaces, forums, and encrypted channels exists for trading stolen data, tools, and services. These platforms are built on reputation and trust.

## Discussion and Conclusions

*   **Societal Impact**: The widespread availability of personal data creates significant risks, especially for vulnerable individuals like children.
*   **Investigative Challenges**: The use of E2EE communication and fragmented data retention policies hinder law enforcement investigations.
*   **AI-Driven Threats**: AI is being used to create more sophisticated attacks, such as deepfakes and advanced social engineering, and to exploit new vulnerabilities like those created by AI code assistants.
*   **Policy Recommendations**: The report calls for lawful access to encrypted communications, harmonized data retention policies, and increased digital literacy to combat these threats.

### Relevance to Thesis

The findings of the IOCTA 2025 report are highly relevant to the ADLAH (Adaptive Multi-Layered Honeynet Architecture) thesis. ADLAH can be used to:

*   **Study Attacker TTPs**: Deploy honeypots that mimic the systems and data targeted by criminals to study their tactics, techniques, and procedures (TTPs) in a controlled environment.
*   **Analyze Malware and Tools**: Safely execute and analyze the malware and tools used by data brokers and IABs, such as infostealers and ransomware.
*   **Simulate Social Engineering Post-Exploitation**: Observe the behavior of attackers who have gained initial access through social engineering to understand their lateral movement and data exfiltration techniques.
*   **Attract and Analyze Attacks**: Use honeypots as sensors to attract and analyze attacks targeting specific vulnerabilities, providing valuable data for developing defensive strategies.