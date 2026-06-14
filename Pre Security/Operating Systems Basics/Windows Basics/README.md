Operating Systems Basics
Windows Basics


---

# 🖥️ Task 1: Introduction to the Workstation & Core Objectives

Task 1 established your role as an Information Technology professional onboarding at TryHatMe. The primary objective was to frame the foundational skills required to interact with a Windows operating system environment safely and efficiently.

### 🌐 The Operating System (OS) as a Resource Manager

An operating system serves as the intermediary software layer between the computer hardware (CPU, RAM, Storage) and the user application layer. It handles process scheduling, memory allocation, file systems, and hardware input/output (I/O).

### 📋 The IT Onboarding Blueprint

To manage a corporate workstation professionally, a system administrator must master four operational dimensions:

1. **Interface Fluency:** The ability to move through a GUI without relying on trial-and-error.
2. **Directory Architecture:** Understanding structural paths to manage data access cleanly.
3. **System Diagnostics:** Knowing how to audit active resource pools (CPU/RAM) when a system is under load.
4. **Endpoint Security:** Verifying baseline defenses before any network or software operations are executed.

---

# 🚀 Task 2: Deep-Dive Into the Windows Workspace

Task 2 focused on the evolutionary context of Windows, authentication boundaries, interface navigation, and the hierarchical file architecture.

### 🕰️ Chronological Evolution: CLI to GUI

* **MS-DOS (Microsoft Disk Operating System):** A non-graphical, Command Line Interface (CLI). Users interacted directly with the system shell using explicit text strings. It was single-tasking and required a deep technical knowledge of system flags.
* **Windows 1.0 (1985):** Not a standalone OS, but a graphical expansion pack running on top of MS-DOS. It introduced the concept of a **Graphical User Interface (GUI)**, leveraging visual icons, drop-down menus, and pointer devices (mice) to abstract command-line complexities.
* **Modern Windows OS Architecture:** The modern system has evolved into a fully independent, multitasking operating system running on the robust **Windows NT kernel**, capable of handling advanced enterprise networking and security architectures.

### 🔐 Authentication Boundaries & AAA (Authentication, Authorization, Accounting)

Authentication is the strict technical process of verifying an identity (via a password, PIN, or biometric token) before granting access to the system. Once authenticated, Windows applies **Authorization** principles based on account groups:

```
[Guest Account]       --> Lowest privilege; temporary volatile access; no persistent modifications.
[Standard Account]    --> Mediated privilege; isolated workspace; cannot modify system-wide states.
[Admin Account]       --> Unrestricted kernel-level access; full privilege escalation capabilities.

```

* **Administrative Privilege Mode:** On your TryHatMe workstation, you are assigned an **Administrator** account. This bypasses typical User Account Control (UAC) prompts, giving you the direct clearance required to execute installations, modify network profiles, and examine root system paths.

### 🏢 The UI Component Blueprint

Using the **Airport Terminal** analogy, the desktop layout organizes your interface workspace:

* **The Desktop:** The persistent top-level folder workspace serving as the anchor layout for active application instances and shortcuts.
* **The Taskbar:** The core system strip housing running processes, pinned applications, the notification tray, and hardware status toggles (Network/Audio).
* **Task View:** A local window manager tool that displays an expanded layout of all currently active threads running in the user session, facilitating rapid switching between concurrent workflows.

### 🔍 System Introspection (About Your PC)

Accessing this menu polls the local system management tools to retrieve live hardware and software configuration strings. It gives an administrator instant clarity on:

* **Device Specifications:** Processor architecture (CPU clock rates) and Installed Physical Memory (RAM), defining the processing power available for workloads.
* **Windows Specifications:** The specific edition (e.g., Windows Server 2019) and OS build numbers, which dictate which enterprise features and patch levels are active.

### 📂 Hierarchical Directory Architecture

