
# Lab Room: Operating Systems: Introduction
### Date Completed: 11 June 2026
### Tools: tryhackme attack box


---

## Task 1: Introduction & Foundational Concepts

### 1. Definition of an Operating System (OS)

An **Operating System (OS)** is the low-level, core software stack that establishes an abstraction layer between the physical hardware components of a computer and the high-level application software or end-user.

* Without an OS, programmers would be forced to write custom machine code to interact directly with physical circuits, storage sectors, and display buffers for every single application.
* **The Layered Architecture:**

$$\text{User} \longleftrightarrow \text{Applications} \longleftrightarrow \text{Operating System (OS)} \longleftrightarrow \text{Physical Hardware}$$



### 2. The Five Core Duties of an OS

To maintain system stability and efficiency, the OS operates as the primary resource allocator. Its core responsibilities include:

* **Processor Management (CPU Scheduling):** The OS manages how execution time is divided among active programs on the Central Processing Unit (CPU). Through algorithms like Round-Robin or Priority Scheduling, it handles **multitasking**—swapping execution threads so rapidly that it creates the illusion of simultaneous execution.
* **Memory Management:** The OS is responsible for tracking every byte of Random Access Memory (RAM). It dynamically allocates specific memory addresses to running processes and isolates them to prevent data corruption. When a process terminates, the OS executes garbage collection to reclaim that memory space. If physical RAM runs low, it utilizes **Virtual Memory** (swapping data to a dedicated portion of the storage drive).
* **Storage & File System Management:** It defines how data is structured, labeled, and retrieved from non-volatile storage media (SSD/HDD). The OS enforces structural rules using file systems such as **NTFS** (Windows), **ext4** (Linux), or **APFS** (macOS), tracking file metadata, absolute paths, and access permissions.
* **Device Management (Hardware Abstraction):** The OS leverages **Device Drivers**—specialized software modules that translate generic OS input/output commands into the specific hardware language required by peripherals (e.g., network interface cards, graphics cards, keyboards).
* **User Interface (UI) Provisioning:** It exposes an environment enabling humans to issue commands to the system. This is delivered either via a visual layout (Graphical User Interface) or a text-based syntax engine (Command-Line Interface).

---

## Task 2: System Privilege Layers & Core Architecture

### 1. Privilege Separation: Kernel Space vs. User Space

Modern CPU architectures support multiple execution modes (often referred to as Protection Rings). The OS splits software execution into two fundamental spaces to preserve security and fault tolerance:

```
+------------------------------------------------------------+
| USER SPACE (Ring 3)                                        |
| Applications (Browsers, Text Editors, Tools)                |
+------------------------------------------------------------+
       |
       |  [ System Call (syscall) ] 
       v
+------------------------------------------------------------+
| KERNEL SPACE (Ring 0)                                      |
| Monolithic/Microkernel Core -> Direct Hardware Control     |
+------------------------------------------------------------+

```

* **Kernel Space (Ring 0):** The highly privileged execution environment where the absolute core of the operating system (the **Kernel**) resides. The kernel has unrestricted, raw access to the CPU instruction set, physical memory addresses, and underlying hardware registers. A failure inside kernel space typically triggers a catastrophic system crash (e.g., Kernel Panic in Linux or Blue Screen of Death in Windows).
* **User Space (Ring 3):** A sandboxed, restricted execution layer where standard, non-privileged user applications run. Software executing in user space is strictly prohibited from touching the hardware directly. If a browser wants to read a file from the disk, it must pass a formal request—a **System Call (syscall)**—to the kernel, which vets the request before executing it on the app's behalf. This architecture ensures that if a user application crashes, it can be cleanly terminated without affecting the rest of the system.

### 2. Built-in OS Security Mechanisms

Before any third-party security agent or firewall executes, the OS enforces foundational security policies:

* **Authentication:** Validating identity claims using credential verification (passwords, cryptographic keys, or biometrics).
* **Permissions / Access Control:** Evaluating Access Control Lists (ACLs) to determine whether an authenticated user or process has the authority to Read ($r$), Write ($w$), or Execute ($x$) a specific resource.
* **Process Isolation:** Encapsulating individual running applications within isolated memory boundaries so that one malicious or compromised process cannot scrape or overwrite the data of an adjacent process.


<img width="1562" height="842" alt="image" src="https://github.com/user-attachments/assets/9f858479-b2f1-4c0e-aa80-7c4771b30c07" />
<img width="1367" height="747" alt="image" src="https://github.com/user-attachments/assets/8865640a-5b6d-4944-8a10-1d7fecf2e755" />
<img width="1377" height="837" alt="image" src="https://github.com/user-attachments/assets/0a7ef8b4-face-4ac4-94d2-e0fd09875f93" />
<img width="1390" height="312" alt="image" src="https://github.com/user-attachments/assets/1ee49bd2-c38b-4ba6-b3ec-a7e61a5f43ee" />

---

## Task 3: OS Interaction Landscape & Practical Lab Analysis

### 1. Interface Parity: GUI vs. CLI

