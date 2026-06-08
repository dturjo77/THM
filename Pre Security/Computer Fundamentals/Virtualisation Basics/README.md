# Lab Room: Virtualisation Basics
### Date Completed: 8 June 2026
### Tools: tryhackme attack box

---

## Task 1: Introduction & The Core Problem

### 1. Traditional IT Inefficiency

Before virtualization, software deployment followed a strict **"One Server = One Application"** blueprint. To ensure reliability and avoid application conflicts, every digital service (e.g., a web server, an active directory, or a mail system) was hosted on its own dedicated physical hardware box.

### 2. The Bottlenecks of Physical Hardware

* **Low Hardware Utilization:** Most individual enterprise applications do not constantly stress a server's hardware. On average, a dedicated physical server operates at only **5% to 20%** of its capacity, meaning vast amounts of CPU cycles, RAM, and storage sit idle.
* **Capital & Operational Expenses (CapEx/OpEx):** Scaling a service meant buying brand-new physical infrastructure. Beyond hardware procurement costs, companies had to pay for server rack space, high-voltage electricity, cooling/air conditioning, and ongoing hardware maintenance.
* **Deployment Latency:** Provisioning a new physical server is slow. It involves ordering hardware, waiting for delivery, physically mounting it in a data center rack, routing cables, and manually installing the operating system.

---

## Task 2: The Virtualization Paradigm Shift

### 1. What is Virtualization?

Virtualization is a technology that abstracts physical hardware from the operating system. It introduces a software layer that tricks a single physical computer into acting as multiple, completely independent virtual computers.

### 2. The Core Mechanism

Instead of an operating system interacting directly with the underlying bare metal, virtualization slices the physical hardware resources (CPU cores, RAM blocks, storage volumes, network interface cards) into smaller virtual pools. This allows multiple distinct operating systems to run concurrently on the exact same physical chassis without interfering with one another.

### 3. The Building Analogy (Deep Dive)

To conceptualize the architectural change, consider real estate:

* **The Physical Server (The Building):** Represents the entire underlying raw hardware framework (the foundation, bricks, plumbing, and main power grid).
* **The Hypervisor (The Building Manager):** The administrative entity that structuralizes the building, safely dividing the massive space into distinct units, allocating utilities fairly, and enforcing boundaries.
* **Lab Machines/VMs (The Apartments):** Completely walled-off spaces. What happens inside Apartment A (e.g., remodeling or a leak) does not affect the tenants in Apartment B.
* **Applications (The Tenants):** The end-users or services residing inside the isolated units, utilizing the allocated resources.

<img width="1047" height="832" alt="image" src="https://github.com/user-attachments/assets/9b303fd1-cd64-4d9e-8bd9-479aea346b04" />

---

## Task 3: Deep Dive into Virtualization Components

### 1. The Hypervisor

The hypervisor (or Virtual Machine Monitor/VMM) is the vital software engine responsible for creating, isolating, and managing virtual environments. It abstracts the physical hardware and maps it to the virtual machines' virtual hardware.

#### Type 1 vs. Type 2 Hypervisors

| Metric / Feature | Type 1 (Bare-Metal) | Type 2 (Hosted) |
| --- | --- | --- |
| **Architecture** | Runs directly on the physical hardware; no host OS underneath. | Runs as an application inside an existing host OS (e.g., Windows/macOS). |
| **Performance** | Highly efficient with minimal overhead; near-native hardware speeds. | Higher latency/overhead due to passing requests through the host OS. |
| **Primary Use Cases** | Enterprise Data Centers, Production Cloud Environments (AWS, Azure), Database Hosting. | Local Testing, Malware Analysis, Software Development, Learning Labs (e.g., Kali Linux). |
| **Examples** | VMware ESXi, Microsoft Hyper-V, Proxmox VE, KVM. | Oracle VirtualBox, VMware Workstation, Parallels Desktop. |

### 2. Lab Machines / Virtual Machines (VMs)

A Virtual Machine is a tightly isolated software container that emulates a complete physical computer.

* **Full Hardware Emulation:** A VM believes it has its own physical motherboard, BIOS, CPU, RAM, and network card, all provided via software translation by the hypervisor.
* **Guest Operating System:** Because it acts like real hardware, a VM can run its own independent **Guest OS** (e.g., a Windows host running a Linux guest VM).
* **Fault Isolation:** VMs are completely decoupled from one another. If a Guest OS inside a VM encounters a kernel panic (BSOD/Crash) or is compromised by malware, the other VMs on the same host continue running completely unaffected.

### 3. Malware Sandboxing & Isolation Safeguards

Virtualization is heavily leveraged in cybersecurity to detonate and analyze malicious files safely inside an isolated Guest VM.

* **Host Infection Risks:** Advanced malware can sometimes detect it is running in a VM and attempt a "VM Escape" to infect the underlying host machine.
* **Defensive Measures:** To mitigate this, security engineers configure VMs with **host-to-guest network isolation** (Host-Only or Internal networking modes), disable shared clipboards/folders, and ensure the Guest OS differs architecturally from the Host OS.

### 4. Containers (Containerization)

Containers represent a step beyond traditional virtualization. Instead of virtualizing the hardware, containers **virtualize the operating system**.

