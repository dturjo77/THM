এখানে তোমার প্রদান করা সবকটি টাস্ক (Task 2 থেকে Task 9) এর ওপর ভিত্তি করে একটি অত্যন্ত বিস্তারিত এবং গোছানো স্টাডি নোট তৈরি করে দেওয়া হলো। প্রতিটি টপিকের ভেতরের মেকানিজম এবং গুরুত্বপূর্ণ কমান্ডগুলো সুন্দরভাবে সাজানো হয়েছে:

---

# TryHackMe: Windows Security & Core Features Complete Notes

## Task 2: Windows Updates

Windows Update is a critical maintenance service provided by Microsoft to automatically deliver security patches, feature enhancements, bug fixes, and system definitions to the operating system.

* **Patch Tuesday:** Microsoft typically releases its scheduled security and system updates on the **2nd Tuesday of each month**.
* **Out-of-Band (OOB) Patches:** If a critical or zero-day vulnerability is actively exploited in the wild, Microsoft will bypass the Patch Tuesday schedule and push an emergency update immediately.
* **Enforced Reboots (Windows 10/Server):** Unlike older versions of Windows where updates could be permanently ignored, newer Windows ecosystems only allow users to temporarily postpone or schedule updates. Eventually, the system will force a restart to complete installation and protect device integrity.
* **Managed Environments:** In corporate environments, update settings are often "Managed by your organization." This means system administrators control patch deployment via tools like WSUS (Windows Server Update Services) or SCCM, rather than allowing individual machines to fetch updates directly from the internet.

> ### 🛠️ Quick Access Methods
> 
> 
> * **GUI Path:** `Settings` ➔ `Update & Security` ➔ `Windows Update`
> * **CLI / Run Command:** `control /name Microsoft.WindowsUpdate`
> 
> 

---

## ## Task 3: Windows Security Overview

Windows Security (formerly known as Windows Defender Security Center) serves as the unified dashboard to monitor and manage all built-in security features.

* **Centralized Security Hub:** It aggregates data from multiple defensive layers (Antivirus, Firewall, Device Security) into a single interface.
* **Status Indicators (Traffic Light System):**
* 🟢 **Green:** The system is fully protected; no immediate actions are required.
* 🟡 **Yellow:** A safety recommendation is available (e.g., a feature is turned off, or a non-critical scan is pending).
* 🔴 **Red:** A critical warning indicating the system is vulnerable and requires immediate attention (e.g., real-time protection is disabled or malware is detected).



---

## ## Task 4: Virus & Threat Protection

This is the core Antivirus component governed by Microsoft Defender Antivirus. It handles both reactive scanning and proactive threat mitigation.

### 1. Current Threats & Scan Options

* **Quick Scan:** Fast and efficient. It focuses exclusively on the operating system directories, memory, and registry locations where malware typically drops or hides.
* **Full Scan:** A deep inspection that checks every file, directory, and active process on the hard drive. This can take over an hour depending on the storage size.
* **Custom Scan:** Allows the user or administrator to target specific files, folders, or external drives for inspection.
* **Threat History Categorization:**
* **Last Scan:** Displays automated or manual historical data of the most recent check.
* **Quarantined Threats:** Malicious files that have been isolated into a secure vault to prevent execution. They are automatically deleted after a retention period.
* **Allowed Threats:** Items flagged as suspicious but overridden by the user to allow execution. *Warning: This should only be used for known false positives.*



### 2. Core Protection Settings

* **Real-Time Protection:** Continually runs in the background, intercepting file access and execution to stop malware instantly. *(Note: Often disabled in sandbox labs to save CPU performance).*
* **Cloud-Delivered Protection:** Connects to Microsoft's global cloud intelligence network to block brand-new, unseen threats instantly using heuristics and AI patterns.
* **Automatic Sample Submission:** Uploads suspicious files directly to Microsoft labs for deep behavioral analysis, improving global threat definitions.
* **Controlled Folder Access:** A robust anti-ransomware feature. It locks designated folders, allowing only authorized, whitelisted applications to modify files within them.
* **Exclusions:** A configuration list telling Defender to completely skip scanning certain files, extensions, or directories. This is useful for preventing performance lag on massive database directories or specialized development tools but presents a security risk if misused.

