# Week 1 — Study Notes
**CEH Class + Personal Study Track**
📅 20 March 2026 → 26 March 2026

---

## Day 1 — Friday, 20 March 2026
**Topics: Intro to Offensive & Defensive Security | Careers in Cyber**

### Key Learnings

**1. Digital Assets & How the Defensive Team Protects Them**
- Digital assets are anything valuable that exists online or on a network — websites, databases, user data, financial records, internal systems
- The **defensive (Blue Team)** monitors, detects, and responds to threats to keep these assets safe
- Key defensive roles: SOC Analyst, Incident Responder, Threat Hunter
- Tools used by defenders: SIEM systems, firewalls, IDS/IPS, antivirus
- A **SOC (Security Operations Centre)** is the central team that watches for attacks 24/7

**2. Offensive Security & Ethical Hacking**
- The **offensive (Red Team)** mimics real hacker techniques — but under legal permission
- This is called **ethical hacking** or **penetration testing**
- The goal: find vulnerabilities before real attackers do
- Key principle: you must always have **written authorisation** before testing any system
- Without permission = illegal. With permission = ethical hacking
- Common offensive techniques practiced legally: scanning, exploitation, privilege escalation

**3. Careers in Cybersecurity**
- The cybersecurity field has many paths — not just "hacker"
- Key roles to know:

| Role | What They Do |
|------|-------------|
| Penetration Tester | Legally attacks systems to find weaknesses |
| SOC Analyst | Monitors alerts and responds to incidents |
| Security Analyst | Analyses threats and improves defences |
| Threat Intelligence Analyst | Researches attacker tactics and trends |
| Bug Bounty Hunter | Finds vulnerabilities in exchange for rewards |

**4. Offensive vs Defensive Security — The Core Difference**
- **Offensive** = thinking like an attacker (Red Team)
- **Defensive** = thinking like a defender (Blue Team)
- Both are equally important — you cannot defend what you don't understand how to attack
- Many professionals start defensive then move offensive, or vice versa

**5. Why Cybersecurity Matters Right Now**
- Every company that has an online presence has digital assets to protect
- Cybercrime is growing every year — demand for security professionals is very high
- Pakistan has a growing need for local cybersecurity talent — especially in banking, telecom, and fintech sectors

### Reflection
- **What clicked:** The difference between offensive and defensive roles — they work together, not against each other
- **What to revisit:** SOC analyst workflow in more detail

---

## Day 2 — Saturday, 21 March 2026
**Topics: Networking Devices | IP & MAC Addressing | Subnetting**

### Key Learnings

**1. Networking Devices and OSI Layers**
- **Hub** operates at Layer 1 (Physical) — sends data to all devices on the network, making sniffing trivial. A major security risk.
- **Switch** operates at Layer 2 (Data Link) — sends data only to the intended device using MAC addresses. More secure than a hub.
- **Router** operates at Layer 3 (Network) — directs traffic between different networks using IP addresses.
- **Firewall** — stateful vs stateless: stateful tracks the full connection, stateless only checks individual packets.
- **Modem** connects your network to the ISP. Router distributes that connection to your devices.

**2. MAC Flooding Attack**
- An attacker floods a switch with fake MAC addresses until its table overflows
- The switch then acts like a hub — broadcasting all traffic to all devices
- This allows an attacker to sniff traffic that was never meant for them

**3. IP Address vs MAC Address**
- **IP address** — logical, assigned by network, can change (e.g. 192.168.1.5)
- **MAC address** — physical, burned into the network card at the factory, permanent
- Private IP ranges to memorise: `192.168.x.x` / `10.x.x.x` / `172.16.x.x`
- **NAT (Network Address Translation)** allows many devices to share one public IP address

**4. Subnetting Basics**
- `/24` means 255 usable host addresses on the network
- `/16` means 65,535 usable host addresses
- **DHCP** automatically assigns IP addresses to devices on a network
- **DHCP Starvation Attack** — attacker sends thousands of DHCP requests to exhaust the available IP pool, causing a DoS condition

