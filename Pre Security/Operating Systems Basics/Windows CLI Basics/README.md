Pre Security
Operating Systems Basics
Windows CLI Basics


---

## 📌 Task 1: Introduction to the Windows CLI (CMD)

This task established the fundamental shift from a Graphical User Interface (GUI) to a Command-Line Interface (CLI) within a Windows environment.

### 1. The Core Purpose of CMD in Cybersecurity

* **Definition:** The Command Prompt (CMD) is a text-based interpreter for the Windows operating system. It allows direct interaction with the OS kernel by executing precise text commands instead of clicking icons.
* **Why Security Professionals Use It:**
* **Speed & Efficiency:** Executing a text command is significantly faster than navigating multiple sub-menus in a GUI.
* **Automation:** Commands can be chained together or written into batch scripts (`.bat`) to automate repetitive investigative tasks.
* **Tool Compatibility:** Many advanced security tools, custom exploit scripts, and digital forensics utilities do not feature a GUI and can *only* be executed via the terminal.
* **Remote Administration:** During an incident response scenario, an analyst often connects to a compromised machine via a headless remote shell (like SSH or WinRM) where no GUI exists.



---

## 📌 Task 2: Navigating Files and Finding Files

This task focused on exploring the Windows filesystem hierarchy, exposing hidden data, searching subdirectories, and reading files.

### 1. Checking the Current Working Directory (`cd`)

* **The Command:** `cd` (without any arguments)
* **Explanation:** In Windows CMD, typing `cd` on its own prints the absolute path of the directory you are currently operating in.
* **Security Context:** Knowing your precise location is critical before running scripts or downloading files so you do not accidentally overwrite system files or lose track of where your tools are dropped.
* **Linux Comparison:** Equivalent to `pwd` (Print Working Directory).

### 2. Listing Directory Contents (`dir`)

* **The Command:** `dir`
* **Explanation:** Displays a list of files and subdirectories contained within the current directory, along with metadata such as file sizes and the date/time of the last modification.
* **Security Context:** Analysts use this to audit what files are present in high-risk directories (like `C:\Users\<User>\Downloads` or `C:\Windows\Temp`) to check for suspicious or unrecognized files.
* **Linux Comparison:** Equivalent to `ls`.

### 3. Exposing Hidden Files and Folders (`dir /a`)

* **The Command:** `dir /a` (The `/a` switch stands for **Attributes**)
* **Explanation:** Windows allows files and directories to be marked with a "Hidden" attribute, masking them from standard `dir` listings and the File Explorer. Adding the `/a` switch forces CMD to display *all* files regardless of their structural attributes.
* **Security Context:** Attackers frequently hide their malicious payloads, staging tools, or keyloggers by modifying their file attributes to "hidden". Running `dir /a` is a fundamental step to uncovering hidden malware configurations or persistent backdoor directories.
* **Linux Comparison:** Equivalent to `ls -a`.

### 4. Navigating the Filesystem (`cd <directory>`)

* **The Command:** `cd <Folder_Name>` or `cd ..`
* **Explanation:** Changes the current working directory. Specifying a folder name moves you forward into that folder. Specifying two periods (`cd ..`) moves you backward exactly one level up the directory tree.
* **Security Context:** Essential for manually traversing the system to inspect sensitive directories, such as user application data paths or system configuration folders.

### 5. Searching Subdirectories Globally (`dir /s`)

* **The Command:** `dir /s <filename>` (The `/s` switch stands for **Subdirectories**)
* **Explanation:** Recursively searches the current directory and *all* of its deeper subdirectories for a specific file name, returning the exact path where it is located.
* **Security Context:** If an incident response alert flags a malicious file name (e.g., `mimikatz.exe` or a specific script), an analyst can use `dir /s` from the root drive (`C:\`) to pinpoint every single location where that file resides on the system.
* **Linux Comparison:** Similar to the `find . -name <filename>` command.

### 6. Reading File Contents (`type`)

* **The Command:** `type <filename>`
* **Explanation:** Reads the raw data of a text-based file and outputs the content directly onto the terminal screen.
* **Security Context:** Allows security professionals to view the contents of configuration files, configuration scripts, or plain-text logs instantly without opening an external text editor (like Notepad), minimizing the forensic footprint left on the system.
* **Linux Comparison:** Equivalent to `cat`.

---

## 📌 Task 3: Gathering System Information on Windows

This task highlighted **System Enumeration**, which involves collecting essential telemetry data regarding the local machine, operating system structure, and live network parameters.

### 1. Identifying Current User Context (`whoami`)

* **The Command:** `whoami`
* **Explanation:** Outputs the domain or computer name followed by the exact username of the account executing the command prompt (Format: `HOSTNAME\Username`).
* **Security Context:** Permissions in Windows dictate what a user can see or modify. If an analyst runs `whoami` and sees a standard user account, they know they must elevate privileges to perform deep forensic tasks. If an attacker runs it and sees `nt authority\system`, they know they have achieved complete administrative control over the machine.

### 2. Identifying Computer Identity (`hostname`)

* **The Command:** `hostname`
* **Explanation:** Displays the specific network name assigned to the local Windows machine.
* **Security Context:** In corporate networks, hostnames follow strict naming conventions (e.g., `HR-LAPTOP-04` or `FIN-SERVER-01`). Identifying the hostname helps an incident responder quickly determine the physical or structural location of an infected machine within the company grid.

### 3. Detailed Operating System Assessment (`systeminfo`)

* **The Command:** `systeminfo`
* **Explanation:** Queries the system configuration and outputs a comprehensive profile of the OS. Key fields to evaluate include:
* **OS Name & Version:** Identifies the precise Windows build (e.g., Windows 10 Pro vs Windows Server 2022).
* **System Type:** Confirms hardware architecture ($x86$ 32-bit vs $x64$ 64-bit), which dictates what type of software or malware can execute on it.
* **Hotfix(es):** Lists all security patches applied to the OS.


* **Security Context:**
* **For Defenders:** Helps verify if the system is fully updated or missing critical patches.
* **For Attackers:** Looking at the missing Hotfixes allows an attacker to identify known unpatched vulnerabilities to escalate their privileges.


* **Linux Comparison:** Similar to running `uname -a` or viewing `/etc/os-release`.

### 4. Network Architecture Inspection (`ipconfig`)

* **The Command:** `ipconfig`
* **Explanation:** Displays all current network adapter configurations. Critical fields include:
* **IPv4 Address:** The local network identity of the machine.
* **Default Gateway:** The IP address of the router or device that connects this machine to external networks/the Internet.


* **Security Context:** Network mapping is key during an investigation. The IPv4 address helps map the machine to network traffic logs, while a missing Default Gateway indicates that the machine is isolated on a local segment without Internet egress.
* **Linux Comparison:** Equivalent to `ifconfig` or `ip a`.

---

## 📌 Task 4: Conclusion & Key Takeaways

This section summarized the core philosophy of transitioning to command-line administration.

* **Visibility Beyond the GUI:** The Windows GUI actively hides operational realities (hidden directories, specific system patches, raw network metrics) to keep things simple for everyday users. Security professionals cannot afford simplicity; they require the absolute visibility that the CLI provides.
* **Footprint Reduction:** Relying on the command line avoids launching heavy graphical applications, keeping system modifications to a minimum during active forensic analysis.
* **Automation Readiness:** Every command mastered in this room serves as a structural block for building automated triage scripts, letting you check hundreds of endpoints simultaneously in real-world security operations.