---

## ## Task 5: Firewall & Network Protection

A firewall acts as a digital barrier or "security guard" that controls the flow of inbound and outbound network traffic based on strict port and protocol rules.

### 1. The Three Firewall Profiles

Windows applies specific security postures depending on the type of network connection:

* **Domain Profile:** Automatically activates when the computer connects to a network where it can authenticate with a corporate Domain Controller (Active Directory environment).
* **Private Profile:** User-assigned for trusted networks, such as home or secure office Wi-Fi. It enables **Network Discovery**, allowing your machine to communicate with printers, file shares, and local media servers.
* **Public Profile:** The default profile applied to untrusted networks (airports, cafes, public spaces). It enforces maximum security by **disabling Network Discovery** and blocking unsolicited inbound connections to keep your computer invisible to hackers on the same network.

### 2. Management Options

* **Allow an app through firewall:** Quick settings to toggle whether an executable can communicate over Private or Public networks without building advanced rules manually.
* **Advanced Settings:** Opens the MMC snap-in for complex rule creation (Inbound/Outbound rules, Connection Security rules, monitoring blocks).

> ### 🛠️ Quick Access Methods
> 
> 
> * **CLI / Run Command:** `WF.msc` (Windows Firewall with Advanced Security)
> 
> 

---

## ## Task 6: App & Browser Control

This layer manages system-wide protection mechanisms against web-based exploits, malicious software drops, and phishing attempts.

* **Microsoft Defender SmartScreen:** A reputation-based security layer. It checks downloaded files and website URLs against a dynamic cloud database of reported phishing pages and known malware downloads. It can be configured to **Warn** users, **Block** actions completely, or be turned **Off**.
* **Exploit Protection:** Mitigation techniques built directly into the OS kernel to stop sophisticated memory exploitation attacks (e.g., preventing buffer overflows, memory injection, or return-oriented programming attacks).

---

## ## Task 7: Device Security

Device Security focuses on specialized isolation and hardware-level root-of-trust features that protect core operating system functions.

* **Core Isolation (Memory Integrity):** Uses virtualization-based security (VBS) to isolate core Windows kernel processes in a secure container. This prevents advanced malware from injecting malicious code into high-security Windows processes.
* **Trusted Platform Module (TPM):** A physical cryptographic chip embedded on modern motherboards. It safely generates, stores, and handles cryptographic keys. Because it works at the hardware layer, malware cannot tamper with its cryptographic functions.

---

## ## Task 8: BitLocker Drive Encryption

BitLocker is a full-disk encryption feature designed to protect data at rest on physical storage drives.

* **Data Theft Prevention:** If a laptop or hard drive is stolen, lost, or improperly disposed of, the data remains completely unreadable without the correct decryption key.
* **Hardware Synergy with TPM:** BitLocker achieves optimal security when paired with a TPM chip. The TPM checks the integrity of the boot files during startup; if it detects the system hardware or firmware has been tampered with offline, it refuses to release the encryption keys, locking down the drive.

---

## ## Task 9: Volume Shadow Copy Service (VSS)

The Volume Shadow Copy Service (VSS) is a critical Windows framework that coordinates blocks of data to capture consistent backup copies, known as **shadow copies** or **snapshots**, while the system is still actively running.

* **Storage Location:** Snapshots are written into a hidden, highly protected system directory located on each root drive path: `\System Volume Information\`.
* **Administrative Features:** When system protection is turned on, VSS facilitates:
* Creating System Restore Points.
* Performing rollback System Restores.
* Configuring custom storage limits for historical snapshots.


* **The Security / Ransomware Catch:** Modern malware and ransomware strains are engineered to explicitly seek out and destroy these backup files using administrative commands (such as running `vssadmin delete shadows`). By wiping out the shadow copies, attackers ensure victims cannot simply restore their systems to a pre-encrypted state, forcing them to rely on completely decoupled offline backups.
