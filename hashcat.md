### **Hashcat.md**
```markdown
# Password Cracking: Hashcat

## Overview
Hashcat is a powerful and advanced password recovery tool supporting GPU acceleration. It is capable of cracking large numbers of hashes quickly.

## Key Features
- Supports CPU and GPU cracking
- Wide range of hash algorithms
- Dictionary, combinator, brute-force, and hybrid attacks
- Highly configurable rules for complex attacks

## Installation
```bash
sudo apt update
sudo apt install hashcat
hashcat -I
Basic Usage
bash
Copiar
Editar
# Crack MD5 hashes using a wordlist
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt

# Perform brute-force attack
hashcat -m 0 -a 3 hashes.txt ?a?a?a?a

# Show cracked passwords
hashcat -m 0 -a 0 --show hashes.txt
Tips
Use GPUs for faster cracking

Start with common passwords and dictionaries before brute-force

Explore hybrid attack modes for advanced cracking