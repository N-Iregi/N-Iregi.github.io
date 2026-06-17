---
title: "TryHackMe: Windows Fundamentals 2"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

Part 2 of the Windows Fundamentals module on TryHackMe. Covers the internals of Windows administration — system configuration, UAC, computer management, system information, resource monitoring, command-line tools, and the Windows Registry. The goal was to move beyond everyday Windows usage into administrative-level understanding relevant to system administration, troubleshooting, and Windows-focused security work.

# Tools & Environment

- TryHackMe browser-based Windows lab environment
- Built-in Windows utilities: MSConfig, compmgmt, msinfo32, resmon, regedit

# Task 1: System Configuration (MSConfig)

MSConfig is an advanced troubleshooting utility, primarily used to diagnose startup issues. It is **not** a startup management program.

## Tabs

| Tab | Purpose |
|-----|---------|
| General | Select boot mode: Normal, Diagnostic, or Selective |
| Boot | Define OS boot options |
| Services | View all configured services (running or stopped) |
| Startup | Redirects to Task Manager for startup item management |
| Tools | Shortcuts to system utilities with launch commands |

**Accessing user startup items on Windows Server:**
`Win + R` → `shell:startup` — displays all programs configured to run at next user login as shortcuts or executables.

## Advanced System Settings — Page File & Crash Dumps

The Performance → Advanced tab shows the page file configuration:
- Drive where page file is stored
- Initial and maximum size (MB)
- Whether Windows manages size automatically

**Crash dump types** (configured under Startup and Recovery):

| Type | Description |
|------|-------------|
| Automatic memory dump | Windows selects dump size automatically |
| Kernel memory dump | Records only kernel memory at time of crash |
| Small memory dump (256 KB) | Minimal info — fastest to write |
| Complete memory dump | Full physical memory — largest file |
| None | No dump written |

Crash dumps are the primary artefact for post-mortem analysis of Blue Screen of Death (BSOD) events.

## Questions

| Question | Answer |
|----------|--------|
| Service with Sysinternals as manufacturer | `PsShutdown` |
| Windows license registered to | `Windows User` |
| Command for Windows Troubleshooting | `C:\Windows\System32\control.exe /name Microsoft.Troubleshooting` |
| Command to open Control Panel | `control.exe` |

# Task 2: User Account Control (UAC)

UAC runs apps and tasks in the security context of a non-administrator account by default, requiring explicit elevation for privileged operations. Functionally similar to Linux's `sudo`.

## UAC Levels

| Level | Behaviour |
|-------|-----------|
| Always notify | Notifies on all changes (apps and user); Secure Desktop dims |
| Notify for apps (default) | Notifies only when apps make changes; user changes pass silently |
| Notify without dimming | Same as above but desktop does not dim |
| Never notify | All notifications disabled — maximum attack surface |

**Security note:** The "Never notify" setting disables a key defence against
privilege escalation — malware can silently request elevated privileges without triggering any user prompt.

## Questions

| Question | Answer |
|----------|--------|
| Command to open UAC Settings | `UserAccountControlSettings.exe` |

# Task 3: Computer Management (compmgmt)

Three primary sections:

## System Tools

**Task Scheduler** — creates and manages automated tasks triggered by time, login, logoff, or system events. Attackers commonly abuse scheduled tasks for persistence.

**Event Viewer** — audit trail of system events, used for incident investigation and troubleshooting. Five event types:

| Type | Description |
|------|-------------|
| Information | Successful operation |
| Warning | Non-critical issue that may cause future problems |
| Error | Significant problem — functionality loss |
| Critical | Severe failure requiring immediate attention |
| Audit Success / Failure | Security auditing events |

Standard logs under Windows Logs: Application, Security, Setup, System, Forwarded Events.

**Shared Folders** — lists all shares accessible to network users.

**Performance Monitor (perfmon)** — real-time and historical performance data across CPU, memory, disk, and network.

## Storage

**Disk Management** — create, extend, shrink, and assign drive letters to partitions without third-party tools.

## Services and Applications

**WMI (Windows Management Instrumentation)** — infrastructure for management data and automation on Windows. Allows scripting languages (VBScript, PowerShell) to manage local and remote systems.

**Security note:** WMI is heavily abused for lateral movement and persistence in post-exploitation scenarios — legitimate use cases and attacker tradecraft overlap significantly here.

## Questions

| Question | Answer |
|----------|--------|
| When is npcapwatchdog scheduled to run? | At system startup |

# Task 4: System Information (msinfo32)

Divided into three sections:

