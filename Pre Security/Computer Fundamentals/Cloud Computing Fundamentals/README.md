# Lab Room: Cloud Computing Fundamentals
### Date Completed: 9 June 2026
### Tools: tryhackme attack box

---

## ## Task 1: Introduction & The Core Problem Statement

### 1. Traditional On-Premises Limitations (The "Why Cloud?" Dilemma)

Before modern infrastructure, deploying an application meant hosting it on physical local hardware (e.g., a personal computer or a local server room). This approach suffers from several architectural bottlenecks:

* **Network Latency & Lag:** Physical distance between the hosting server and the end-user introduces high latency. Network packets must traverse multiple geographical hops, creating a poor user experience for international traffic.
* **Vertical/Horizontal Scalability Caps:** Physical hardware has fixed CPU, RAM, and bandwidth capacities. A sudden influx of concurrent users exhausts these resources, leading to bottlenecking, memory leaks, and Denial of Service (DoS) conditions.
* **Single Point of Failure (SPOF):** Local setups lack hardware and power redundancy. If the host machine suffers a power outage, hardware failure, or OS crash, the entire application goes offline (downtime).

### 2. Foundational Cloud Enablers: Virtualization and Containerization

The cloud abstractifies physical hardware using two key architectural technologies:

* **Virtualization:** A software technology (driven by a **Hypervisor**) that allows a single physical machine to be split into multiple isolated **Virtual Machines (VMs)**. Each VM runs its own independent Guest Operating System, maximizing physical hardware utilization.
* **Containerization:** A lightweight alternative to VMs (e.g., Docker) that shares the host OS kernel but isolates the application execution space. It allows rapid environment provisioning and highly efficient resource utilization.

---

## ## Task 2: Cloud Computing Overview & Taxonomy

### 1. Architectural Characteristics & Benefits

Cloud computing transforms IT infrastructure from a capital expenditure (**CapEx**) to an operational expenditure (**OpEx**) through specific key traits:

* **Scalability:** The ability to scale resources. This includes **Vertical Scaling (Scale-up/down)**—adding more power (CPU/RAM) to an existing node, and **Horizontal Scaling (Scale-out/in)**—adding more instances/machines to handle distributed loads.
* **On-Demand Self-Service:** Infrastructure provisioning (servers, network topologies, databases) is fully automated via APIs or web consoles, eliminating the need for human intervention or manual hardware setups.
* **Pay-as-You-Go Pricing Model:** Metered billing structures where resources are billed by the second or minute. Users only pay for active compute time, storage volume, or data egress.
* **High Availability (HA) & Fault Tolerance:** Cloud data centers are built with redundant power grids, network paths, and storage arrays. Applications can be mirrored across different physical buildings to prevent downtime even during catastrophic hardware failure.
* **Global Access (Edge Networks):** Providers deploy data centers worldwide, allowing applications to be hosted closer to target regions or cached via **Content Delivery Networks (CDNs)** to reduce latency.

### 2. Cloud Deployment Models

The deployment model dictates who owns, manages, and has access to the physical cloud infrastructure:

| Cloud Deployment Model | Architectural Definition | Primary Use Case |
| --- | --- | --- |
| **Public Cloud** | Infrastructure owned and operated by a third-party provider (e.g., AWS, GCP). Resources are multi-tenant (shared among different customers via virtualization logical isolation). | Startups, SaaS platforms, and standard global web apps requiring low entry barriers. |
| **Private Cloud** | Infrastructure dedicated exclusively to a single organization. It can be physically located on-site or hosted by a third party, providing absolute control. | Highly regulated sectors like Banking, National Security, and Healthcare. |
| **Hybrid Cloud** | An orchestrated environment combining Public and Private clouds. Data and application components communicate seamlessly between the two distinct environments. | E-commerce sites handling burstable traffic spikes on public clouds while storing core customer transaction databases in a private cloud. |

### 3. Cloud Service Models (The Responsibility Spectrum)

Service models define the boundary of management responsibility between the cloud vendor and the consumer.

```
Traditional On-Premises      Infrastructure (IaaS)         Platform (PaaS)            Software (SaaS)
+-----------------------+   +-----------------------+   +-----------------------+   +-----------------------+
|   [Applications]      |   |   [Applications]      |   |   [Applications]      |   |   (Managed by Vendor) |
|   [Data & Runtime]    |   |   [Data & Runtime]    |   |   (Managed by Vendor) |   |   (Managed by Vendor) |
|   [Operating System]  |   |   [Operating System]  |   |   (Managed by Vendor) |   |   (Managed by Vendor) |
|   [Virtualization]    |   |   (Managed by Vendor) |   |   (Managed by Vendor) |   |   (Managed by Vendor) |
|   [Hardware/Network]  |   |   (Managed by Vendor) |   |   (Managed by Vendor) |   |   (Managed by Vendor) |
+-----------------------+   +-----------------------+   +-----------------------+   +-----------------------+

```

* **Infrastructure as a Service (IaaS):**
The provider delivers fundamental raw compute, storage block volumes, and networking. The user retains complete control over the configuration of the Operating System, network firewalls, runtimes, and application binaries.
* *Analogy:* Renting an unfurnished apartment. You own and arrange everything inside.


