# Vulnerability Assessment: Nikto

## Overview
Nikto is an open-source web server scanner that identifies security vulnerabilities, outdated software, and misconfigurations. It is useful for penetration testing and web server hardening.

## Key Features
- Detects over 6,700 potentially dangerous files/CGIs
- Identifies outdated server software
- Checks server configuration issues
- Reports in multiple formats (TXT, HTML, CSV)

## Installation
```bash
git clone https://github.com/sullo/nikto.git
cd nikto
perl nikto.pl -h
```

## Basic Usage
```bash
# Scan a target website
perl nikto.pl -h http://example.com

# Specify output file
perl nikto.pl -h http://example.com -o report.html -Format htm
```

## Tips
- Combine with other reconnaissance tools for better assessment
- Regularly update Nikto’s database using `nikto.pl -update`
- Focus on scanning non-production servers first to avoid service disruption