**5. Hands-on Commands in Kali**
```bash
ifconfig          # shows your IP, MAC address, subnet mask, interface name
```

### Reflection
- **What clicked:** Why hubs are dangerous — every packet is visible to every device
- **What to revisit:** DHCP starvation attack in more detail
- **Key port cheatsheet started:** 21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 80 HTTP, 443 HTTPS, 445 SMB, 3389 RDP, 3306 MySQL

---

## Day 3 — Sunday, 22 March 2026
**Topics: OSI Model | TCP/IP Deep Dive | Critical Ports**

### Key Learnings

**1. The 7 OSI Layers**

| Layer | Name | Protocol Examples | Attack Examples |
|-------|------|------------------|-----------------|
| 7 | Application | HTTP, FTP, DNS | SQLi, XSS |
| 6 | Presentation | SSL/TLS, JPEG | SSL stripping |
| 5 | Session | NetBIOS, RPC | Session hijacking |
| 4 | Transport | TCP, UDP | SYN flood |
| 3 | Network | IP, ICMP | IP spoofing |
| 2 | Data Link | Ethernet, MAC | MAC flooding |
| 1 | Physical | Cables, Wi-Fi | Physical tapping |

**2. TCP/IP Model (4 Layers)**
- Network Access → Internet → Transport → Application
- OSI has 7 layers (theoretical model); TCP/IP has 4 layers (practical implementation)

**3. Critical Ports to Memorise**

| Port | Service | Why Attackers Target It |
|------|---------|------------------------|
| 21 | FTP | Credentials sent in plaintext |
| 22 | SSH | Brute force attempts |
| 23 | Telnet | No encryption — plaintext passwords |
| 25 | SMTP | Email spoofing and spam |
| 53 | DNS | DNS poisoning and zone transfer |
| 80 | HTTP | Web attacks (XSS, SQLi) |
| 443 | HTTPS | SSL vulnerabilities |
| 445 | SMB | EternalBlue — used in WannaCry |
| 3389 | RDP | Remote Desktop brute force |
| 3306 | MySQL | Database access |

**4. Hands-on Commands in Kali**
```bash
ping google.com       # tests connectivity; ICMP operates at Layer 3
netstat -an           # shows all open ports and active connections on your machine
ifconfig              # shows network interface details
```

**5. Wireshark and Nmap Layer Reference**
- Wireshark captures from Layer 2 upward
- Nmap scans at Layer 3 and Layer 4

### Reflection
- **What clicked:** OSI layers finally made sense when connecting them to real attacks at each layer
- **What to revisit:** TCP three-way handshake (SYN, SYN-ACK, ACK) in more detail

---

## Day 4 — Monday, 23 March 2026
**Topics: Google Dorking | WHOIS | DNS Enumeration**

### Key Learnings

**1. Google Dorking**
- Google Dorking is a searching method that uses multiple filters to surface results that a simple query would never show. The results were always there — we just changed the method of searching.
- Key operators:

| Operator | Example | What It Does |
|----------|---------|-------------|
| `site:` | `site:gov.pk` | Limits results to one domain |
| `filetype:` | `filetype:pdf cybersecurity` | Finds specific file types |
| `intitle:` | `intitle:"admin login"` | Searches page titles |
| `inurl:` | `inurl:admin` | Searches URLs |
| `cache:` | `cache:example.com` | Shows Google's cached version |
| `"index of"` | `"index of"/backup` | Finds exposed directory listings |

- Most interesting finding: searching `"index of"/backup` returned actual backup files from real websites that were never meant to be public, including images and resources used on those sites. This is a real-world data exposure risk.

**2. WHOIS**
- WHOIS is a Linux command that reveals everything publicly registered about a domain
- Fields it typically returns:

