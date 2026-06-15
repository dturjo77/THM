
# Lab Room: Linux CLI Basics
### Date Completed: 15 June 2026
### Tools: tryhackme attack box

---

## Task 1: Introduction to the Command-Line Environment

### 1. The Command-Line Interface (CLI) vs. Graphical User Interface (GUI)

* **Definition:** The CLI (Terminal) is a text-based application interface used to pass instructions directly to the operating system kernel. The GUI relies on visual rendering engines to turn system parameters into clickable components.
* **Deep-Dive Analysis:**
* *Resource Efficiency:* GUIs consume substantial RAM and CPU cycles to render graphic frames. Headless Linux servers completely omit the GUI to preserve memory for services (like databases or web applications).
* *Automation and Scripting:* Commands executed in a CLI can be chained, piped, and embedded into shell scripts (`.sh` files). This allows a security analyst to automate thousands of manual operations into a single execution thread.
* *Remote Administration:* Managing systems over low-bandwidth channels (like satellite connections or highly secure remote environments) requires text-only access protocols like Secure Shell (SSH).



### 2. Anatomy of a Linux Terminal Prompt

A typical prompt strings together contextual indicators that state who you are, where you are, and your privilege level:


$$\text{\texttt{username@hostname:current\_directory\$}}$$

* **Username:** Represents the identity of the current shell process. This dictates what files you can read, write, or execute based on Linux Access Control Lists (ACLs).
* **Hostname:** The network identification name assigned to the machine. Essential when handling multiple SSH sessions to avoid running destructive commands on the wrong machine.
* **The Tilde ($\sim$):** This is a system shortcut variable representing the current user's **Home Directory** (absolute path: `/home/username`).
* **Privilege Indicators:**
* `$` (Dollar Sign): Indicates a standard, unprivileged user. Processes run under this restriction cannot touch global system configurations.
* `#` (Hash/Pound Sign): Indicates the **root** user account (Superuser). This account has absolute power over the operating system kernel and underlying storage file blocks.



---

## Task 2: Navigation & File Discovery Operations

### 3. Print Working Directory (`pwd`)

* **Command Structure:** `pwd`
* **Functional Mechanics:** Linux coordinates file tracking based on directories arranged in an inverted-tree structure originating at the root directory (`/`). `pwd` queries the kernel environment variables to resolve the absolute path from `/` down to your exact active node.
* **Cybersecurity Context:** Vital during incident response. When checking malware binaries, running a script with relative paths can fail or execute erroneously. Knowing your absolute directory path prevents execution errors.

### 4. Listing Directory Contents (`ls`)

* **Command Structure:** `ls [options] [directory]`
* **Detailed Parameter Analysis:**
* `ls`: Basic execution. Simply outputs visible filenames aligned horizontally or vertically depending on shell settings.
* `ls -l` (Long Listing Format): Renders detailed metadata tables including:
* *File Types and Permissions:* (e.g., `drwxr-xr-x` where `d` indicates a directory).
* *Hard Link Count:* Number of references pointing to the data inode.
* *Owner and Group:* Shows which user account and system group own the item.
* *File Size:* Standard display is rendered in raw byte count.
* *Modification Timestamp:* Last recorded time the file contents were altered.


* `ls -a` (All/Hidden Files): Reveals files beginning with a literal dot (`.`). Linux hides these files by convention to keep configurations clear from standard user views.
* `ls -al` or `ls -la`: Combines long listing with hidden file viewing.


* **Cybersecurity Context:** Threat actors frequently disguise web shells, malicious backdoors, or persistence scripts by placing a dot before the filename (e.g., `.hidden_backdoor.py`) inside public directories like `/tmp` or `/dev/shm`. Standard `ls` will completely skip these; running `ls -al` is a fundamental requirement during manual compromise triage.

### 5. Change Directory (`cd`)

* **Command Structure:** `cd [target_path]`
* **Functional Mechanics:** Updates the active working environment block of the current shell process to the specified destination path.
* **Directory Traversal Syntax:**
* `cd Documents/`: Moves forward into a relative child directory.
* `cd ..`: Relative shorthand to traverse one level upward to the parent folder.
* `cd ../..`: Traverses two levels upward simultaneously.
* `cd ~`: Instantly drops the user back into their absolute personal user folder (`/home/username`).



### 6. File Locating Utility (`find`)

* **Command Structure:** `find [starting_point] [expression] [argument]`
* **Deep-Dive Analysis:** The `find` utility directly searches the filesystem hierarchy on a storage level based on user-defined query filters.
* *Command breakdown:* `find ~ -name mission_brief.txt`
* `~`: Tells the engine to recursive-search downwards exclusively inside the Home directory.
* `-name`: Specifies a file-naming match expression.
* `mission_brief.txt`: The literal string case-sensitive target target.




* **Cybersecurity Context:** Security professionals rely on `find` during threat-hunting exercises to discover Indicators of Compromise (IoCs). For instance, finding all files modified within the last 24 hours (`find / -mtime -1`) or finding files with execution rules set for absolute access rights.

---

## Task 3: System Reconnaissance & Triage

### 7. Identity Verification (`whoami`)

