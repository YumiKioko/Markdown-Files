# Password Cracking: John the Ripper

## Overview
John the Ripper is a fast, open-source password cracking tool used to detect weak passwords. It supports a wide variety of hash types and is widely used in penetration testing.

## Key Features
- Cracks multiple types of password hashes (MD5, SHA, DES, etc.)
- Supports dictionary and brute-force attacks
- Multi-platform support (Linux, Windows, macOS)
- Can be extended with custom rules

## Installation
```bash
sudo apt update
sudo apt install john
john --test
Basic Usage
bash
Copiar
Editar
# Test a password file with default mode
john passwords.txt

# Use a specific wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt

# Show cracked passwords
john --show passwords.txt

# Crack a specific hash type
john --format=md5 hashes.txt
Tips
Use incremental mode for unknown password patterns

Combine with hash extraction tools for more efficiency

Keep your wordlists updated for better success rates