- **Hardware Resources** — IRQs, DMA, I/O, memory addresses
- **Components** — display, input, network, storage hardware
- **Software Environment** — drivers, running tasks, startup programs, environment variables

## Environment Variables

Environment variables store OS configuration data referenced by the system and applications during operation.

| Variable | Example Value |
|----------|---------------|
| `WINDIR` | `C:\Windows` |
| `ComSpec` | `%SystemRoot%\system32\cmd.exe` |
| `TEMP` | `C:\Users\<user>\AppData\Local\Temp` |

## Questions

| Question | Answer |
|----------|--------|
| Command to open System Information | `msinfo32.exe` |
| System Name | `THM-WINFUN2` |
| Value of ComSpec | `%SystemRoot%\system32\cmd.exe` |

# Task 5: Resource Monitor (resmon)

Advanced real-time monitoring tool showing per-process and aggregate usage across four tabs:

| Tab | Data Shown |
|-----|------------|
| CPU | Per-process CPU usage, services, associated handles/modules |
| Memory | Physical memory usage, working sets, page faults |
| Disk | Per-process disk I/O, active reads/writes |
| Network | Per-process network activity, TCP connections, listening ports |

**Security relevance:** resmon can identify unexpected outbound connections, unusual process network activity, and file handles held open by suspicious processes — useful for basic triage during incident investigation.

# Task 6: Command Prompt Tools

| Command | Output |
|---------|--------|
| `hostname` | Computer name |
| `whoami` | Current logged-in user |
| `ipconfig` | Network interface addresses |
| `ipconfig /all` | Detailed network config (MAC, DHCP, DNS) |
| `netstat` | Active TCP/IP connections and listening ports |
| `net` | Network resource management (users, shares, sessions) |
| `<command> /?` | Help manual for any command |

## Questions

| Question | Answer |
|----------|--------|
| Full command for IP Configuration in MSConfig | `C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe` |
| Show detailed ipconfig output | `ipconfig /all` |

# Task 7: Windows Registry (regedit)

The Windows Registry is a central hierarchical database storing configuration
data for the OS, users, applications, and hardware.

## What the Registry Stores

- User profiles and preferences
- Installed applications and associated file types
- Hardware configuration and device settings
- Active network ports and service configurations
- Startup program entries

## Root Keys

| Key | Contains |
|-----|----------|
| `HKEY_LOCAL_MACHINE` (HKLM) | System-wide hardware and OS settings |
| `HKEY_CURRENT_USER` (HKCU) | Settings for the currently logged-in user |
| `HKEY_USERS` (HKU) | All user profiles on the system |
| `HKEY_CLASSES_ROOT` (HKCR) | File type associations and COM objects |
| `HKEY_CURRENT_CONFIG` (HKCC) | Current hardware profile |

**Security relevance:** The registry is a primary persistence mechanism for malware — `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` and the HKLM equivalent are the most commonly abused keys for auto-starting malicious executables on login.

## Questions

| Question | Answer |
|----------|--------|
| Command to open Registry Editor | `regedt32.exe` |

# Key Concepts Demonstrated

- **MSConfig** is a diagnostic tool, not a startup manager — Task Manager handles startup items
- **UAC** is a privilege separation mechanism; disabling it removes a key barrier against silent privilege escalation
- **Scheduled tasks and registry Run keys** are the two most common Windows persistence mechanisms — understanding their legitimate use is prerequisite to detecting their abuse
- **WMI** is a powerful management interface and a heavily abused attacker tool — knowing normal WMI behaviour is essential for detection
- **Event Viewer** is the primary forensic source for Windows incident investigation — Security, System, and Application logs are the first stops
- **Environment variables** control system-wide path resolution — tampering with `PATH` or `ComSpec` is a classic privilege escalation technique

# Key Takeaways

- Windows internals knowledge is a prerequisite for effective offensive and defensive security work on Windows targets
- Every administrative utility (compmgmt, msinfo32, regedit) has a direct security relevance — legitimate use and attacker abuse patterns overlap
- `ipconfig /all`, `netstat`, `whoami`, and `net` are the first commands run during post-exploitation enumeration for good reason — they reveal the information needed to move laterally
- The registry is not just configuration storage — it is the central target for persistence, credential harvesting, and lateral movement techniques

# Full Technical Report

📄 **Detailed Lab Report**
- [⬇️ Download PDF](/assets/reports/THM-Windows-Fundamentals.pdf)
- [👁 View in new tab](/assets/reports/THM-Windows-Fundamentals.pdf){:target="_blank"}