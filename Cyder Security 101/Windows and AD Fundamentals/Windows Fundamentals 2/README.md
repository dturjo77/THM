---
###Lab: Windows Fundamentals 2

##Date Completed: 2 June 2026
##Tools: tryhackme attack box, Windows 10 

---

### Task 1 & 2: Environment Variables & System Identification

Through the verification of System Information and internal variables, we analyzed how Windows maps its infrastructure identities and key directory locations.

#### 1. Environment Variables

Environment variables are dynamic, system-wide values stored by the operating system to keep track of critical paths, system configurations, and operational data used by multiple applications.

* **`ComSpec` (Command Specification):** A vital system variable that explicitly defines the exact location and file name of the default command-line interpreter. In standard Windows environments, its value points directly to `%SystemRoot%\system32\cmd.exe`. Applications rely on this variable to spawn command-line operations dynamically.
* **`%SystemRoot%` vs. Explicit Paths:** Operating systems use variable paths like `%SystemRoot%` to maintain flexibility. Whether the Windows directory is installed on the `C:` drive or an alternate volume, the variable abstracts the specific drive letter to prevent hardcoded directory link errors.

#### 2. System Name Context

Every workstation on a network requires a unique identity to communicate.

* **Workstation/System Name:** In this laboratory infrastructure, the System Name was identified as `THM-WINFUND2`.
* **User/Machine Mapping:** When accessing resources or parsing administrative metrics, the naming syntax often follows the `ComputerName\UserName` hierarchy (e.g., `THM-WINFUND2\Administrator`). This explicitly differentiates local administrative privileges from network domain directory structures.

---

### Task 3 & 4: System Performance Diagnostics (Resource Monitor)

Resource Monitor (`resmon`) is an advanced diagnostics console utilized by system administrators and analysts to monitor system resources and perform deep-dive troubleshooting.

#### 1. Core Structural Metrics

Resource Monitor breaks operating performance into four essential pillars:

* **CPU (Processor):** Displays per-process metrics, thread allocations, and CPU utilization percentages. A key enterprise feature here is the **Analyze Wait Chain** capability. If an application hangs, this function maps out the hidden dependencies, identifying exactly which background process or thread is causing a deadlock, allowing administrators to target and terminate the root issue rather than crashing the primary application.
* **Memory (RAM):** Offers a comprehensive, color-coded breakdown of physical RAM allocation, organizing memory into categories such as *Hardware Reserved*, *In Use*, *Modified*, *Standby*, and *Free*.
* **Disk (Storage):** Tracks active disk I/O operations, disk queue lengths, and individual transfer rates in real time. It monitors **File Handles**, showing exactly which file a process is reading from or writing to, making it easy to identify programs that lock specific files and trigger the *"File in use"* warning.
* **Network (Connections):** Monitors active processes sending data across local or external interfaces. It exposes live bandwidth throughput (Kbps) per process and reveals **Network Connections** detailing active Remote IP Addresses and ports. This section functions as an essential, quick tool for tracking unauthorized background connections or potential data exfiltration vectors.

#### 2. Advanced GUI Utilities

* **Granular Filtering:** Selecting a checkbox next to a specific process (e.g., `chrome.exe`) isolates data across all other tabs, dynamically filtering out unrelated noise to display only that application's resource impact.
* **Service Integration:** It enables users to directly control systemic services (Start, Stop, Pause, Resume) inside the active diagnostics window.

---

### Task 5 & 6: Command Line Interface (Command Prompt)

Before the introduction of Graphical User Interfaces (GUIs), command-line interpreters were the exclusive method for interacting with an operating system. Today, they remain a high-efficiency environment for automation, scripting, and system interrogation.

#### 1. Identity & Network Commands

* **`hostname`:** Queries the kernel configuration to return the distinct local computer network name.
* **`whoami`:** Returns the exact domain or system security group name concatenated with the active logged-in username.
* **`ipconfig`:** Interrogates network adapters to display essential network settings like IP Address, Subnet Mask, and Default Gateway.
* **`ipconfig /all`:** An expanded flag that provides a deeper analysis of the network stacks, revealing hardware MAC addresses, DHCP lease timelines, and local DNS server configurations.



