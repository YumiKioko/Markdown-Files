
# 🛠️ Abusing Windows Internals for Offensive Security

> **Disclaimer**: This guide is for **educational** and **authorized penetration testing** only. Do **not** use any of this information on systems without **explicit written permission**.

---

## 📚 Table of Contents

1. [Process Injection & Hollowing](#1-process-injection--hollowing)
2. [Direct System Calls (Syscalls)](#2-direct-system-calls-syscalls)
3. [Token Manipulation & Privilege Escalation](#3-token-manipulation--privilege-escalation)
4. [AMSI & ETW Bypass](#4-amsi--etw-bypass)
5. [DLL Sideloading, Hijacking, and Search Order Abuse](#5-dll-sideloading-hijacking-and-search-order-abuse)
6. [WMI & COM Abuse](#6-wmi--com-abuse)
7. [Process Doppelgänging / Ghosting](#7-process-doppelgänging--ghosting)
8. [LSASS Dumping & Credential Theft](#8-lsass-dumping--credential-theft)
9. [Persistence via Windows Internals](#9-persistence-via-windows-internals)

---

## 1. 🚨 Process Injection & Hollowing

**Concept:** Replace or inject shellcode into a legitimate process to evade detection.

**Technique:**
```c
CreateProcess(...); // Create in suspended state
NtUnmapViewOfSection(); // Unmap target process
VirtualAllocEx(); // Allocate memory in remote process
WriteProcessMemory(); // Write shellcode
SetThreadContext(); // Point to shellcode
ResumeThread(); // Resume execution
```

Used in: `process hollowing`, `RunPE`, malware loaders.

---

## 2. 💻 Direct System Calls (Syscalls)

**Concept:** Bypass user-mode hooks (used by AV/EDR) via low-level system calls.

**Tools:** 
- SysWhispers2 / 3
- Hell’s Gate
- TARTAN
- Blackbone (C++)

**Example:**
```c
NTSTATUS status = NtAllocateVirtualMemory(...); // Direct syscall
```

Use to replace: `VirtualAlloc`, `CreateThread`, `ReadProcessMemory`, etc.

---

## 3. 🪪 Token Manipulation & Privilege Escalation

**Concept:** Steal or impersonate access tokens from privileged users (e.g., SYSTEM).

**Techniques:**
- `OpenProcessToken()`
- `DuplicateTokenEx()`
- `ImpersonateLoggedOnUser()`

**Tool example:**
```cmd
whoami /priv
```

Exploit misconfigured privileges: `SeImpersonate`, `SeDebug`, `SeAssignPrimaryToken`.

---

## 4. 🛡️ AMSI & ETW Bypass

**Concept:** Disable or bypass Windows built-in monitoring.

**AMSI (Antimalware Scan Interface) bypass:**
```csharp
IntPtr amsiContext = GetProcAddress(..., "AmsiScanBuffer");
Patch memory with "0xC3" (RET)
```

**ETW (Event Tracing for Windows) bypass:**
```c
ZeroMemory(pointers to EtwEventWrite);
```

**Tools:** 
- `Invoke-BypassAMSI.ps1`
- `PatchAMSI.dll`

---

## 5. 🧩 DLL Sideloading, Hijacking, and Search Order Abuse

**Concept:** Trick a high-privilege process into loading a malicious DLL.

**Methods:**
- Place malicious DLL where Windows will load it first
- Exploit missing DLLs in known applications
- Abuse `%PATH%` environment

**Tools:**
- `Process Monitor`
- `DLLSpy`
- Custom DLL loader

---

## 6. 🧠 WMI & COM Abuse

**WMI (Windows Management Instrumentation):**
- Persistence via:
```powershell
Set-WmiInstance -Namespace "root\subscription"
```

- Remote code execution via:
```powershell
Invoke-WmiMethod -Class Win32_Process
```

**COM Hijacking:**
- Abusing registry keys like:
```reg
HKCU\Software\Classes\CLSID\{...}\InprocServer32
```

---

## 7. 👻 Process Doppelgänging / Ghosting

**Concept:** Create a process from a deleted or modified file to avoid detection.

**Steps:**
- Create transaction via `CreateTransaction`
- Write PE in transaction
- Roll back, making process untraceable
- Execute process using `NtCreateProcessEx`

Evades: Antivirus, file scanning, whitelisting.

---

## 8. 🔓 LSASS Dumping & Credential Theft

**Goal:** Extract credentials from memory.

**Methods:**
- `MiniDumpWriteDump`
- `ReadProcessMemory`
- Mimikatz

**Process:**
```cmd
procdump64.exe -ma lsass.exe lsass.dmp
mimikatz.exe -> sekurlsa::minidump
```

Bypass protections with:
- `PPL Bypass`
- `Direct Syscalls`

---

## 9. 🔁 Persistence via Windows Internals

**Techniques:**
- WMI Event Subscription
- Scheduled Tasks (hidden with `COM` or `WMI`)
- Registry (e.g., `Run`, `UserInitMprLogonScript`)
- DLL hijacking in auto-elevated apps
- AppCert DLLs (`HKLM\System\CurrentControlSet\Control\Session Manager\AppCertDlls`)

---

## ✅ Summary

| Technique                      | Use Case                     |
|-------------------------------|------------------------------|
| Process Hollowing             | Shellcode execution stealth  |
| Syscalls                      | AV/EDR evasion               |
| Token Stealing                | Privilege escalation         |
| DLL Hijacking                 | Execution & persistence      |
| WMI + COM                     | Stealthy code execution      |
| LSASS Dumping                 | Credential harvesting        |

---

## ⚠️ Legal Notice

Use this guide **only** in environments where:
- You have **written authorization**
- You're performing **red teaming**, **pentesting**, or **research**
Unauthorized use is illegal and unethical.

---