| Field | What It Tells You |
|-------|------------------|
| Registrar Name | Who manages the domain |
| Registration Date | When the domain was created |
| Expiration Date | When the domain expires |
| DNS Server | Which DNS server handles it |
| Registrant Country | Country of the domain owner |

- Some fields may be hidden due to WHOIS privacy protection, but key infrastructure details almost always appear
- Before attacking or defending anything, you need to know who owns it, where it is hosted, and who manages it. WHOIS gives you that in seconds. It is always the first step in any OSINT or footprinting exercise.

**3. DNS Enumeration with `dig`**
- `dig` queries DNS records for a domain and returns structured results
```bash
dig google.com A        # Returns the IP address of the domain
dig google.com MX       # Returns mail servers (who handles their email)
dig google.com NS       # Returns nameservers (who manages their DNS)
dig google.com TXT      # Returns text records (SPF, verification keys)
dig google.com ANY      # Returns everything at once
```
- DNS Enumeration = aggressively querying all DNS records of a target to map their full infrastructure — subdomains, mail servers, nameservers, IP addresses
- This is a recon technique that happens before any active attack

**Key Takeaway**
- Google Dorking, WHOIS, and `dig` are all passive recon tools — no direct interaction with the target's systems, so they leave no trace on the target. This is why they are always the first steps in any penetration test or investigation.

### Reflection
- **What clicked:** `dig ANY` is a powerful one-command intelligence snapshot of a domain's full DNS footprint
- **What to revisit:** DNS zone transfers — what they are and why misconfigured DNS servers leak everything

---

## Day 5 — Tuesday, 24 March 2026
**Topics: Network Service Enumeration | OSINT Tools (theHarvester, Shodan, Maltego)**

### Key Learnings

**1. Scanning vs Enumeration — The Difference**
- **Scanning** = finding what ports are open on a target
- **Enumeration** = extracting detailed information from those open ports (usernames, services, versions, shares)
- Enumeration always comes after scanning — you cannot enumerate what you have not yet found

**2. Email Header Analysis**
- A raw email header reveals: sender's real IP address, mail servers the email passed through, timestamp at each hop, and signs of email spoofing
- Tool: **MXToolbox** (mxtoolbox.com) — free online tool to look up MX records for any domain without installing anything

**3. Subdomain Enumeration**
- Subdomains matter to attackers because they often run on older, less-maintained infrastructure
- Example: `dev.company.com` or `staging.company.com` may have weaker security than the main site
- Finding all subdomains = mapping the full attack surface

**4. NetBIOS Enumeration**
- NetBIOS can reveal: computer name, logged-in username, domain, and MAC address
- This is internal network information that should never be exposed externally

**5. SNMP Enumeration**
- SNMP (Simple Network Management Protocol) is used to manage network devices
- The **community string** functions like a password — many devices still use the default `public` community string
- Attackers can read device configurations, interface information, and routing tables via SNMP if it is misconfigured

**6. theHarvester**
- theHarvester is an OSINT tool built into Kali that collects emails, subdomains, and hostnames from public sources
```bash
theHarvester -d google.com -l 100 -b bing
# -d = target domain
# -l = number of results to retrieve
# -b = source to search (bing, google, linkedin, etc.)
```
- Output: list of email addresses and subdomains associated with the target domain

**7. Shodan**
- Shodan is a search engine for internet-connected devices — it indexes banners and metadata from servers, routers, webcams, industrial systems, and IoT devices
- Called "the hacker's Google" because it reveals exposed infrastructure that normal search engines ignore
- Useful filters:
```
apache country:PK          # Apache servers in Pakistan
port:22 country:PK         # SSH servers in Pakistan
port:3389                  # Exposed RDP servers worldwide
os:"Windows XP"            # Devices still running Windows XP
hostname:company.com       # All devices registered under a hostname
```
- Important: Shodan is read-only recon. You observe results — you do not interact with or connect to any listed device.

