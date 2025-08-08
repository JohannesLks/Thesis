# Summary of "A View of Cloud Computing"

## Introduction

The paper "A View of Cloud Computing" by Michael Armbrust et al. aims to clarify the concept of cloud computing, identify its key opportunities and obstacles, and provide a clear definition to reduce confusion. Cloud computing is presented as the realization of computing as a utility, transforming the IT industry by making software available as a service (SaaS) and changing how IT hardware is designed and purchased.

## Key Concepts

### Defining Cloud Computing

- **Cloud Computing**: Refers to both applications delivered as services over the Internet (SaaS) and the hardware and systems software in the data centers that provide those services.
- **Public Cloud**: A cloud made available to the general public in a pay-as-you-go manner. The service sold is called **utility computing**.
- **Private Cloud**: Internal data centers of a business or organization that are large enough to benefit from cloud computing advantages but are not available to the public.
- **Hybrid Cloud**: A composition of two or more clouds (private, community or public) that remain unique entities but are bound together, offering the benefits of multiple deployment models.

The authors argue that cloud computing is the sum of SaaS and utility computing. The key enabler for cloud computing was the development of extremely large-scale, commodity-computer data centers at low-cost locations, leading to significant cost reductions (factors of 5 to 7) in electricity, network bandwidth, operations, software, and hardware.

### New Aspects of Cloud Computing

1.  **Illusion of Infinite Computing Resources**: Available on demand, eliminating the need for users to plan for provisioning far in advance.
2.  **No Up-front Commitment**: Allows companies to start small and increase resources as needed.
3.  **Pay-as-you-go**: Users can pay for computing resources on a short-term basis (e.g., per hour), rewarding conservation.

### Classes of Utility Computing

Different utility computing offerings are distinguished by the level of abstraction of the cloud system software and the level of resource management.
- **Amazon EC2**: Low-level abstraction where instances look like physical hardware, giving users control over the software stack. This makes automatic scalability and failover difficult for the provider.
- **Google AppEngine**: A domain-specific platform for web applications with a stateless computation tier and a stateful storage tier, enabling automatic scaling and high availability.
- **Microsoft Azure**: An intermediate solution between EC2 and AppEngine, offering more flexibility than AppEngine but still constraining the user's choice of storage and application structure.

## Economics of Cloud Computing

Cloud computing is compelling for several use cases:
- **Variable Demand**: For services with fluctuating demand, cloud computing avoids the underutilization of resources that occurs when provisioning for peak load.
- **Unknown Demand**: Startups can scale resources up or down based on actual demand, mitigating the risks of overprovisioning (wasted resources) and underprovisioning (lost customers).
- **Batch Analytics**: The "cost associativity" of cloud computing allows for faster computation by using many servers for a short time at the same cost as using one server for a long time.

The economic benefit is not just about converting CapEx to OpEx but is about **elasticity** and the **transference of risk**.

## Top 10 Obstacles and Opportunities

The paper identifies and ranks ten obstacles to the growth of cloud computing, each paired with an opportunity.

1.  **Availability/Business Continuity**:
    - **Obstacle**: Users worry about the availability of cloud services. A single provider represents a single point of failure.
    - **Opportunity**: Use multiple cloud providers and standardize APIs to allow for migration.

2.  **Data Lock-in**:
    - **Obstacle**: Proprietary storage APIs make it difficult to move data and applications between cloud providers.
    - **Opportunity**: Standardize APIs to enable hybrid cloud computing and surge computing, where a public cloud can handle overflow from a private data center.

3.  **Data Confidentiality and Auditability**:
    - **Obstacle**: Security concerns about who can access data, and the need to comply with regulations like HIPAA and Sarbanes-Oxley.
    - **Opportunity**: Use encryption, VLANs, and firewalls. Providers can offer auditable services. User-level encryption is a key defense.

4.  **Data Transfer Bottlenecks**:
    - **Obstacle**: High costs and low speeds for transferring large datasets over the Internet.
    - **Opportunity**: Physically shipping disks (e.g., using FedEx) can be more efficient for large, delay-tolerant data transfers. Higher bandwidth switches will also help.

5.  **Performance Unpredictability**:
    - **Obstacle**: I/O sharing in virtualized environments leads to variable performance, especially for disk and network I/O.
    - **Opportunity**: Improve VM support in hardware, use flash memory to reduce I/O interference, and implement gang scheduling for HPC applications.

6.  **Scalable Storage**:
    - **Obstacle**: Creating a storage system that offers durability, high availability, and rich query capabilities while also scaling up and down on demand is an open research problem.
    - **Opportunity**: Invent a scalable storage solution that combines the benefits of traditional databases with the elasticity of the cloud.

7.  **Bugs in Large-Scale Distributed Systems**:
    - **Obstacle**: Debugging is difficult as bugs may only appear at scale in production environments.
    - **Opportunity**: Leverage virtualization to create debuggers that can capture valuable information from distributed VMs.

8.  **Scaling Quickly**:
    - **Obstacle**: How to automatically scale resources in response to load to save money without violating SLAs.
    - **Opportunity**: Develop auto-scaling solutions that use machine learning to predict load and adjust resources. Fast snapshot/restart tools can encourage resource conservation.

9.  **Reputation Fate Sharing**:
    - **Obstacle**: The actions of one customer (e.g., sending spam) can harm the reputation of other customers on the same cloud (e.g., by getting IPs blacklisted).
    - **Opportunity**: Create reputation-guarding services, similar to "trusted email" services.

10. **Software Licensing**:
    - **Obstacle**: Traditional software licensing models (per-machine) are not suited for the pay-as-you-go nature of cloud computing.
    - **Opportunity**: Move to pay-for-use licensing models.

## Conclusion

The authors predict that cloud computing will continue to grow and that developers, infrastructure providers, and hardware designers need to adapt:
- **Application Software**: Must be designed to scale up and down rapidly and use pay-for-use licensing.
- **Infrastructure Software**: Must be aware it is running on VMs and incorporate metering and billing.
- **Hardware Systems**: Should be designed at the scale of a container (multiple racks), with a focus on energy proportionality, and better support for VMs, flash memory, and high-bandwidth networking.