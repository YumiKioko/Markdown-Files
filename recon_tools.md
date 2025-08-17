---

# Recon-ng.md

````markdown
# Reconnaissance & Information Gathering: Recon-ng

## Overview
Recon-ng is a full-featured reconnaissance framework written in Python. It allows penetration testers to automate the gathering of information about targets in a structured manner. Its modular design makes it easy to extend and integrate with other tools.

## Key Features
- Modular architecture for information gathering
- Web-based interface for data management
- API integration with services like Shodan, Google, and WHOIS
- Ability to create custom modules

## Installation
```bash
git clone https://github.com/lanmaster53/recon-ng.git
cd recon-ng
pip install -r REQUIREMENTS
python recon-ng
````

## Basic Workflow

1. **Workspace Setup**

```bash
workspaces create target_company
workspaces select target_company
```

2. **Add Domains or Targets**

```bash
add domains example.com
```

3. **Load Modules**

```bash
modules search all
modules load recon/domains-hosts/bing_domain_web
```

4. **Run Modules**

```bash
options set SOURCE example.com
run
```

5. **Export Results**

```bash
export csv /path/to/output.csv
```

## Useful Modules

- `recon/domains-hosts/bing_domain_web` – Enumerate subdomains via Bing.
- `recon/contacts-credentials/`
- `recon/netblocks-contacts/` – Gather email addresses from netblocks.
- `recon/hosts-hosts/shodan_ip` – Integrate with Shodan for host info.

## Tips

- Always verify API keys for modules like Shodan or HaveIBeenPwned.
- Use workspaces to keep different targets organized.
- Combine module results for more in-depth reporting.

````

---

# Shodan.md

```markdown
# Reconnaissance & Information Gathering: Shodan

## Overview
Shodan is a search engine for Internet-connected devices. It can identify servers, routers, webcams, IoT devices, and more. Shodan is often used in penetration testing to find exposed services and vulnerabilities.

## Key Features
- Search devices by IP, hostname, or service
- Filter results by country, port, operating system, and more
- API integration for automation
- Discover vulnerable or misconfigured devices

## Account Setup
1. Sign up at [Shodan.io](https://www.shodan.io/)
2. Retrieve your API key from the dashboard

## Installation
```bash
pip install shodan
````

## Basic Commands

```bash
# Initialize Shodan with API key
shodan init YOUR_API_KEY

# Search example: find webcams
shodan search webcam

# Search with filters: country and port
shodan search "apache port:80 country:US"

# Get info about an IP
shodan host 8.8.8.8
```

## Python Integration

```python
import shodan

api = shodan.Shodan("YOUR_API_KEY")
result = api.search("apache")
print(result['matches'][0])
```

## Tips

- Use Shodan filters to narrow down searches efficiently.
- Monitor your target’s IP ranges for exposure.
- Combine with other reconnaissance tools like Recon-ng for automation.

````

---

# GoogleDorks.md

```markdown
# Reconnaissance & Information Gathering: Google Dorks

## Overview
Google Dorks are advanced search operators used to find specific information indexed by Google. Attackers and penetration testers can use them to locate sensitive data, exposed directories, or vulnerable applications.

## Common Google Dork Operators
| Operator | Description |
|----------|-------------|
| `site:` | Search within a specific domain |
| `filetype:` | Search for specific file types (e.g., pdf, xls) |
| `inurl:` | Search for keywords in the URL |
| `intitle:` | Search for keywords in the page title |
| `cache:` | Access Google’s cached version of a page |
| `allintext:` | Search for keywords in the text of the page |

## Examples
```text
# Find PDF documents on a site
site:example.com filetype:pdf

# Find login pages
inurl:login site:example.com

# Search for exposed configuration files
filetype:env inurl:.env site:example.com
````

## Tips

- Combine multiple operators for more targeted results.
- Be cautious: some results may contain sensitive data.
- Automate Google Dorking with scripts carefully to avoid rate limits.

## References

- Google Advanced Search Operators: [https://support.google.com/websearch/answer/2466433](https://support.google.com/websearch/answer/2466433)
- OWASP Google Hacking Database: [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)

```
```