**8. Maltego**
- Maltego is an OSINT and link-analysis tool that visualises relationships between people, domains, IPs, organisations, and social profiles
- Uses **transforms** — automated queries that take one piece of data and find everything connected to it
- Example: input a domain → Maltego finds emails, subdomains, associated IPs, employees, social profiles
- The Community Edition is free and sufficient for learning and portfolio work
- Most useful for understanding how scattered pieces of information connect to form a complete picture of a target

### Reflection
- **What clicked:** The distinction between scanning and enumeration — they are separate phases with separate goals
- **What to revisit:** Maltego transforms in more detail — there are hundreds of them and knowing which to use takes practice

---

## Day 6 — Wednesday, 25 March 2026
**Topics: Advanced OSINT Tools | Dark Web Awareness | OSINT Frameworks**

### Key Learnings

**1. Shodan Advanced Filters**
```
port:22 country:PK        # SSH servers in Pakistan
port:80 country:PK        # Web servers in Pakistan
os:"Windows Server 2008"  # Devices on outdated OS
hostname:company.com      # Devices under a specific hostname
```
- IoT device exposure: cameras, printers, routers, and industrial control systems regularly appear in Shodan with default credentials still active
- Practical observation: searching Pakistani devices on Shodan reveals a significant number of exposed systems — which is why cybersecurity talent is in high demand locally

**2. Geolocation and IP Tracing**
- **ipinfo.io** — enter any IP address and get back: city, country, ISP, ASN (Autonomous System Number), and approximate coordinates
- Attackers use this to map out where a target's infrastructure is physically located and who hosts it

**3. Social Engineering OSINT**
- LinkedIn reveals: employee names, job titles, tech stack (from job postings), company size, internal tool names
- Facebook/Instagram reveals: personal routines, relationships, travel patterns — used for spear phishing
- Spear phishing = highly targeted phishing using personal details gathered through OSINT to make the attack believable

**4. Deep Web vs Dark Web**
| | Deep Web | Dark Web |
|--|----------|----------|
| **Definition** | Not indexed by search engines | Requires Tor browser to access |
| **Examples** | Email inboxes, bank portals, private databases | .onion sites |
| **Accessible via** | Normal browser with credentials | Tor browser only |
| **Legitimate use** | Yes — most of the internet is deep web | Some (journalism, privacy) |

**5. How Tor Anonymises Traffic**
- Tor routes your connection through 3 relays: Guard node → Middle node → Exit node
- Each relay only knows the previous and next hop — no single node knows both the source and destination
- Your real IP is never visible to the destination website

**6. OSINT Framework**
- osintframework.com — a comprehensive mind map of every OSINT tool categorised by type: people, usernames, email, domain, IP, social media, geolocation, and more
- Essential reference for any recon task — if you need to find something, this tells you which tool to use

**7. Threat Intelligence Feeds**
- **VirusTotal** (virustotal.com) — search any file hash, URL, or IP against 70+ security vendors to check if it is flagged as malicious
- **AbuseIPDB** — database of reported malicious IP addresses; search an IP to see its abuse history

**8. Legal Boundaries of OSINT in Pakistan**
- Legal: WHOIS lookups, `dig` queries, searching public social media, Google Dorking, reading Shodan results
- Potentially illegal: accessing systems without permission, even if Shodan or Google exposed the login page
- The data being publicly accessible does not mean accessing it is legal — always observe, never interact

### Reflection
- **What clicked:** Tor's three-relay system — understanding why no single node knows the full path makes the anonymity model very clear
- **What to revisit:** AbuseIPDB and VirusTotal — practice using them on real suspicious IPs and hashes for the portfolio

---

## Day 7 — Thursday, 26 March 2026
**Topics: Week 1 Review | CEH Assessment Tasks | Portfolio Consolidation**

### Key Learnings

**1. Full OSINT Methodology — End to End**
- This is the professional order of steps for any target investigation:

