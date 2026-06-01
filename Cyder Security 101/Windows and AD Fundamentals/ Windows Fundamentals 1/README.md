
***Title: Windows Fundamentals Room 1 Writeup***

**Overview:**
This room covers basic Windows operating system concepts useful for desktop users, administrators, and blue‑team/forensic work. Main topics covered are Windows editions, the desktop GUI, the NTFS file system, the System32 folder, user accounts & permissions, User Account Control (UAC), Settings vs. Control Panel, and Task Manager.

**Task 1 — Windows Editions:**
- Windows has dominated since 1985; major consumer releases include XP, Vista, 7, 8.x, 10, and 11. Server releases have separate lifecycles (e.g., Windows Server 2019).
- Support lifecycles significantly impact organizations’ upgrade planning; for example, Windows 10 support lasted until October 14, 2025 — forcing costly migrations and compatibility work.
- The lab VM uses Windows Server 2019 Standard, which helps illustrate enterprise/server contexts.

**Task 2 — The Desktop (GUI):**
- The Desktop is the graphical interface shown after login — containing shortcuts, wallpaper, and personalization/display settings.
- Start Menu: offers account actions, recently added apps, an alphabetical list of all installed apps, and pin‑able tiles for quick access.
- Taskbar: shows running and pinned apps with hover previews. The Notification Area (bottom right) holds system icons like clock, network, and volume, all customizable via taskbar settings.

**Task 3 — The File System:**
- Modern Windows uses NTFS — a journaling file system that addresses FAT limitations (supports files larger than 4 GB, per‑file/folder permissions, compression, and EFS encryption).
- NTFS permissions include Full Control, Modify, Read & Execute, List Folder Contents, Read, and Write. These are visible under Properties → Security.
- Alternate Data Streams (ADS) can hide data (often abused by malware) but are invisible in Explorer; they can be inspected with PowerShell or third‑party tools.

**Task 4 — The Windows\System32 Folders:**
- %windir% (usually C:\Windows) holds the OS files; it can reside on another drive but environment variables indicate its location.
- System32 contains critical system binaries — modifying or deleting files there can break Windows.
- Many built‑in administration and forensics utilities reside in System32; handle this folder with extreme caution.

**Task 5 — User Accounts, Profiles, and Permissions:**
- Local account types: Administrator (full system control) and Standard User (limited to their own files and settings).
- User profiles are created at first login and live under C:\Users\<username> with standard folders like Desktop and Documents.
- Local Users and Groups (lusrmgr.msc) lets admins manage users, groups, and group‑based permissions that determine access rights.

**Task 6 — User Account Control (UAC):**
- UAC (introduced in Vista) encourages running daily tasks without elevated privileges by prompting before granting admin rights, reducing malware risk.
- The built‑in local Administrator account does not use UAC prompts by default.
- Programs that require elevation show a shield icon; if the UAC prompt is not confirmed with admin credentials, the install or elevated action fails.

**Task 7 — Settings and the Control Panel:**
- Settings (introduced in Windows 8) is the modern, simpler, touch‑friendly interface; Control Panel is the legacy tool for some advanced options.
- Some Settings links redirect to Control Panel (for example, Network & Internet → Change adapter options).
- To view installed applications, use Control Panel → Programs and Features (shows name, publisher, and version).

**Task 8 — Task Manager:**
- Task Manager displays running applications/processes and system resource usage (CPU, RAM) under the Performance tab.
- Open it by right‑clicking the taskbar; it starts in Simple View — click “More details” to switch to the full view with extended information.
- Task Manager is useful for monitoring system performance and identifying core or suspicious Windows processes.

**Conclusion:**
This room provides a concise, practical introduction to Windows fundamentals: OS versions, the GUI, NTFS, critical folders, user management, UAC, settings, and Task Manager. From a blue‑team perspective, focus on NTFS permissions and ADS, System32 integrity verification, and monitoring UAC/events for suspicious activity.
