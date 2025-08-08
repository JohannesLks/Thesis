# Summary of Kubernetes Hardening Guidance (NSA/CISA)

This document provides a detailed overview of security best practices for hardening Kubernetes clusters, focusing on mitigating threats from supply chain risks, malicious actors, and insider threats.

## 1. Kubernetes Pod Security

-   **Non-Root Containers**: Run applications in containers as non-root users to limit privileges. Use the `runAsUser` directive in the Pod's `securityContext` or build container images to execute as a non-root user.
-   **Immutable File Systems**: Configure containers to use a read-only root file system (`readOnlyRootFilesystem: true`) to prevent modification of the application and reduce the attack surface.
-   **Secure Container Images**:
    -   Use trusted repositories and scan images for vulnerabilities and misconfigurations.
    -   Employ admission controllers to enforce policies, such as only allowing signed images to run.
-   **Pod Security Admission**: Utilize Kubernetes' native `Pod Security Admission` feature to enforce security standards (e.g., `privileged`, `baseline`, `restricted`).
-   **Protect Service Account Tokens**: Disable the automatic mounting of service account tokens (`automountServiceAccountToken: false`) in Pods where the application does not need to interact with the Kubernetes API.

## 2. Network Separation and Hardening

-   **Namespaces**: Partition cluster resources using namespaces, but remember that they are not isolated by default.
-   **Network Policies**:
    -   Implement `NetworkPolicy` objects to control traffic flow between Pods and namespaces.
    -   Start with a default "deny all" policy for ingress and egress traffic and then explicitly allow required connections.
-   **Resource Policies**: Use `LimitRange` and `ResourceQuota` objects to manage and restrict the consumption of compute resources (CPU, memory) and storage on a per-Pod or per-namespace basis.

## 3. Control Plane Hardening

-   **Secure API Server**: Protect the Kubernetes API server (port 6443) with a firewall and restrict access from untrusted networks.
-   **etcd Security**:
    -   Isolate the `etcd` server, allowing access only from the API servers.
    -   Enforce encrypted communication using TLS and encrypt `etcd` data at rest.
-   **Kubeconfig Files**: Protect `kubeconfig` files, as they contain sensitive cluster credentials.
-   **Worker Node Segmentation**: Isolate worker nodes from other network segments to limit the attack surface.

## 4. Authentication and Authorization

-   **Authentication**:
    -   Disable anonymous login (`--anonymous-auth=false`).
    -   Implement a strong authentication method for users (e.g., client certificates, OIDC tokens).
-   **Role-Based Access Control (RBAC)**:
    -   Enable and enforce RBAC to apply the principle of least privilege.
    -   Create specific `Roles` and `ClusterRoles` for users, groups, and service accounts.
    -   Use `RoleBindings` and `ClusterRoleBindings` to assign these roles.

## 5. Audit Logging and Threat Detection

-   **Enable Audit Logging**: Kubernetes audit logging is disabled by default. Enable it and configure a comprehensive audit policy.
-   **Log Aggregation**: Collect logs from all components (control plane, nodes, applications) and aggregate them in a central, external location (e.g., a SIEM).
-   **Threat Detection**:
    -   Establish baselines for normal activity to identify anomalies.
    -   Monitor for suspicious activities, such as unusual API requests, privileged container execution, and unexpected resource consumption spikes.
-   **Alerting**: Configure automated alerts for security-critical events.

## 6. Upgrading and Application Security Practices

-   **Regular Updates**: Keep all components of the Kubernetes environment (Kubernetes itself, container runtimes, OS, applications) patched and up-to-date.
-   **Vulnerability Scanning**: Regularly scan for vulnerabilities and misconfigurations.
-   **Remove Unused Components**: Uninstall old and unused components to reduce the attack surface.

### Relevance to Thesis

The hardening guidelines outlined in the NSA/CISA document are directly applicable to the **ADLAH (Adaptive Multi-Layered Honeynet Architecture)** thesis. The ADLAH system relies on a Kubernetes-based infrastructure to orchestrate and manage its honeypots and other components. Securing this underlying orchestration layer is critical to the integrity and effectiveness of the entire honeynet.

Here's how these security principles apply to ADLAH:

1.  **Preventing Orchestration Layer Compromise**: The primary goal is to prevent attackers from compromising the Kubernetes control plane or worker nodes. A compromised orchestration layer would allow an attacker to:
    *   Disable or manipulate the honeypots, rendering the honeynet ineffective.
    *   Gain insight into the true nature of the ADLAH system, breaking the deception.
    *   Use the ADLAH infrastructure as a pivot point to attack other internal or external systems.

2.  **Applying Specific Controls to ADLAH**:
    *   **Pod Security**: Each honeypot instance within ADLAH runs as a Pod. These Pods must be hardened by running as non-root users, using immutable file systems where possible, and applying strict `Pod Security` standards. This contains the attacker within the honeypot and prevents them from "breaking out" to the underlying worker node.
    *   **Network Policies**: Network policies are crucial for isolating the honeypots from the ADLAH management components (e.g., the RL agent, logging infrastructure). For instance, a network policy can ensure that a compromised honeypot cannot directly communicate with the Kubernetes API server or other sensitive internal services.
    *   **Control Plane Hardening**: The ADLAH's Kubernetes control plane must be rigorously secured. Access to the API server should be restricted, and `etcd` must be encrypted and isolated. This prevents an attacker from seizing control of the entire honeynet orchestration.
    *   **RBAC**: The principle of least privilege must be enforced for all components of the ADLAH system. The RL agent, for example, should only have the permissions it needs to deploy, configure, and destroy honeypot Pods, and nothing more. Service account tokens should be managed carefully.
    *   **Audit Logging**: Detailed audit logs are a.mdssential for the "Adaptive" aspect of ADLAH. By logging all API interactions and system calls, the ADLAH system can detect anomalous behavior not just within the honeypots, but also at the orchestration level. These logs can serve as a valuable data source for the RL agent to learn about sophisticated attackers who might be attempting to target the Kubernetes infrastructure itself.

In summary, the security of the Kubernetes infrastructure is not just a prerequisite for the ADLAH system but an integral part of its architecture. By implementing the hardening guidelines from the NSA/CISA document, the ADLAH thesis can ensure that its orchestration layer is resilient against attacks, thereby maintaining the integrity of its deception and data collection capabilities.