```
Step 1: WHOIS          → Who owns the domain? When registered? Where hosted?
Step 2: dig            → What DNS records exist? IP? Mail servers? Nameservers?
Step 3: theHarvester   → What emails and subdomains are publicly findable?
Step 4: Shodan         → Are any of their servers or devices exposed online?
Step 5: VirusTotal     → Is the domain or IP flagged in any threat intelligence feeds?
```

- Document every step and every finding — a professional report shows methodology, not just results

**2. CEH Assessment Tasks Completed**
- **Task 1:** Lab setup verification — confirmed Kali Linux boots cleanly, all tools functional (WHOIS, dig, theHarvester, Shodan, VirusTotal)
- **Task 2:** Full OSINT domain investigation — ran the complete methodology above on the trainer-assigned target and submitted a one-page professional report

**3. Professional Report Structure**
A clean OSINT report follows this format:
- **Background** — What the target is and why it was investigated
- **Findings** — What each tool revealed (registrant info, IP, hosting provider, subdomains, threat intel hits)
- **Conclusion** — Overall risk assessment based on findings

**4. Week 1 Tools Summary**

| Tool | Type | What It Does |
|------|------|-------------|
| WHOIS | Passive OSINT | Domain registration and ownership data |
| `dig` | Passive OSINT | DNS record enumeration |
| Google Dorks | Passive OSINT | Finds exposed files and pages via search engine |
| theHarvester | Passive OSINT | Collects emails and subdomains from public sources |
| Shodan | Passive OSINT | Searches for exposed internet-connected devices |
| Maltego | Passive OSINT | Link analysis and visual intelligence mapping |
| VirusTotal | Threat Intel | Checks files, URLs, IPs against 70+ security vendors |
| MXToolbox | Passive OSINT | Email and DNS record lookup |
| ipinfo.io | Passive OSINT | IP geolocation and ISP information |

**5. GitHub Portfolio — Week 1 Files Pushed**
- `README.md` — updated with Week 1 completed section
- `week1-notes.md` — all 7 days documented
- `ports-cheatsheet.md` — 10 critical ports with service names
- `osint-cheatsheet.md` — 10 Google Dorks with explanations
- `tools-used.md` — theHarvester, dig, WHOIS, Shodan documented
- `reports/week1-osint-report.md` — full OSINT investigation writeup

### Reflection
- **Biggest win this week:** Went from zero hands-on OSINT experience to running a complete professional-grade domain investigation using 5 tools in sequence — and documenting it as a portfolio piece
- **Key lesson:** The methodology matters more than any single tool. Knowing the order — WHOIS → dig → theHarvester → Shodan → VirusTotal — is what separates a professional from someone who just runs random commands

---

## Week 1 — Concepts Summary

**Core topics covered this week:**
- Offensive vs defensive security (Red Team vs Blue Team)
- OSI 7-layer model and where attacks happen at each layer
- TCP/IP 4-layer model
- Networking devices: Hub, Switch, Router, Firewall — layers and security implications
- IP vs MAC addressing, private IP ranges, NAT, subnetting basics
- DHCP and DHCP starvation attack
- Critical ports: 21, 22, 23, 25, 53, 80, 443, 445, 3389, 3306
- Google Dorking — 10 key operators
- WHOIS — domain registration intelligence
- DNS enumeration with `dig` — A, MX, NS, TXT, ANY records
- Scanning vs enumeration — the difference
- theHarvester — email and subdomain collection
- Shodan — internet device exposure search engine
- Maltego — link analysis and visual OSINT mapping
- Deep Web vs Dark Web, Tor anonymisation model
- OSINT Framework reference
- VirusTotal and AbuseIPDB for threat intelligence
- Legal boundaries of OSINT

---

*Week 1 complete. Week 2 begins 27 March 2026 — Nmap deep dive + first real network scan.*

*Updated daily. Part of the 2.5-month cybersecurity roadmap — COMSATS Lahore, 7th Semester CS.*