```
+---------------------+     +---------------------+
| App 1   | App 2     |     | App 1   | App 2     |
+---------+-----------+     +---------+-----------+
| Bin/Lib | Bin/Lib   |     | Bin/Lib | Bin/Lib   |
+---------+-----------+     +---------+-----------+
| Guest OS| Guest OS  |     |   Container Engine  |
+---------+-----------+     +---------------------+
|     Hypervisor      |     |  Host OS (Kernel)   |
+---------------------+     +---------------------+
|  Physical Hardware  |     |  Physical Hardware  |
+---------------------+     +---------------------+
  VIRTUAL MACHINES (VMs)          CONTAINERS

```

* **The OS Kernel Sharing Mechanism:** Unlike VMs, containers do not package a whole guest operating system. They sit on top of a single host operating system and **share its Kernel** (the core engine of the OS that manages system resources).
* **Lightweight and Instant Execution:** Because they do not need to boot up a separate OS kernel, containers spin up in milliseconds and consume significantly less RAM and CPU space compared to a VM.
* **OS Constraints:** Since they share the host kernel, containers must be compatible with the host OS architecture (e.g., you cannot natively run a strict Windows-kernel container directly on a bare Linux host kernel).
* **Docker:** The premier open-source platform used to automate the deployment, scaling, and management of applications inside these lightweight containers.
* **Container Images:** Static, read-only blueprint templates containing the application code, runtime engine, system tools, and libraries required to build and deploy a live container.

### 5. Network Ports

Network ports are specialized, logical, numbered endpoints (ranging from `0` to `65535`) utilized by operating systems and applications to channel network traffic. In virtualized and containerized setups, ports allow the host machine to route external internet traffic straight to a specific application running inside a container or VM (known as Port Forwarding).

<img width="852" height="527" alt="image" src="https://github.com/user-attachments/assets/c282c5ba-5917-43a9-9bab-ed3745ef3bb1" />

---

## Task 4: Virtual Infrastructure & Lifecycle Management

Managing an enterprise virtual environment requires specialized command consoles (such as a *Virtualization Manager* dashboard) to supervise three structural scopes:

### 1. The Core Infrastructure Scopes

* **Summary Layer:** Offers a global view of the virtual network's health indexes, total resource allocation percentages, and system alerts.
* **Lab Machine Layer:** Allows administrators to track active VM states and trigger lifecycle actions: *Start, Stop, Pause, Restart/Reboot, Clone, or Delete*.
* **Host Layer:** Monitors the hardware utilization rates of the actual physical servers (the bare metal) holding up the virtual ecosystem.

### 2. VM Incident Response & Troubleshooting

Virtualization tools allow administrators to quickly resolve critical application failures. For example, if a mission-critical service (such as a `Mail-SERVER`) enters a corrupted software or **Error state**, administrators can issue a remote hard reset/reboot command via the hypervisor console to clean the volatile memory cache and safely bring the server back online without visiting a physical data center.

### 3. Elastic Provisioning (Creating a VM)

Virtualization allows rapid, on-demand resource provisioning. Creating a server for a department (e.g., a web server named `Marketing-VM`) requires filling out a software form allocating specific virtual limits:

* **Virtual CPU (vCPU) Cores:** Determining processing power allocation.
* **Virtual Memory (RAM):** Assigning volatile workspace boundaries.
* **Virtual Disk Size:** Allocating virtual storage blocks inside the physical hard drives.

### 4. Host Capacity Planning & Load Balancing

Supervising physical host metrics is essential to prevent system-wide downtime:

* **Under-utilized Hosts (`HV-PROD-01`):** Servers with ample remaining hardware headroom, making them ideal targets to safely host new virtual machines.
* **Over-utilized Hosts (`HV-PROD-02`):** Servers operating close to **100% compute capacity**. These require urgent remediation (such as migrating VMs to another host) to avoid hardware exhaustion and server crashes.
* **Disconnected Hosts (`HV-BACKUP-01`):** Nodes that are offline or decoupled from the cluster network matrix, serving zero active virtual workloads.

---

## Task 5: Macro Benefits 

### 1. Consolidated Summary of Strategic Benefits

* **Exceptional Cost Optimization:** Drastically minimizes physical hardware footprint, cutting data center real estate, power, and maintenance fees.
* **Rapid Scalability & Elasticity:** Resources can be added to an app instantly via software sliders rather than unmounting and modifying physical servers.
* **High Portability & Migration:** Virtual machines and containers exist as software files, allowing them to be backed up, cloned, or moved across global servers instantaneously.
* **Centralized Infrastructure Governance:** Provides a single glass pane dashboard to oversee hundreds of distributed virtual systems at once.

---

<img width="1727" height="827" alt="image" src="https://github.com/user-attachments/assets/1c975c5f-ad81-4a2e-898f-be8f17a0935a" />
<img width="1716" height="820" alt="image" src="https://github.com/user-attachments/assets/1622e5b2-8290-443d-941b-de9b6e9997e9" />
<img width="1082" height="581" alt="image" src="https://github.com/user-attachments/assets/d6167662-4c2e-4030-8e43-c53cf3236274" />
<img width="667" height="387" alt="image" src="https://github.com/user-attachments/assets/e3594f42-e8b3-4ec9-ae83-720a1164182a" />
<img width="1532" height="796" alt="image" src="https://github.com/user-attachments/assets/35fba0a9-5d08-4a1c-9b4a-dcc9e44f4eba" />
<img width="1082" height="807" alt="image" src="https://github.com/user-attachments/assets/e2779f0d-86cc-494e-a6b7-2f6979de85f7" />