#### 2. Network Statistics & System Resources

* **`netstat`:** Displays protocol statistics and current TCP/IP network connections. Using switches like `-a` or `-b` allows administrators to map out open ports and identify the exact executable binaries managing active external network sessions.
* **`net` (Network Management Suite):** An administrative suite used to manage network resources and user environments. It utilizes distinct sub-commands:
* `net user`: Audits, configures, or creates user accounts on the machine.
* `net localgroup`: Lists or modifies access control groups, such as adding a user to the local Administrators group.



#### 3. Command Syntax & Assistance Flags

* **Universal Help Switch (`/?`):** Appending this switch to a standard command queries its built-in manual, listing proper syntax structures, arguments, and parameter modifiers.
* **`net help` Context:** The `net` utility handles syntax documentation differently; rather than utilizing the standard `/?` flag, it requires entering `net help [sub-command]` (e.g., `net help user`) to retrieve instructions.
* **`cls` (Clear Screen):** Flushes the command-line buffer to clear visual clutter without interrupting running shell processes.

---

### Task 7: MSConfig Integration (System Configuration Tools)

The System Configuration tool (`msconfig`) functions as a launchpad for complex administrative tools embedded within the operating system directory.

#### 1. Executable Command Line Formats

When launching default networking tools from the System Configuration console, Windows maps the execution by invoking the command line processor along with explicit target targets.

* **Internet Protocol Configuration Mapping:** To spin up a persistent display for network analysis, the console uses the following format:

$$\text{C:\Windows\System32\cmd.exe /k System32\ipconfig.exe}$$


* **Understanding Command Arguments (`/k`):** The `/k` switch instructs `cmd.exe` to execute the following command (in this case, `ipconfig.exe`) and **keep** the command prompt window open afterward. Without this argument, the shell would execute the utility and close immediately, preventing users from reviewing the network output data.

---

### Task 8: Windows Registry Structure (Registry Editor)

The Windows Registry is a centralized, hierarchical database that stores configuration settings and options for the operating system, installed applications, user profiles, and attached hardware components.

#### 1. Architectural Hives

The database is structured around five primary root keys, or "hives":

* **`HKEY_CLASSES_ROOT` (HKCR):** Manages file associations, Object Linking and Embedding (OLE) configurations, and COM class identifiers, ensuring that clicking a specific extension launches its correct software application.
* **`HKEY_CURRENT_USER` (HKCU):** Houses configuration states mapped directly to the user profile currently logged into the local session, including desktop layouts, personal mapped network drives, and application preferences.
* **`HKEY_LOCAL_MACHINE` (HKLM):** Holds the core configuration settings for the local physical machine, including hardware initialization data, boot configurations, and system-wide security settings. This data remains constant regardless of which user account is active.
* **`HKEY_USERS` (HKU):** Contains configuration profiles for all user accounts actively configured on the local system, serving as the master template from which `HKCU` mirrors its active parameters.
* **`HKEY_CURRENT_CONFIG` (HKCC):** Tracks volatile runtime information regarding the hardware profile established during the system boot sequence.

#### 2. Access Tools & Administrative Hazards

* **`regedt32.exe` vs. `regedit`:** Modern Windows systems merge these execution strings into the same central interface. Historically, `regedt32.exe` was utilized across advanced Windows NT environments for managing complex 32-bit registry parameters.
* **System Integrity Risks:** Direct registry modifications carry significant risk. Inserting invalid string values, disrupting binary keys, or deleting entries within `HKLM` can cause critical system errors, corrupt application behaviors, or completely break the operating system's boot capabilities.

---

### Task 9: Architectural Conclusion

Mastering these administrative utilities expands your system troubleshooting options:

* While `msconfig` provides an accessible launchpad for these diagnostics, running binaries like `resmon` or `regedit.exe` directly via the **Run box (Win + R)** or command interpreter increases speed and efficiency.
* These tools provide a foundation for managing system state configurations, auditing network connections, identifying bottlenecks, and maintaining system integrity across enterprise-level Windows deployments.

---


<img width="1166" height="278" alt="image" src="https://github.com/user-attachments/assets/bc9136eb-ee47-4291-bbd6-b8ba6e174d43" />
