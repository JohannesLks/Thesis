# Summary of NIST SP 800-207: Zero Trust Architecture

This document provides a detailed guide to Zero Trust Architecture (ZTA), a cybersecurity model that shifts from traditional perimeter-based defense to a more dynamic, identity-centric approach.

## Core Concepts

*   **Zero Trust (ZT):** A set of concepts designed to minimize uncertainty in enforcing access decisions. It assumes that the network is always compromised and that trust should never be granted implicitly. Access is granted on a per-request basis with the least privilege necessary.
*   **Zero Trust Architecture (ZTA):** An enterprise's cybersecurity plan that utilizes ZT concepts, including component relationships, workflow planning, and access policies.

## Tenets of Zero Trust

1.  **All data sources and computing services are resources:** No distinction is made between different types of devices or services.
2.  **All communication is secured regardless of network location:** Network location does not imply trust.
3.  **Access to resources is granted on a per-session basis:** Trust is evaluated before each access request.
4.  **Access is determined by dynamic policy:** Policies are based on client identity, application/service, asset state, and other attributes.
5.  **The enterprise monitors the integrity and security posture of all assets:** No asset is inherently trusted.
6.  **All resource authentication and authorization are dynamic and strictly enforced:** This is a continuous cycle of obtaining access, scanning, assessing threats, and re-evaluating trust.
7.  **The enterprise collects as much information as possible about the current state of assets, infrastructure, and communications to improve its security posture.**

## Logical Components of ZTA

*   **Policy Engine (PE):** Makes the decision to grant access.
*   **Policy Administrator (PA):** Establishes and terminates the communication path.
*   **Policy Enforcement Point (PEP):** Enables, monitors, and terminates connections.

## Deployment Models

*   **Device Agent/Gateway-Based:** An agent on the client and a gateway in front of the resource.
*   **Enclave-Based:** A gateway protects a resource enclave (e.g., a data center).
*   **Resource Portal-Based:** A single portal acts as a gateway for subject requests.
*   **Device Application Sandboxing:** Vetted applications run in isolated environments on assets.

## Migration to ZTA

*   Migration is a gradual process, not a complete overhaul.
*   Requires a thorough understanding of assets, subjects, and business processes.
*   The process involves identifying actors, assets, and key processes, formulating policies, identifying solutions, and expanding the ZTA incrementally.

The document emphasizes that ZTA is not a single product but a set of principles and that most organizations will operate in a hybrid mode during the transition.