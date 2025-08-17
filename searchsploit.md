# Vulnerability Assessment: SearchSploit

## Overview
SearchSploit is a command-line tool that allows penetration testers to search and access the Exploit Database (Exploit-DB) offline. It helps identify public exploits for known vulnerabilities quickly.

## Key Features
- Local access to Exploit-DB repository
- Search by software, version, or CVE
- Copy exploits directly for testing
- Supports filtering by platform or type

## Installation
```bash
# Install ExploitDB package (Debian/Ubuntu)
sudo apt update
sudo apt install exploitdb

# Verify installation
searchsploit -h
```

## Basic Usage
```bash
# Search for exploits for a specific software
searchsploit apache

# Search by CVE
searchsploit CVE-2023-12345

# List all exploits for a platform
searchsploit --platform linux

# Copy exploit to current directory
searchsploit -m 12345.py
```

## Tips
- Keep the Exploit-DB repository updated:
```bash
sudo searchsploit -u
```
- Combine with Nmap or Nessus scan results for targeted exploitation
- Always verify exploits in a controlled environment before testing on production systems
- Use `searchsploit -w` to view exploits in a web browser for easier reading

## References
- Exploit Database: https://www.exploit-db.com
- SearchSploit GitHub: https://github.com/offensive-security/exploitdb