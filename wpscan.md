# Vulnerability Assessment: WPScan

## Overview
WPScan is a command-line tool used to scan WordPress websites for security vulnerabilities. It helps identify outdated plugins, themes, weak passwords, and core vulnerabilities.

## Key Features
- Scan WordPress core, plugins, and themes for known vulnerabilities
- Detect weak passwords and default credentials
- Enumeration of usernames and plugins
- Generate reports in various formats

## Installation
```bash
# Install WPScan on Linux
sudo apt update
sudo apt install wpscan

# Verify installation
wpscan --help
Basic Usage
bash
Copiar
Editar
# Scan a target WordPress site
wpscan --url https://example.com

# Enumerate plugins
wpscan --url https://example.com --enumerate p

# Enumerate users
wpscan --url https://example.com --enumerate u

# Scan with API token for vulnerability database
wpscan --url https://example.com --api-token YOUR_TOKEN
Tips
Regularly update WPScan and its vulnerability database using wpscan --update

Use safe scanning options on production sites to avoid disruptions

Combine WPScan with other reconnaissance tools for comprehensive assessment

Export results for reporting purposes