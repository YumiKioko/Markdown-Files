### **Hydra.md**
```markdown
# Password Cracking: Hydra

## Overview
Hydra is a fast network logon cracker supporting many protocols. It automates password guessing against remote services for penetration testing purposes.

## Key Features
- Supports numerous protocols (SSH, FTP, HTTP, SMTP, etc.)
- Parallelized brute-force attacks
- Flexible input options (wordlists, username/password combos)
- Command-line interface for scripting

## Installation
```bash
sudo apt update
sudo apt install hydra
hydra -h
Basic Usage

# SSH login brute-force
hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.100

# FTP login brute-force
hydra -L users.txt -P passwords.txt ftp://192.168.1.100

# HTTP login brute-force
hydra -L users.txt -P passwords.txt 192.168.1.100 http-get /login
Tips
Start with small, focused wordlists to reduce detection

Use verbose mode -vV to see detailed attempts

Combine with recon tools to discover valid usernames first