* **Platform as a Service (PaaS):**
The vendor manages the underlying physical infrastructure, hypervisors, and Operating Systems. They provide a managed execution runtime environment (e.g., Node.js, Python, Java platforms). The developer only worries about deploying their source code.
* *Analogy:* Staying in a fully furnished hotel room where maintenance is handled by the staff.


* **Software as a Service (SaaS):**
A complete, fully functional end-user software application delivered entirely over the web. The end-user has zero visibility or control over the infrastructure, backend code, or system configuration (e.g., Gmail, Zoom).
* *Analogy:* Dining at a restaurant where food is cooked, served, and cleaned up for you.



### 4. Global Market Ecosystem & Real-World Use Cases

* **Amazon Web Services (AWS):** The market pioneer and leader, featuring the most exhaustive array of mature enterprise cloud services.
* **Microsoft Azure:** Strongly optimized for hybrid deployments, Active Directory integrations, and legacy Windows Server enterprise environments.
* **Google Cloud Platform (GCP):** Renowned for native containerization support (Kubernetes architecture), big data analytics, and advanced AI systems.

**Real-World Architectural Footprints:**

* *Netflix:* Operates completely on AWS, taking advantage of massive horizontal compute scaling to process heavy video encoding pipelines and stream content to millions globally.
* *Spotify & Instagram:* Utilize cloud object storage and distributed microservices architectures to process, store, and serve exabytes of media data on demand.

---

## ## Task 3: Deploying a Cloud Instance (Hands-on Lab Mechanics)

This task analyzed the direct deployment mechanics of setting up isolated instances within a simulated Amazon Web Services environment.

### 1. Key AWS Conceptual Framework

* **EC2 (Elastic Compute Cloud):** This is the core AWS IaaS offering. It provides resizeable virtual computing capacity (instances) acting as virtual servers in the cloud.
* **Instance Types:** Categorized groupings of hardware configurations optimized for distinct computing requirements:
* `t3.micro`: A burstable, general-purpose instance type designed for low-baseline compute needs (low cost, ideal for lightweight application frontends or testing).
* `m5.large`: A balanced general-purpose instance featuring significantly higher physical vCPUs and RAM allocations (designed for sustained, compute-heavy application tasks).


* **Geographical Regions:** Completely isolated geographic locations (e.g., `us-east-1` or `eu-west-1`) composed of multiple independent Availability Zones. Selecting a region is crucial for adhering to compliance laws and optimizing user latency.

### 2. Lab Lifecycle & Cost Optimization Architecture

The lab demonstrated a multi-tier infrastructure setup representing an active development cycle:

1. **Provisioning Tier:**
* $1 \times$ `t3.micro` instance named `application-interface` (Status: `Running`). Acts as the web gateway.
* $2 \times$ `m5.large` instances named `study-machine-1` and `study-machine-2` (Status: `Running`). Used as isolated sandboxes for student cybersecurity labs.


2. **Cost Dynamics Analysis:**
Running continuous compute instances incurs explicit baseline resource costs. When an instance status is set to **`Running`**, the user pays for the allocated vCPU/RAM reservation by the minute.
3. **Optimizing Costs via State Modification:**
By selecting the `study-machine-1` and `study-machine-2` instances and executing a **`Stop`** command, the instance state transitions from `Running` to `Stopped`.
* **Under the Hood:** The hypervisor releases the physical vCPU and RAM reservations on the underlying cloud host, **halting the compute billing metrics entirely**. The storage state is preserved on block volumes, demonstrating how cloud environments prevent waste during periods of inactivity.



---

## ## Task 4: Conclusion & Knowledge Synthesis

This task formalized the theoretical and operational framework established throughout the room. It served as a final synthesis of core competencies, cementing the definitions of Public/Private/Hybrid topologies, IaaS/PaaS/SaaS abstraction layers, and AWS EC2 instantiation frameworks.

The ultimate takeaway confirms that modern IT architectural engineering relies on moving away from rigid physical hardware towards dynamic, software-defined infrastructure to maximize efficiency and reach a global scale.

<img width="832" height="632" alt="image" src="https://github.com/user-attachments/assets/490bb23f-5ca1-4bb5-8de0-f598aa21161e" />
<img width="1202" height="767" alt="image" src="https://github.com/user-attachments/assets/09f2ef1f-bc87-48a0-aa59-511365e321be" />
<img width="1002" height="717" alt="image" src="https://github.com/user-attachments/assets/2edf43a4-9000-4025-91f5-a5735ee60847" />

<img width="1906" height="775" alt="image" src="https://github.com/user-attachments/assets/35ae179d-1b1c-41be-8d04-5a13a3e5eec6" />
<img width="1892" height="767" alt="image" src="https://github.com/user-attachments/assets/0d7566d2-2c86-4cda-ae1d-9ddca69439bb" />
<img width="996" height="567" alt="image" src="https://github.com/user-attachments/assets/64659387-871a-4b33-b5b1-d4cd51764798" />
<img width="996" height="535" alt="image" src="https://github.com/user-attachments/assets/00b48e6d-23b4-4dea-8072-f489988375a0" />
<img width="997" height="586" alt="image" src="https://github.com/user-attachments/assets/4d6c9edb-984f-4714-9043-4dffedb04400" />
<img width="995" height="525" alt="image" src="https://github.com/user-attachments/assets/e5caa929-5c80-4dfb-97be-53cd17190cbe" />