* **Command Structure:** `whoami`
* **Functional Mechanics:** Queries the current effective User ID (UID) of the running process context and outputs the textual username associated with that specific identification number.
* **Cybersecurity Context:** Crucial when testing for **Privilege Escalation**. When exploiting an application vulnerability or running an exploit, you run `whoami` to verify if your shell identity changed from a low-level service profile (like `www-data`) to administrative dominance (`root`).

### 8. Kernel and System Identification (`uname`)

* **Command Structure:** `uname -a`
* **Detailed Output Parsing:**
* `Linux`: Identifies the system core engine foundation.
* `tryhackme`: Node hostname identifier.
* `#17-Ubuntu SMP Mon Sep 2 13:48:07 UTC 2024`: Detailed kernel build compilation sequence, patch iteration tracking, and release architecture timeframes.
* `x86_64`: Confirms the hardware platform architecture is 64-bit.


* **Cybersecurity Context:** This represents initial system **fingerprinting**. Penetration testers use the exact kernel release signature found via `uname -a` to cross-reference public exploit engines (like ExploitDB) for known, unpatched Local Privilege Escalation (LPE) vulnerabilities (e.g., Dirty COW, CVE-2021-3156).

### 9. Disk Space Allocation (`df`)

* **Command Structure:** `df -h`
* **Detailed Parameter Analysis:**
* `df`: Reports filesystem disk space usage metrics.
* `-h` (Human-Readable): Instructs the system to automatically compute raw byte values into megabytes (M) or gigabytes (G) to ensure rapid human consumption of the data.


* **Subsystem Definitions:**
* `/dev/root` or `/dev/sda1`: The hardware/virtual partition backing the principal root filesystem structure (`/`).
* `tmpfs`: Volatile, dynamic Virtual memory partitions mapped directly into RAM. Data residing inside a `tmpfs` structure is completely wiped out upon system power depletion or hard reboot.


* **Cybersecurity Context:** Security teams analyze disk allocations to check for Denial of Service (DoS) conditions caused by explosive logging or malicious data collection operations. Additionally, malware staging activities often write large compressed zip archives inside `/dev/shm` (shared memory space mapped via `tmpfs`) because it leaves no trace on the non-volatile magnetic/solid-state physical drives.

### 10. The Filesystem Hierarchy and `/etc` Configuration Vault

* **Concept:** In Linux, absolute system global parameters are cleanly isolated from normal software packages. The `/etc` directory functions as the central management library containing static text records configuration mappings, security policies, and application profiles.
* **Operational Execution:** Using `cd /etc` allows a technician to view configuration properties globally governing the active workstation engine.

### 11. Reading File Streams (`cat`)

* **Command Structure:** `cat [file_path]`
* **Functional Mechanics:** Short for "Concatenate." This command opens a target file stream, extracts the underlying ASCII/UTF-8 character codes, and dumps the raw text directly into the standard output stream (`stdout`) of the active console session.
* **File Analysis Context (`/etc/os-release`):** Reading this specific file returns critical operational distributions details (`PRETTY_NAME="Ubuntu 24.04.1 LTS"`).
* **Cybersecurity Context:** `cat` allows safe, static evaluation of raw data files without running an interactive text editing platform like `nano` or `vi`. This prevents accidental metadata modification or unintended saving actions while reviewing configuration properties or small, non-binary log files.

---

## Task 4: Practical Synthesis & Workflow Integration

The final component demonstrated how standalone linear commands integrate into a cohesive operational workflow to extract threat intelligence or report metrics.

### The Attack/Analysis Chain Applied:

1. **Reconnaissance (File Tracking):** Initial deployment of `find ~ -name day1_report.txt` map discovery coordinates across storage sectors.
2. **Navigation (Maneuvering):** Utilization of `cd` to bypass directory obstacles and directly reposition process execution context into the folder sector.
3. **Validation (Target Verification):** Executing `ls` to cross-examine local storage arrays to confirm target physical accuracy.
4. **Exfiltration (Data Reading):** Implementing `cat` to pipeline stored file properties into readable information blocks.

---

### Command Cheat Sheet Summary

| Command | Primary Flag / Parameter | Technical Action Summary | Cyber Security Application |
| --- | --- | --- | --- |
| **`pwd`** | *None* | Prints absolute path from system root `/`. | Assures location accuracy during script execution. |
| **`ls`** | `-al` | Renders extended attributes table including hidden files. | Exposes hidden rootkits, malicious payloads, and backdoors. |
| **`cd`** | `..` / `~` | Transitions shell execution focus path node. | Essential for manual lateral movement within directory filesystems. |
| **`find`** | `~ -name [file]` | Traverses filesystem tree looking for pattern match strings. | Searches for Indicators of Compromise (IoCs) and script files. |
| **`whoami`** | *None* | Returns active operating user account reference. | Confirms successful horizontal or vertical privilege escalation. |
| **`uname`** | `-a` | Pulls core system kernel and hardware architecture data. | Fingerprints OS platform to look up matched known vulnerability exploits. |
| **`df`** | `-h` | Prints available structural memory thresholds cleanly. | Evaluates log saturation attacks or in-memory dynamic storage usage. |
| **`cat`** | `[file]` | Dumps raw file data payload contents to terminal. | Safely previews configuration records and diagnostic indicators. |