* **Graphical User Interface (GUI):** A visual, intuitive paradigm representing system constructs as icons, menus, and windows. It relies heavily on mouse/touch inputs. While highly user-friendly, it lacks trivial automation capabilities and consumes significant system overhead.
* **Command-Line Interface (CLI):** A lean, text-based shell prompt accepting explicit lexical commands governed by strict syntactical rules. It allows for raw precision, headless scripting, and rapid programmatic administration, making it the industry standard for network engineers, system administrators, and security professionals.

### 2. The Operating System Ecosystem Matrix

Operating systems are highly specialized depending on their target hardware constraints and deployment environments:

| OS Classification | Primary Use Case | Architectural Strengths | Common Real-World Examples |
| --- | --- | --- | --- |
| **Desktop OS** | Workstations, Personal PCs, Gaming, Content Creation. | Heavy emphasis on rich GUI rendering, broad consumer application compatibility, multi-tasking optimization. | Windows 11, macOS (Sequoia), Linux Distributions (Ubuntu Desktop, Fedora). |
| **Server OS** | Enterprise Networking, Database Hosting, Cloud Backends. | Headless deployment (no GUI), optimized for maximum uptime, high concurrency, robust remote access (SSH). | Windows Server 2025, Ubuntu Server, Red Hat Enterprise Linux (RHEL). |
| **Mobile OS** | Smartphones, Tablets, Wearables. | Built-in app sandboxing, advanced power-efficiency states, touch-centric UI, cellular network integration. | Android (built on modified Linux kernel), iOS. |
| **Embedded OS / IoT** | Routers, Smart TVs, Industrial Controllers, Automotive. | Extremely minimal storage and memory footprint, compiled tailored binaries optimized for fixed microcontrollers. | OpenWrt, Embedded Linux, Real-Time OS (FreeRTOS, VxWorks). |
| **Virtual / Cloud OS** | Virtual Machines, Microservices, Containers. | Highly lightweight, stripped of unneeded hardware drivers, optimized for rapid elasticity and minimal boot times. | Amazon Linux, Alpine Linux (Container-optimized), Rocky Linux. |

---

### 3. Practical Lab Deconstruction & Forensic Walkthrough

During the practical session, you interacted via a CLI environment with a remote machine running **Ubuntu Linux** to locate a target file. The following sequence breaks down the exact technical commands and methodology you used to escalate privileges and extract the flag:

#### Command Reference & Execution Context

* **`ls /home`**
* *Purpose:* Lists the directory contents of the `/home` path, which is where local user profile directories are traditionally mapped in Linux systems.
* *Analysis:* The terminal returned `alex`, `guest`, and `ubuntu`, establishing that **three (3)** user profiles existed on the machine.


* **`su - alex`**
* *Purpose:* **S**ubstitute **U**ser command. The hyphen (`-`) specifies that a login shell should be initiated, pulling Alex's unique environment variables.
* *Analysis:* This initially failed with an authentication error because the current user did not possess Alex’s pre-existing account password.


* **`sudo passwd alex`**
* *Purpose:* Invokes Superuser Do (`sudo`) to run the password modification utility (`passwd`) with administrative privileges. This allowed the root-privileged user to override and overwrite the password for the account `alex` without knowing the old password.
* *Analysis:* The Linux security module intercepted several initial weak inputs, throwing dictionary-check alerts (`BAD PASSWORD: too simplistic/systematic`). A compliant, complex string was eventually passed, resulting in a successful change (`passwd: password updated successfully`).


* **`su - alex` (Second Attempt)**
* *Purpose:* Re-executed the user substitution command.
* *Analysis:* Providing the newly verified password successfully dropped the session into the target shell environment: `alex@tryhackme:~$`.


* **`cd Documents`**
* *Purpose:* **C**hange **D**irectory. Moves the working directory pointer into the relative path folder named `Documents`.


* **`ls`**
* *Purpose:* Discovers visible files in the current working directory, revealing the presence of a text file named `note.txt`.


* **`cat note.txt`**
* *Purpose:* **Cat**enate. Reads the binary stream of the text file and flushes the string payload directly to stdout (the screen).
* *Analysis:* The data output revealed the definitive verification flag:

$$\text{THM\{new\_pc\_for\_free!\}}$$


<img width="1552" height="402" alt="image" src="https://github.com/user-attachments/assets/689dfdbd-413b-4ea6-94e3-48ba875bc7ae" />
<img width="1917" height="902" alt="image" src="https://github.com/user-attachments/assets/29ef68a5-2cee-4b39-b12b-7e9a7a463f8b" />


---

## Task 4: Key Terminology Summary

**Operating system (OS)**
The core software that manages hardware, applications, and all system resources.
**Kernel space**
The OS’s highly privileged area with direct hardware access, and the home of the kernel, which directly manages hardware and system resources.
**User space**
The area where regular applications run with limited permissions for safety and system stability.
**Graphical user interface (GUI)**
The visual part of the OS, windows, icons, and menus, that lets you interact through clicking and tapping.
**Command-line interface (CLI)**
A text-based interface where you type commands to control the system with precision and speed.


