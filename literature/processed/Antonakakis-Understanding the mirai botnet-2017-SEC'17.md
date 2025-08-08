# Understanding the Mirai Botnet

## Abstract

The Mirai botnet, primarily composed of embedded and IoT devices, emerged in late 2016, launching massive distributed denial-of-service (DDoS) attacks against high-profile targets. This paper provides a seven-month retrospective analysis of Mirai's growth, which peaked at 600,000 infections, and a history of its DDoS victims. By combining data from various sources, the paper analyzes the botnet's emergence, the types of devices affected, and the evolution of Mirai variants. The authors argue that Mirai represents a significant shift in botnet evolution, demonstrating that simple malicious techniques can compromise enough low-end devices to threaten even well-defended targets. The paper recommends technical and non-technical interventions and proposes future research directions to address this risk.

## 1. Introduction

Starting in September 2016, a series of massive DDoS attacks targeted Krebs on Security, OVH, and Dyn. The initial attack on Krebs exceeded 600 Gbps, sourced from hundreds of thousands of IoT devices under the control of the Mirai botnet. While other IoT botnets existed, Mirai was the first to become a high-profile DDoS threat. Its success is attributed to efficient spreading via Internet-wide scanning, the use of insecure default passwords in IoT products, and a simple design that allowed it to infect a wide range of heterogeneous devices.

This paper presents a longitudinal study of Mirai from August 1, 2016, to February 28, 2017, using data from network telescopes, Internet-wide scans, IoT honeypots, C2 milkers, DNS traces, and logs from attack victims. The analysis covers Mirai's growth, composition, evolution, and DDoS activities.

## 2. The Mirai Botnet

Mirai is a worm-like malware family that infects IoT devices and uses them for DDoS attacks.

### Timeline of Events

- **August 31, 2016:** First reports of Mirai.
- **Mid-September 2016:** Massive DDoS attacks on Krebs on Security and OVH.
- **September 30, 2016:** Mirai's source code is publicly released on hackforums.net.
- **October 21, 2016:** High-profile attacks target DNS provider Dyn.
- **Late November 2016:** A variant with a CPE WAN Management Protocol (CWMP) exploit causes an outage at Deutsche Telekom.
- **Early 2017:** The author of Mirai is identified, and the Deutsche Telekom attacker is arrested.

### Botnet Structure & Propagation

Mirai spreads through a two-phase process:
1.  **Rapid Scanning Phase:** It sends TCP SYN probes to random IPv4 addresses on Telnet ports 23 and 2323.
2.  **Brute-Force Login Phase:** Upon finding a potential victim, it attempts to log in using a list of 62 hardcoded username/password pairs.

Successful logins are reported to a `report server`. A separate `loader` program then infects the device, downloading and executing architecture-specific malware. The malware conceals its presence, kills competing processes, and then listens for commands from a Command and Control (C2) server while also scanning for new victims.

### Malware Phylogeny

Mirai evolved from the BASHLITE (Gafgyt) malware family. It improved upon BASHLITE by using a larger, more specific dictionary of credentials and a faster, stateless scanning module.

## 3. Methodology

The study uses a variety of data sources:
-   **Network Telescope:** A /8 network telescope to monitor Mirai's scanning activity by fingerprinting its unique TCP sequence number pattern.
-   **Active Scanning:** Data from Censys to identify the manufacturers and models of infected devices.
-   **Telnet Honeypots:** Deployed to collect Mirai binaries and analyze their evolution.
-   **Passive & Active DNS:** Data to track C2 infrastructure and disambiguate different Mirai variants.
-   **Attack Commands:** A C2 "milker" to log DDoS attack commands issued by operators.
-   **DDoS Attack Traces:** Network traces from victims like Akamai, Google Shield, and Dyn to corroborate findings.

## 4. Tracking Mirai’s Spread

### Bootstrapping

Mirai's first scan originated from a single IP at a bulletproof hosting provider on August 1, 2016. It infected nearly 65,000 devices in its first 20 hours, with an initial doubling time of 75 minutes.

### Steady State Size

Mirai's population fluctuated, starting with a steady state of 200,000–300,000 infections, peaking at 600,000 after the introduction of the CWMP exploit in late November 2016, and later declining to around 100,000.

### Global Distribution

-   **Geographic Concentration:** Infections were disproportionately located in Brazil (15.0%), Colombia (14.0%), and Vietnam (12.5%).
-   **Network Concentration:** The top 10 Autonomous Systems (ASes) accounted for 44.3% of infections.
-   **Stability:** The geographic and network distribution was largely stable, except during the initial growth phase and the CWMP exploit period.

