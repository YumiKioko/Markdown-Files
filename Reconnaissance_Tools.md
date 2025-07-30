
# Reconnaissance Tools Cheat Sheet

---

## Passive Reconnaissance

### OSINT Tools

#### **theHarvester**
- Purpose: Collect emails, subdomains, IPs, and more from public sources.
- Usage:
```bash
theHarvester -d target.com -l 500 -b all
```
- `-d`: target domain  
- `-l`: limit of results  
- `-b`: data source (e.g., all, google, bing, etc.)

---

#### **Recon-ng**
- Purpose: Modular web reconnaissance framework, similar to Metasploit.
- Features: Automates information gathering with modules for domain info, social media, and more.
- Usage:
```bash
recon-ng
# Inside framework
workspaces create target
modules search domain
modules load <module_name>
options set SOURCE target.com
run
```

---

### DNS Enumeration Tools

#### **dnsrecon**
- Purpose: Performs DNS enumeration, zone transfers, brute force subdomain discovery.
- Usage:
```bash
dnsrecon -d target.com -t std
```
- `-d`: target domain  
- `-t`: type of test (e.g., std, brt for brute force)

---

#### **sublist3r**
- Purpose: Fast subdomain enumeration using multiple search engines.
- Usage:
```bash
sublist3r -d target.com -o subdomains.txt
```
- `-d`: target domain  
- `-o`: output file for subdomains found

---

## Active Reconnaissance

### Network Scanning

#### **nmap**
- Purpose: Network discovery and security auditing tool, scans ports, services, OS detection.

##### Basic Network Discovery
```bash
nmap -sn <IP>/24
```
- `-sn`: Ping scan, no port scan; used to discover live hosts.

##### Full Port Scan with Service and OS Detection
```bash
nmap -sS -sCV -O -A -p- <IP>
```
- `-sS`: TCP SYN stealth scan  
- `-sC`: Default scripts scan  
- `-sV`: Service/version detection  
- `-O`: OS detection  
- `-A`: Aggressive scan (includes above plus traceroute)  
- `-p-`: Scan all ports (1-65535)

##### Scan without DNS Resolution
```bash
nmap -Pn -n -T4 -sCV <IP>
```
- `-Pn`: No ping, treat hosts as online  
- `-n`: No DNS resolution  
- `-T4`: Faster timing template  
- `-sC`: Default scripts  
- `-V`: Service/version detection

---

# Notes
- Replace `<IP>` or `target.com` with the real target IP or domain.
- Use these tools ethically and only on targets you have permission to test.