Windows manages data using a **Tree Structure Layout**. The root of this logical tree is a drive letter (typically `C:\`).

```
C:\ (System Root)
└── Users
    └── Administrator
        └── Desktop
            └── TryHatMe Onboarding (Target Folder)

```

* **File Paths vs. Display Names:** While File Explorer represents directories visually as folders, the OS identifies assets through literal, absolute string addresses called **File Paths** (e.g., `C:\Users\Administrator\Desktop\TryHatMe Onboarding`).
* **Address Bar Resolution:** Clicking into the address bar drops the graphical interface and reveals the absolute structural path, which is crucial for configuring scripts, terminal automation, or network maps.

---

# ⚙️ Task 3: Configuring, Auditing, and Securing Windows

Task 3 provided hands-on experience with package installation, interface differences, process monitoring, and endpoint threat defense.

### 📦 Software Lifecycle Management

* **Operating System Updates (Windows Update):** The mechanism that polls Microsoft servers to fetch code patches, vulnerability remediations, and hardware driver revisions to protect against active security exploits.
* **Installer Configurations (`.exe` vs. `.msi`):**
* `.exe` (Executable File): A standalone application binary that runs custom setup scripts or the software itself.
* `.msi` (Microsoft Installer): A standardized database format optimized for Windows Installer services, allowing silent installations, easy automated rollbacks, and corporate network deployments.


* **The Application Deployed:** Executing the **`TryHatMeWelcome`** application illustrated the user-guided initialization of software assets within an isolated virtual playground environment.

### 🎛️ System Configuration: Dual Management Interfaces

Windows balances modernized usability with legacy enterprise requirements across two key tools:

* **Windows Settings:** A clean, centralized UWP (Universal Windows Platform) layout designed for user-facing preferences, direct personalization, account profiles, and universal system updates.
* **Control Panel:** The deep legacy administration core. It retains advanced configurations, applet access, and network routing profiles that enterprise IT teams still require for precision adjustments.

### 📊 System Diagnostics via Task Manager

The Task Manager (`Ctrl + Shift + Esc`) acts as an immediate structural window into local system resource pools:

```
+----------------------------------------------------------------------------+
| TASK MANAGER                                                               |
+-------------+-----------------+-----------+-------------+------------------+
|  Processes  |   Performance   |   Users   |   Details   |     Services     |
+-------------+-----------------+-----------+-------------+------------------+
| Active apps | Real-time graphs| Connected | Raw list of | Background tasks |
| & resource  | for CPU, RAM,   | sessions  | system PIDs | without a visual |
| consumption | and network I/O | & usage   | & states    | user interface   |
+-------------+-----------------+-----------+-------------+------------------+

```

* **Process Isolation and PIDs:** Every application or background task running on Windows is assigned a unique tracking decimal number known as a **PID (Process ID)**. This abstraction ensures the operating system can isolate and manage hardware memory spaces without conflict.

### 🛡️ End-to-End Endpoint Defense

* **Windows Security Architecture:** A built-in security suite working as an endpoint detection and response layer, broken down into specialized categories:
* *Virus & Threat Protection:* The signature and heuristic antimalware engine.
* *App & Browser Control:* SmartScreen reputational filtering preventing the execution of unsigned or suspicious binaries.


* **The Custom Directory Scan Engine:** Running a custom scan targeted at the `TryHatMe Onboarding` folder demonstrates how the scanning utility checks files against known hashes and suspicious behavior models. Isolating the built-in, completely benign security test file verified that the monitoring system was fully alert and operating efficiently.

### 🧱 Network Traffic Regulation: Windows Defender Firewall

A software-based firewall operating at the network layer to inspect incoming and outgoing packets. It references distinct operational rule sets depending on network categorization profiles:

* **Domain Profile:** Automatically assigned when the computer is joined to an enterprise Active Directory domain controller.
* **Private Profile:** Designed for trusted local area networks (LANs) like an internal lab or home office network, allowing relaxed local discovery.
* **Public Profile:** High-restriction mode. Disables network discovery and locks down ports to isolate your machine when connecting to untrusted networks like public Wi-Fi access points.

---

# 🏁 Task 4: Master Definitions and Key Terminology

Task 4 synthesized all practical training into a structured dictionary of core enterprise components. This list acts as your vocabulary baseline for future engineering modules:

* **Desktop:** The foundational graphical workspace containing folders, shortcuts, and active program canvases.
* **Taskbar:** The primary system strip housing active program windows, settings trays, and pinned apps.
* **Start Menu:** The unified application directory and power option terminal.
* **Search Bar:** A rapid indexing search utility to instantly pull files, applications, or settings via text strings.
* **File Explorer:** The visual file manager mapping the internal hierarchical tree structure.
* **Windows Update:** The automated patch management module ensuring code-level integrity against active security threats.
* **Microsoft Store:** A sandboxed application market offering vetted, trusted platform apps.
* **Windows Settings:** The modern system applet for device configurations and personalization.
* **Control Panel:** The deep legacy administration core housing advanced system components.
* **Task Manager:** The real-time triage hub used to view PIDs, monitor hardware utilization, and terminate frozen operations.
* **Windows Security:** The centralized engine controlling antimalware sweeps, app protections, and system health baselines.
* **Windows Defender Firewall:** The stateful network packet filter regulating inbound and outbound connections based on environment trust levels.

  
