### **Medusa.md**
```markdown
# Password Cracking: Medusa

## Overview
Medusa is a speedy, parallel, modular login brute-forcing tool. It is designed for penetration testers to automate login attacks against multiple services efficiently.

## Key Features
- Supports many protocols (SSH, FTP, HTTP, SMB, etc.)
- Parallel login attempts for speed
- Modular design for flexibility
- Works with usernames and passwords from files

## Installation
```bash
sudo apt update
sudo apt install medusa
medusa -h
Basic Usage
bash
Copiar
Editar
# SSH brute-force attack
medusa -h 192.168.1.100 -u root -P /usr/share/wordlists/rockyou.txt -M ssh

# FTP brute-force attack
medusa -h 192.168.1.100 -U users.txt -P passwords.txt -M ftp

# HTTP login brute-force
medusa -h 192.168.1.100 -U users.txt -P passwords.txt -M http
Tips
Use small, focused wordlists for initial tests

Combine with Hydra for cross-validation of results

Run in verbose mode -v for detailed logging