---
lang: en
title: WPSCAN Complete Walkthrough
viewport: width=device-width, initial-scale=1.0
---

::: container
<div>

# WPSCAN

Complete WordPress Security Scanner Walkthrough

</div>

::: toc
### Table of Contents

-   [Introduction](#introduction)
-   [Installation](#installation)
-   [Basic Usage](#basic-usage)
-   [Advanced Options](#advanced-options)
-   [Enumeration Techniques](#enumeration)
-   [Vulnerability Scanning](#vulnerability-scanning)
-   [Brute Force Attacks](#brute-force)
-   [Output Formats](#output-formats)
-   [API Integration](#api-integration)
-   [Troubleshooting](#troubleshooting)
-   [Best Practices](#best-practices)
:::

::: {#introduction .section .section}
## Introduction to WPSCAN

WPSCAN is a free, open-source WordPress security scanner written in
Ruby. It\'s designed to identify security vulnerabilities in WordPress
installations, including:

::: feature-grid
::: feature-card
### Core Vulnerabilities

Scans WordPress core files for known security issues and outdated
versions.
:::

::: feature-card
### Plugin & Theme Scanning

Identifies vulnerable plugins and themes with comprehensive database
checks.
:::

::: feature-card
### User Enumeration

Discovers usernames and performs brute force attacks on authentication.
:::

::: feature-card
### Configuration Issues

Detects misconfigurations and security weaknesses in WordPress setup.
:::
:::

::: warning
**⚠️ Legal Warning:** Only use WPSCAN on websites you own or have
explicit permission to test. Unauthorized scanning may violate terms of
service and local laws.
:::
:::

::: {#installation .section .section}
## Installation Guide

### System Requirements

-   Ruby 2.7 or higher
-   RubyGems package manager
-   Internet connection for vulnerability database updates

### Installation Methods

#### Method 1: Gem Installation (Recommended)

::: code-block
    gem install wpscan
:::

#### Method 2: Docker Installation

::: code-block
    docker pull wpscanteam/wpscan
    docker run -it --rm wpscanteam/wpscan --url https://example.com
:::

#### Method 3: From Source

::: code-block
    git clone https://github.com/wpscanteam/wpscan.git
    cd wpscan
    bundle install
    ruby wpscan.rb --url https://example.com
:::

#### Method 4: Kali Linux (Pre-installed)

::: code-block
    wpscan --update
    wpscan --url https://example.com
:::

::: success
**✅ Verification:** Run `wpscan --version` to confirm installation.
:::
:::

::: {#basic-usage .section .section}
## Basic Usage

### Simple Scan

::: terminal
wpscan \--url https://target-website.com
:::

### Scan with User Enumeration

::: terminal
wpscan \--url https://target-website.com \--enumerate u
:::

### Comprehensive Scan

::: terminal
wpscan \--url https://target-website.com \--enumerate ap,at,cb,dbe
:::

### Understanding Basic Output

::: code-block
    [+] URL: https://example.com/
    [+] Started: Thu Aug 17 10:30:00 2025

    [+] WordPress version 6.2.2 identified (Latest, released on 2023-05-20).

    [+] WordPress theme in use: twentytwentythree
     | Location: https://example.com/wp-content/themes/twentytwentythree/
     | Last Updated: 2023-05-10T00:00:00.000Z
     | [!] The version is out of date, the latest version is 1.2

    [+] Enumerating All Plugins (via Passive Methods)
    [i] No plugins found.

    [+] Finished: Thu Aug 17 10:32:15 2025
    [+] Requests Done: 125
    [+] Cached Requests: 0
    [+] Data Sent: 29.156 KB
    [+] Data Received: 142.534 KB
    [+] Memory used: 85.156 MB
    [+] Elapsed time: 00:02:15
:::
:::

::: {#advanced-options .section .section}
## Advanced Options

### Core Command Line Options

  Option                 Description                      Example
  ---------------------- -------------------------------- ----------------------------
  \--url                 Target URL to scan               \--url https://example.com
  \--enumerate           Enumeration mode                 \--enumerate u,p,t
  \--threads             Number of threads (1-50)         \--threads 20
  \--throttle            Milliseconds between requests    \--throttle 1000
  \--random-user-agent   Use random user agents           \--random-user-agent
  \--force               Force scan non-WordPress sites   \--force

### Authentication Options

::: code-block
    # HTTP Basic Authentication
    wpscan --url https://example.com --http-auth username:password

    # Cookie-based Authentication
    wpscan --url https://example.com --cookie "session=abc123"

    # Custom Headers
    wpscan --url https://example.com --headers "Authorization: Bearer token123"
:::

### Proxy and Network Options

::: code-block
    # Using Proxy
    wpscan --url https://example.com --proxy http://127.0.0.1:8080

    # SOCKS Proxy
    wpscan --url https://example.com --proxy socks5://127.0.0.1:1080

    # Custom Timeout
    wpscan --url https://example.com --request-timeout 60

    # Disable SSL Verification
    wpscan --url https://example.com --disable-tls-checks
:::
:::

::: {#enumeration .section .section}
## Enumeration Techniques

### Enumeration Modes

  Mode   Description          Command
  ------ -------------------- ------------------
  u      Users                \--enumerate u
  ap     All Plugins          \--enumerate ap
  p      Popular Plugins      \--enumerate p
  vp     Vulnerable Plugins   \--enumerate vp
  at     All Themes           \--enumerate at
  t      Popular Themes       \--enumerate t
  vt     Vulnerable Themes    \--enumerate vt
  cb     Config Backups       \--enumerate cb
  dbe    Database Exports     \--enumerate dbe

### User Enumeration Examples

::: code-block
    # Basic user enumeration
    wpscan --url https://example.com --enumerate u

    # Enumerate users with ID range
    wpscan --url https://example.com --enumerate u1-100

    # Enumerate users from author archives
    wpscan --url https://example.com --enumerate u --detection-mode aggressive
:::

::: code-block
    [+] Enumerating Users (via Passive and Aggressive Methods)
     Brute Forcing Author IDs - Time: 00:00:02 <==> (10 / 10) 100.00% Time: 00:00:02

    [i] User(s) Identified:

    [+] admin
     | Found By: Author Posts - Display Name (Passive Detection)
     | Confirmed By:
     |  Rss Generator (Passive Detection)
     |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)

    [+] john.doe
     | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
     | Confirmed By: Login Error Messages (Aggressive Detection)
:::

### Plugin and Theme Enumeration

::: code-block
    # Enumerate all plugins (aggressive)
    wpscan --url https://example.com --enumerate ap --detection-mode aggressive

    # Enumerate vulnerable plugins only
    wpscan --url https://example.com --enumerate vp

    # Enumerate themes with version detection
    wpscan --url https://example.com --enumerate at --plugins-version-all
:::
:::

::: {#vulnerability-scanning .section .section}
## Vulnerability Scanning

### WordPress Vulnerability Database

::: info
**ℹ️ Info:** WPSCAN uses the WPVulnDB (WordPress Vulnerability Database)
to identify known security issues. Register for a free API token to
access the full database.
:::

### API Token Registration

::: code-block
    # Register at https://wpscan.com/register
    # Add token to scan
    wpscan --url https://example.com --api-token YOUR_API_TOKEN

    # Save token globally
    wpscan --url https://example.com --api-token YOUR_TOKEN --save-api-token
:::

### Vulnerability Detection Examples

::: code-block
    # Full vulnerability scan
    wpscan --url https://example.com --api-token YOUR_TOKEN --enumerate ap,at,u

    # Only scan for vulnerable components
    wpscan --url https://example.com --api-token YOUR_TOKEN --enumerate vp,vt

    # Include main theme in vulnerability check
    wpscan --url https://example.com --api-token YOUR_TOKEN --enumerate at --plugins-detection aggressive
:::

::: code-block
    [+] contact-form-7
     | Location: https://example.com/wp-content/plugins/contact-form-7/
     | Last Updated: 2023-07-15T12:34:00.000Z
     | [!] The version is out of date, the latest version is 5.7.8
     | [!] 2 vulnerabilities identified:
     |
     | [!] Title: Contact Form 7 < 5.7.6 - Stored Cross-Site Scripting
     |     Fixed in: 5.7.6
     |     References:
     |      - https://wpscan.com/vulnerability/12345678-1234-1234-1234-123456789012
     |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-12345
     |
     | [!] Title: Contact Form 7 < 5.7.7 - Missing Authorization
     |     Fixed in: 5.7.7
     |     References:
     |      - https://wpscan.com/vulnerability/87654321-4321-4321-4321-210987654321
:::
:::

::: {#brute-force .section .section}
## Brute Force Attacks

::: warning
**⚠️ Warning:** Brute force attacks can be detected by security plugins
and may result in IP blocking. Use responsibly and only on authorized
targets.
:::

### Password Attacks

::: code-block
    # Basic brute force with wordlist
    wpscan --url https://example.com --usernames admin --passwords /path/to/wordlist.txt

    # Multiple users
    wpscan --url https://example.com --usernames admin,user1,editor --passwords passwords.txt

    # User and password from files
    wpscan --url https://example.com --usernames users.txt --passwords passwords.txt
:::

### Advanced Brute Force Options

::: code-block
    # Limit login attempts
    wpscan --url https://example.com --usernames admin --passwords passwords.txt --max-threads 5

    # Add delay between attempts
    wpscan --url https://example.com --usernames admin --passwords passwords.txt --throttle 2000

    # Multicall method (faster but detectable)
    wpscan --url https://example.com --usernames admin --passwords passwords.txt --multicall-max-passwords 100
:::

### Common Password Lists

::: feature-grid
::: feature-card
#### SecLists

/usr/share/seclists/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt
:::

::: feature-card
#### RockYou

/usr/share/wordlists/rockyou.txt
:::

::: feature-card
#### Custom WordPress

/usr/share/wpscan/passwords.txt
:::
:::

### XML-RPC Brute Force

::: code-block
    # Test if XML-RPC is enabled
    wpscan --url https://example.com --enumerate m

    # XML-RPC brute force (if available)
    wpscan --url https://example.com --usernames admin --passwords passwords.txt --multicall
:::
:::

::: {#output-formats .section .section}
## Output Formats and Reporting

### Output Formats

::: code-block
    # JSON output
    wpscan --url https://example.com --format json --output results.json

    # CLI output with colors disabled
    wpscan --url https://example.com --format cli-no-color --output results.txt

    # CLI output with minimal info
    wpscan --url https://example.com --format cli-no-banner --output results.txt
:::

### Verbose and Debug Output

::: code-block
    # Verbose output
    wpscan --url https://example.com --verbose

    # Debug mode
    wpscan --url https://example.com --debug

    # Log to file
    wpscan --url https://example.com --log results.log
:::

### Custom Report Generation

::: code-block
    # Generate comprehensive report
    wpscan --url https://example.com \
      --enumerate ap,at,u,cb,dbe \
      --api-token YOUR_TOKEN \
      --format json \
      --output comprehensive_scan.json \
      --verbose
:::

### Sample JSON Output Structure

::: code-block
    {
      "version": {
        "number": "6.3.1",
        "release_date": "2023-08-17",
        "status": "latest"
      },
      "main_theme": {
        "slug": "twentytwentythree",
        "location": "https://example.com/wp-content/themes/twentytwentythree/",
        "version": {
          "number": "1.1",
          "confidence": 80
        },
        "vulnerabilities": []
      },
      "plugins": {
        "contact-form-7": {
          "location": "https://example.com/wp-content/plugins/contact-form-7/",
          "version": {
            "number": "5.7.5",
            "confidence": 100
          },
          "vulnerabilities": [
            {
              "title": "Contact Form 7 < 5.7.6 - Stored XSS",
              "fixed_in": "5.7.6",
              "references": {
                "cve": ["CVE-2023-12345"],
                "url": ["https://wpscan.com/vulnerability/12345"]
              }
            }
          ]
        }
      },
      "users": [
        {
          "id": 1,
          "username": "admin",
          "found_by": "Author Posts - Display Name"
        }
      ]
    }
:::
:::

::: {#api-integration .section .section}
## API Integration and Automation

### WPScan API Token Setup

::: code-block
    # Get free API token from https://wpscan.com/api
    # Free tier: 25 requests per day
    # Paid tiers: Up to 5000 requests per day

    # Configure API token globally
    wpscan --api-token YOUR_TOKEN --save-api-token

    # Verify API token status
    wpscan --api-token YOUR_TOKEN --status
:::

### Batch Scanning Script

::: code-block
    #!/bin/bash
    # batch_wpscan.sh - Scan multiple WordPress sites

    SITES_FILE="wordpress_sites.txt"
    OUTPUT_DIR="scan_results"
    API_TOKEN="YOUR_API_TOKEN_HERE"

    mkdir -p $OUTPUT_DIR

    while IFS= read -r site; do
        if [[ $site =~ ^https?:// ]]; then
            echo "Scanning: $site"
            filename=$(echo $site | sed 's/https\?:\/\///g' | tr '/' '_')
            
            wpscan --url "$site" \
                   --api-token "$API_TOKEN" \
                   --enumerate ap,at,u \
                   --format json \
                   --output "$OUTPUT_DIR/${filename}_scan.json" \
                   --random-user-agent \
                   --throttle 1000
            
            echo "Completed: $site"
            sleep 5
        fi
    done < "$SITES_FILE"
:::

### Python Integration Example

::: code-block
    #!/usr/bin/env python3
    import subprocess
    import json
    import sys
    from datetime import datetime

    def run_wpscan(target_url, api_token=None):
        """Run WPScan against target URL"""
        cmd = [
            'wpscan',
            '--url', target_url,
            '--enumerate', 'ap,at,u',
            '--format', 'json',
            '--random-user-agent'
        ]
        
        if api_token:
            cmd.extend(['--api-token', api_token])
        
        try:
            result = subprocess.run(cmd, capture_output=True, text=True, timeout=300)
            if result.returncode == 0:
                return json.loads(result.stdout)
            else:
                print(f"Error scanning {target_url}: {result.stderr}")
                return None
        except subprocess.TimeoutExpired:
            print(f"Timeout scanning {target_url}")
            return None
        except Exception as e:
            print(f"Exception scanning {target_url}: {e}")
            return None

    def analyze_results(scan_data):
        """Analyze scan results for vulnerabilities"""
        if not scan_data:
            return
        
        vulnerabilities = []
        
        # Check WordPress core vulnerabilities
        if 'version' in scan_data and 'vulnerabilities' in scan_data['version']:
            vulnerabilities.extend(scan_data['version']['vulnerabilities'])
        
        # Check plugin vulnerabilities
        if 'plugins' in scan_data:
            for plugin_name, plugin_data in scan_data['plugins'].items():
                if 'vulnerabilities' in plugin_data:
                    vulnerabilities.extend(plugin_data['vulnerabilities'])
        
        # Check theme vulnerabilities
        if 'themes' in scan_data:
            for theme_name, theme_data in scan_data['themes'].items():
                if 'vulnerabilities' in theme_data:
                    vulnerabilities.extend(theme_data['vulnerabilities'])
        
        return vulnerabilities

    if __name__ == "__main__":
        if len(sys.argv) < 2:
            print("Usage: python3 wpscan_automation.py  [api_token]")
            sys.exit(1)
        
        target = sys.argv[1]
        token = sys.argv[2] if len(sys.argv) > 2 else None
        
        print(f"Scanning {target} at {datetime.now()}")
        results = run_wpscan(target, token)
        
        if results:
            vulns = analyze_results(results)
            if vulns:
                print(f"Found {len(vulns)} vulnerabilities:")
                for vuln in vulns:
                    print(f"- {vuln.get('title', 'Unknown vulnerability')}")
            else:
                print("No vulnerabilities found.")
        else:
            print("Scan failed or returned no results.")
:::

### Continuous Monitoring Setup

::: code-block
    # Crontab entry for daily scans
    # Run daily at 2 AM
    0 2 * * * /usr/local/bin/wpscan --url https://example.com --api-token TOKEN --enumerate ap,at --format json --output /var/log/wpscan/daily_$(date +\%Y\%m\%d).json

    # Weekly comprehensive scan
    0 3 * * 0 /usr/local/bin/wpscan --url https://example.com --api-token TOKEN --enumerate ap,at,u,cb,dbe --format json --output /var/log/wpscan/weekly_$(date +\%Y\%m\%d).json
:::
:::

::: {#troubleshooting .section .section}
## Troubleshooting Common Issues

### Connection Problems

::: feature-grid
::: feature-card
#### SSL Certificate Issues

::: code-block
    wpscan --url https://example.com --disable-tls-checks
:::
:::

::: feature-card
#### Timeout Problems

::: code-block
    wpscan --url https://example.com --request-timeout 120
:::
:::

::: feature-card
#### Rate Limiting

::: code-block
    wpscan --url https://example.com --throttle 3000 --max-threads 1
:::
:::

::: feature-card
#### User Agent Blocking

::: code-block
    wpscan --url https://example.com --random-user-agent
:::
:::
:::

### Detection Evasion

::: code-block
    # Stealth scan configuration
    wpscan --url https://example.com \
      --random-user-agent \
      --throttle 5000 \
      --max-threads 1 \
      --request-timeout 60 \
      --detection-mode passive

    # Use proxy chain
    wpscan --url https://example.com \
      --proxy http://proxy1.example.com:8080 \
      --random-user-agent \
      --throttle 10000
:::

### Common Error Messages

  Error                                                                  Cause                                           Solution
  ---------------------------------------------------------------------- ----------------------------------------------- -------------------------------------------------
  The remote website is up, but does not seem to be running WordPress.   Target is not WordPress or heavily customized   Use \--force flag or verify target
  API token limit reached                                                Daily API quota exceeded                        Wait 24 hours or upgrade plan
  Connection timeout                                                     Network issues or server blocking               Increase timeout, use proxy, or throttle
  403 Forbidden                                                          Web Application Firewall blocking               Change user agent, use proxy, or reduce threads

### Debug and Logging

::: code-block
    # Enable debug mode
    wpscan --url https://example.com --debug

    # Verbose output with logging
    wpscan --url https://example.com --verbose --log debug.log

    # Check WPScan configuration
    wpscan --help
    wpscan --version
:::

### Performance Optimization

::: info
**Performance Tips:**

-   Use appropriate thread counts (5-20 for most cases)
-   Implement throttling for stable connections
-   Use passive detection mode for stealth
-   Cache DNS lookups for batch scans
-   Monitor bandwidth usage with large wordlists
:::
:::

::: {#best-practices .section .section}
## Security Best Practices

### Ethical Guidelines

::: warning
**⚠️ Important:** Always follow these ethical guidelines:

-   Only scan websites you own or have explicit written permission to
    test
-   Respect rate limits and avoid overwhelming target servers
-   Don\'t attempt to exploit discovered vulnerabilities without
    permission
-   Report findings responsibly through proper channels
-   Comply with local laws and regulations
:::

### Recommended Scan Configurations

#### Basic Security Assessment

::: code-block
    wpscan --url https://target.com \
      --api-token YOUR_TOKEN \
      --enumerate vp,vt \
      --random-user-agent \
      --throttle 1000
:::

#### Comprehensive Audit

::: code-block
    wpscan --url https://target.com \
      --api-token YOUR_TOKEN \
      --enumerate ap,at,u,cb,dbe \
      --detection-mode aggressive \
      --plugins-version-all \
      --format json \
      --output comprehensive_audit.json \
      --verbose
:::

#### Stealth Reconnaissance

::: code-block
    wpscan --url https://target.com \
      --detection-mode passive \
      --enumerate p,t \
      --random-user-agent \
      --throttle 5000 \
      --max-threads 1
:::

### Reporting and Documentation

::: feature-grid
::: feature-card
#### Vulnerability Assessment

Document all discovered vulnerabilities with severity ratings, affected
components, and recommended fixes.
:::

::: feature-card
#### Risk Analysis

Analyze the business impact of discovered issues and prioritize
remediation efforts.
:::

::: feature-card
#### Remediation Guidance

Provide specific steps to address each vulnerability, including version
updates and configuration changes.
:::

::: feature-card
#### Follow-up Testing

Schedule re-testing to verify that vulnerabilities have been properly
addressed.
:::
:::

### Integration with Security Workflow

::: code-block
    # Pre-deployment security check
    wpscan --url https://staging.example.com \
      --api-token YOUR_TOKEN \
      --enumerate ap,at \
      --format json | \
      jq '.plugins[] | select(.vulnerabilities | length > 0)'

    # Automated vulnerability monitoring
    #!/bin/bash
    SCAN_RESULT=$(wpscan --url https://example.com --api-token TOKEN --format json --enumerate vp,vt)
    VULN_COUNT=$(echo $SCAN_RESULT | jq '[.plugins[]?.vulnerabilities[]?, .themes[]?.vulnerabilities[]?] | length')

    if [ "$VULN_COUNT" -gt 0 ]; then
        echo "ALERT: $VULN_COUNT vulnerabilities found!"
        # Send notification (email, Slack, etc.)
        echo $SCAN_RESULT | mail -s "WordPress Security Alert" admin@example.com
    fi
:::

### Security Hardening Recommendations

::: success
**Based on WPScan findings, implement these security measures:**

-   Keep WordPress core, themes, and plugins updated
-   Remove unused themes and plugins
-   Implement strong authentication (2FA)
-   Use security plugins (Wordfence, Sucuri)
-   Configure Web Application Firewall (WAF)
-   Regular security monitoring and scanning
-   Backup and disaster recovery procedures
-   Limit login attempts and user enumeration
:::

### Advanced Detection Bypass

::: code-block
    # Rotate user agents from file
    wpscan --url https://example.com --user-agents-list user_agents.txt

    # Custom headers to bypass WAF
    wpscan --url https://example.com \
      --headers "X-Forwarded-For: 192.168.1.1" \
      --headers "X-Real-IP: 10.0.0.1" \
      --random-user-agent

    # Distributed scanning through proxies
    for proxy in $(cat proxy_list.txt); do
      wpscan --url https://example.com \
        --proxy $proxy \
        --enumerate u1-10 \
        --throttle 10000
      sleep 60
    done
:::
:::

### WPSCAN Complete Walkthrough {#wpscan-complete-walkthrough style="color: #fff; margin-bottom: 15px;"}

This comprehensive guide covers all aspects of WordPress security
scanning with WPScan.

Remember: Always use WPScan responsibly and ethically. Only scan
websites you own or have explicit permission to test.

::: {style="margin-top: 20px;"}
[Version 1.0 \| Created for educational and authorized testing
purposes]{style="color: #666;"}
:::

↑
:::