### Device Composition

-   **Targeted Devices:** Analysis of hardcoded credentials showed Mirai targeted a wide range of devices, including DVRs, IP cameras, routers, printers, and TV receivers.
-   **Infected Devices:** Active scanning confirmed that security cameras, DVRs, and consumer routers were the most commonly infected devices. The top manufacturers included Dahua, Huawei, ZTE, Cisco, ZyXEL, and MikroTik.
-   **Device Bandwidth:** The low scanning rate of bots (majority under 250 Bps) confirmed that they were low-resource embedded devices.

## 5. Ownership and Evolution

The public release of the source code led to multiple competing Mirai variants.

### Ownership

-   **C2 Infrastructure:** Using DNS expansion, 33 independent C2 clusters were identified, indicating multiple operators.
-   **Estimating Size:** DNS lookup volume for C2 domains was used to estimate the relative size and lifespan of different botnets. The original botnet (Cluster 1) was eventually surpassed by newer variants.

### Evolution

-   **Pre-Source Release:** Early binaries showed maturation, including upgrades from IP-based to domain-based C2s, binary deletion, process ID obfuscation, and more aggressive credential lists.
-   **Post-Source Release:** A rapid emergence of new features was observed, including 48 new credential sets, new infection mechanisms (e.g., scanning ports for CWMP), and the use of Domain Generation Algorithms (DGA). Some variants removed the U.S. Department of Defense IP blocks from their scanning exclusion list.

## 6. Mirai’s DDoS Attacks

Mirai conducted over 15,000 unique DDoS attacks during the monitoring period.

### Types of Attacks

The attacks were a mix of:
-   Volumetric (32.8%)
-   TCP State Exhaustion (39.8%)
-   Application-Layer (34.5%)

Unlike many booter services, Mirai rarely used amplification attacks (only 2.8% of commands), relying instead on the sheer volume of its bots.

### Attack Targets

-   **Victims:** 5,046 victims were targeted, including individual IPs, subnets, and domains.
-   **Distribution:** Targets were spread across 906 ASes and 85 countries, with a heavy concentration in the U.S. (50.3%).
-   **Victim Types:** Victims ranged from game servers (Minecraft, Runescape), telecoms, and anti-DDoS providers to political websites. The pattern of targets resembled that of DDoS-for-hire booter services.
-   **Competition:** Some Mirai C2 servers were themselves the target of Mirai attacks, confirming competition between operators.

### High Profile Attacks

-   **Krebs on Security (Sept 2016):** An unprecedented 623 Gbps attack. Analysis confirmed a 96.4% overlap between attacking IPs and known Mirai bots.
-   **Dyn (Oct 2016):** A series of attacks disrupted major websites. While Mirai was clearly involved (71% IP overlap), other hosts may have participated. The attacks on Dyn appeared to be collateral damage from an attack primarily aimed at PlayStation Network infrastructure.
-   **Lonestar Cell (Oct 2016 - Feb 2017):** A sustained campaign against the Liberian telecom provider, involving 616 separate attacks from a single C2 cluster.

## 7. Discussion

Mirai highlights the security challenges of the IoT ecosystem. The paper suggests several technical and policy-based defenses, drawing lessons from past experiences with desktop worms.

-   **Security Hardening:** IoT devices need to move from default-open ports to default-closed, adopt randomized default passwords, and implement security best practices like ASLR and principle of least privilege.
-   **Automatic Updates:** A critical mechanism for timely patching. This requires secure software architecture and PKI infrastructure. The Deutsche Telekom case serves as a successful example of patching via automatic updates.
-   **Notifications:** An out-of-band channel is needed to alert consumers of vulnerabilities and infections, though this is challenging for interfaceless IoT devices.
-   **Facilitating Device Identification:** A uniform way to identify device model and firmware version (e.g., via MAC address) would help network operators detect vulnerable devices.
-   **Defragmentation:** The push towards common IoT operating systems could help abstract away security complexities.
-   **End-of-Life:** A plan for unsupported devices is crucial, as they pose a growing risk to the Internet.

## 8. Conclusion

Mirai's emergence demonstrated how the absence of basic security practices in the IoT space can create a fragile environment ripe for large-scale abuse. The paper serves as a call to action for industry, academia, and government to address the security, privacy, and safety of an increasingly IoT-enabled world.