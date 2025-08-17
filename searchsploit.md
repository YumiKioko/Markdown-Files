---
lang: en
title: SearchSploit Complete Walkthrough
viewport: width=device-width, initial-scale=1.0
---

::: container
::: header
# SearchSploit {#searchsploit .title}

Complete Walkthrough & Reference Guide
:::

::: navigation
::: nav-title
Quick Navigation
:::

::: nav-links
[Overview](#overview){.nav-link}
[Installation](#installation){.nav-link} [Basic
Usage](#basic-usage){.nav-link} [Advanced](#advanced){.nav-link}
[Examples](#examples){.nav-link} [Tips & Tricks](#tips){.nav-link}
:::
:::

::: {#overview .section}
## What is SearchSploit? {#what-is-searchsploit .section-title}

SearchSploit is a command-line search tool for Exploit Database that
allows you to take a copy of the Exploit Database with you wherever you
go. It\'s an essential tool for penetration testers and security
researchers.

::: tip
::: tip-content
**Key Features:**\
• Offline searching of exploit database\
• Integration with Metasploit\
• Multiple search options and filters\
• Regular updates from Exploit-DB
:::
:::
:::

::: {#installation .section}
## Installation {#installation .section-title}

::: step
::: step-number
1
:::

::: step-content
### Update System

::: code-block
`sudo apt update && sudo apt upgrade -y`
:::
:::
:::

::: step
::: step-number
2
:::

::: step-content
### Install SearchSploit

::: code-block
`sudo apt install exploitdb -y`
:::
:::
:::

::: step
::: step-number
3
:::

::: step-content
### Alternative Installation (Git)

::: code-block
`git clone https://github.com/offensive-security/exploitdb.git /opt/exploitdb`
:::

::: code-block
`ln -sf /opt/exploitdb/searchsploit /usr/local/bin/searchsploit`
:::
:::
:::

::: step
::: step-number
4
:::

::: step-content
### Verify Installation

::: code-block
`searchsploit -h`
:::

::: command-output
Usage: searchsploit \[options\] term1 \[term2\] \... \[termN\]
========== Examples ========== searchsploit afd windows local
searchsploit -t oracle windows searchsploit -p 39446 searchsploit linux
kernel 3.2 \--exclude=\"(PoC)\|/dos/\"
:::
:::
:::
:::

::: {#basic-usage .section}
## Basic Usage {#basic-usage .section-title}

### Command Structure

::: code-block
`searchsploit [options] search_terms`
:::

### Essential Commands

::: table-container
  Command                    Description                        Example
  -------------------------- ---------------------------------- -------------------------------------
  `searchsploit apache`      Basic search for Apache exploits   Returns all Apache-related exploits
  `searchsploit -t apache`   Search in title only               More precise results
  `searchsploit -e apache`   Exact match search                 Exact \"apache\" matches only
  `searchsploit -s apache`   Strict search                      Case-sensitive search
:::
:::

::: {#advanced .section}
## Advanced Features {#advanced-features .section-title}

### Filtering Options

::: step
::: step-number
1
:::

::: step-content
#### Exclude Terms

::: code-block
`searchsploit linux kernel --exclude="dos"`
:::

Excludes results containing \"dos\"
:::
:::

::: step
::: step-number
2
:::

::: step-content
#### Platform Specific

::: code-block
`searchsploit windows local privilege`
:::

Search for Windows local privilege escalation
:::
:::

::: step
::: step-number
3
:::

::: step-content
#### JSON Output

::: code-block
`searchsploit apache --json`
:::

Output results in JSON format
:::
:::

### Working with Exploits

::: step
::: step-number
1
:::

::: step-content
#### View Exploit Path

::: code-block
`searchsploit -p 39446`
:::

::: command-output
Exploit: Linux Kernel 4.4.0 (Ubuntu 14.04/16.04 x86-64) - \'AF_PACKET\'
Race Condition Privilege Escalation URL:
https://www.exploit-db.com/exploits/39446 Path:
/usr/share/exploitdb/exploits/linux/local/39446.c File Type: C source,
ASCII text
:::
:::
:::

::: step
::: step-number
2
:::

::: step-content
#### Mirror/Copy Exploit

::: code-block
`searchsploit -m 39446`
:::

Copies the exploit to current directory
:::
:::

::: step
::: step-number
3
:::

::: step-content
#### Examine Exploit

::: code-block
`searchsploit -x 39446`
:::

Opens exploit in terminal viewer
:::
:::
:::

::: {#examples .section}
## Practical Examples {#practical-examples .section-title}

::: step
::: step-number
1
:::

::: step-content
#### Search for SMB Exploits

::: code-block
`searchsploit smb windows remote`
:::

::: command-output
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
Exploit Title \| Path
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--
Microsoft Windows - \'EternalBlue\' SMB Remote\... \|
windows/remote/42315.py Microsoft Windows 7/8.1/2008 R2/2012 R2/2016\...
\| windows/remote/42030.py Microsoft Windows SMB Server -
\'EternalRomance\' \| windows/remote/43970.rb
:::
:::
:::

::: step
::: step-number
2
:::

::: step-content
#### Find Privilege Escalation

::: code-block
`searchsploit linux kernel 4.15 local`
:::

Search for Linux kernel 4.15 local privilege escalation
:::
:::

::: step
::: step-number
3
:::

::: step-content
#### Web Application Exploits

::: code-block
`searchsploit wordpress 5.0 --exclude="dos"`
:::

WordPress 5.0 exploits excluding denial of service
:::
:::

::: step
::: step-number
4
:::

::: step-content
#### Multiple Terms Search

::: code-block
`searchsploit "apache 2.4" "remote code execution"`
:::

Search with quoted phrases for exact matches
:::
:::
:::

::: {#tips .section}
## Tips & Best Practices {#tips-best-practices .section-title}

::: tip
::: tip-content
**Update Regularly:** Keep your exploit database updated

::: {.code-block style="margin-top: 0.5rem;"}
`searchsploit -u`
:::
:::
:::

::: tip
::: tip-content
**Use Version Numbers:** Include specific version numbers for better
results

::: {.code-block style="margin-top: 0.5rem;"}
`searchsploit "apache 2.4.41" instead of searchsploit apache`
:::
:::
:::

::: tip
::: tip-content
**Combine with Nmap:** Use nmap results to guide your searches

::: {.code-block style="margin-top: 0.5rem;"}
`nmap -sV target_ip`
:::

::: code-block
`searchsploit "service_name version_number"`
:::
:::
:::

::: warning
::: warning-content
**Legal Notice:** Only use SearchSploit and exploits on systems you own
or have explicit permission to test. Unauthorized access to computer
systems is illegal.
:::
:::

### Common Search Patterns

::: table-container
  Pattern             Use Case                   Example
  ------------------- -------------------------- ------------------------------------
  Service + Version   Target specific software   `searchsploit "openssh 7.4"`
  OS + Kernel         System exploits            `searchsploit "linux kernel 4.15"`
  CVE Numbers         Known vulnerabilities      `searchsploit CVE-2019-0708`
  Platform + Type     Exploit categories         `searchsploit windows remote`
:::

### Integration with Other Tools

::: step
::: step-number
1
:::

::: step-content
#### Metasploit Integration

::: code-block
`searchsploit -m apache --nmap nmap_output.xml`
:::

Automatically search based on Nmap results
:::
:::

::: step
::: step-number
2
:::

::: step-content
#### Custom Database Path

::: code-block
`searchsploit -p 39446 --path /custom/path/to/exploitdb`
:::

Use custom exploit database location
:::
:::

::: tip
::: tip-content
**Pro Tip:** Create aliases for common searches in your .bashrc

::: {.code-block style="margin-top: 0.5rem;"}
`alias sswin="searchsploit windows remote"`
:::

::: code-block
`alias sslinux="searchsploit linux local privilege"`
:::
:::
:::
:::
:::
