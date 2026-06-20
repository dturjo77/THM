Pre Security
Attacks and Defenses
The CIA Triad


---

### 📌 Task 1: Introduction & High-Level Objectives

Task 1 serves as the bridge between general computing foundations and the specialized domain of information security.

* **The Transition:** Moving from understanding *how* technology works (operating systems, networks, protocols) to understanding *how to protect* that technology.
* **The Core Question:** What does cyber security actually protect inside the digital world? It shifts focus away from the vague idea of "protecting systems" and zeros in on protecting specific **data conditions**.
* **Learning Outcomes:** Developing a foundational vocabulary, understanding the goals of the core pillars of security, and learning how to evaluate real-world scenarios to make risk-based decisions.

---

### 📌 Task 2: In-Depth Analysis of the CIA Triad

The **CIA Triad** is the definitive framework around which all cybersecurity policies, architectural designs, and defensive protocols revolve. Every security incident is defined by which leg of this triad was broken.

```
            [ Confidentiality ]
                   /   \
                  /     \
                 /       \
      [ Integrity ] --- [ Availability ]

```

#### 1. Confidentiality (Secrecy & Privacy)

* **Definition:** Ensuring that sensitive data is hidden from and completely inaccessible to unauthorized individuals, processes, or devices.
* **Real-World Analogy:** A private conversation between two individuals in a secure room where eavesdroppers cannot listen.
* **Digital Analogy:** Logging into an account over an unencrypted public Wi-Fi network where an attacker sniffs and intercepts your login credentials.
* **Implementation & Defenses:**
* **Encryption:** Transforming readable data (plaintext) into an unreadable format (ciphertext) using cryptographic protocols (e.g., SSH for secure remote access, HTTPS for secure web traffic).
* **Access Controls:** Utilizing strict Access Control Lists (ACLs), strong password policies, and Multi-Factor Authentication (MFA) to strictly limit who can view data.


* **Practical Scenarios:**
* *Confidentiality Breached:* Gmail credentials written on sticky notes left visible on an office desk; personal documents leaked or publicly accessible on the internet.
* *Confidentiality Maintained:* Internal company documents accessible *only* to employees who strictly need them to perform their jobs.



#### 2. Integrity (Accuracy & Trustworthiness)

* **Definition:** Ensuring that digital information remains completely accurate, unaltered, and untampered with during storage, processing, and transit.
* **Real-World Analogy:** A teacher assigning a grade on an exam paper, and an unauthorized entity modifying that grade before it reaches the final registrar database.
* **Digital Analogy:** Initiating an electronic bank transfer, only for a malicious actor to intercept the network packet and alter the destination account number mid-transit to steal the funds.
* **Implementation & Defenses:**
* **Hashing:** Passing data through an algorithm to generate a unique digital fingerprint (e.g., MD5, SHA-256). If even a single bit of the data changes, the hash changes completely, alerting administrators to tampering.
* **Digital Signatures & Version Control:** Ensuring accountability and tracking authorized revisions.


* **Practical Scenarios:**
* *Integrity Breached:* Student attendance records modified after being locked by an instructor; an online shopping cart order price modified by a user before checking out.
* *Integrity Maintained:* System data or configurations changed through explicit, authorized administrative approval workflows.



#### 3. Availability (Reliability & Access)

* **Definition:** Ensuring that networks, systems, applications, and data are consistently accessible and usable by authorized personnel whenever they are required.
* **Real-World Analogy:** A physical bank vault keeping your cash incredibly secure, but the branch is locked shut and completely closed during standard business hours due to a total power failure. The asset is safe, but useless because it is unavailable.
* **Digital Analogy:** Attackers launching a Denial of Service (DoS) or Distributed Denial of Service (DDoS) attack, flooding a web server with millions of bogus requests until it exhausts its resources and crashes.
* **Implementation & Defenses:**
* **Redundancy:** Implementing backup power generators, secondary network links, and redundant hardware components (RAID, backup routers).
* **Load Balancing & Traffic Scrubbing:** Distributing incoming web traffic evenly across multiple servers and using edge protections to block or rate-limit suspicious traffic spikes.


* **Practical Scenarios:**
* *Availability Breached:* Critical IT network services disrupted or broken by a poorly tested software update; a company's e-commerce platform dropping completely offline during core operating business hours.
* *Availability Maintained:* Systems, servers, and applications staying fully accessible and functional for employees throughout their working hours.


<img width="1347" height="722" alt="image" src="https://github.com/user-attachments/assets/19d84944-8f7e-44bb-a080-d73993f82113" />


---

### 📌 Task 3: The Security Mindset & Evaluation Framework

Task 3 transitions the CIA Triad from rigid textbook definitions into an active, analytical framework—**the Security Mindset**.

* **What is the Security Mindset?** It is a proactive framework used by security professionals to analyze system flaws, investigate incidents, and build defenses. When an incident occurs, a practitioner does not panic; instead, they immediately classify the impact by asking three fundamental triage questions:
1. *Was sensitive data exposed to unauthorized individuals?* $\rightarrow$ (**Confidentiality Impact**)
2. *Was data modified or tampered with without permission?* $\rightarrow$ (**Integrity Impact**)
3. *Were systems or services rendered offline or unusable?* $\rightarrow$ (**Availability Impact**)


* **Practical Application:** Understanding this classification allows professionals to accurately calculate risk, measure business damage, and structure a defensive response plan.

---

### 📌 Task 4: Key Terminology & Final Takeaways

The final phase of this module establishes an exact, standardized vocabulary to carry forward into technical infrastructure engineering and deeper security labs.

> ### 🛑 Summary Cheat Sheet
> 
> 
> * **Confidentiality:** Focuses on **Secrecy**. Safeguards data against unauthorized *viewing* or leaks.
> * **Integrity:** Focuses on **Accuracy**. Safeguards data against unauthorized *modification* or tampering.
> * **Availability:** Focuses on **Access**. Safeguards systems against unauthorized *downtime* or disruptions.
> * **The Core Mindset:** It is specifically referred to as a **security mindset**.
> 
>
