# Week 1 — Study Notes
**CEH Class + Personal Study Track**
📅 20 March 2026 → 26 March 2026

---

## Day 1 — Friday, 20 March 2026
**Topics: Intro to Offensive & Defensive Security | Careers in Cyber**

### 🔑 Key Learnings

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

| Role | What they do |
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

---

### 💡 Today's Reflection
- **What clicked:** The difference between offensive and defensive roles — they work together, not against each other
- **What to revisit:** SOC analyst workflow in more detail
- **TryHackMe rooms completed:** Intro to Offensive Security · Intro to Defensive Security · Careers in Cyber

---

## Day 2 — Saturday, 21 March 2026
*(Notes to be added after class)*

---

## Day 3 — Sunday, 22 March 2026
*(Notes to be added after class)*

---

## Day 4 — Monday, 23 March 2026
## Day 4 — Monday 23 March 2026
### Topic: Google Dorking + WHOIS + DNS Enumeration

**Google Dorking**
- Google Dorking is a searching method which use multiple filter to give you those answer which we usually not get with simple query - In simple words results were always there, we just change the method to search them. Here are few filters that we normally use: 
site: .edu.pk -- intitle:"admin password" -- inurl:admin
- Here are few examples:
    1. site: gov.pk
    2. filetype: pdf cyber security
    3. "index of"/backup

- by applying different filter, different results I found but the most interesting and shocking thing was that when I search for "index of"/backup and it actually provide me backup files of few websites which were not supposed to expose but they were and there were some resources also available like images that they may use on website

**WHOIS**
- WHOIS is a linux command which tell everything about domain, few things might not be shown due to privacy concers but registration date, expiry date, registrar mail or address all these are available easily
- Usually it reveals all these five fields:
    1. Registrar Name: Who manage the domain
    2. Registration Date: when this domain was ceated
    3. Expiration Date: when this will be expired
    4. DNS Server: which DNS Server handles it
    5. Registrant Country: Owner's country, he belongs to
- Before attacking or defending anything, you need to know who owns it, where it is hosted, and who manages it. WHOIS gives you that in seconds. It is always the first step in any OSINT or footprinting exercise — you are building a profile of the target before doing anything else.

**DNS Enumeration**
- dig provide us the ip address of any domain
-   dig google.com A        # IP address of the domain
    dig google.com MX       # mail servers (who handles their email)
    dig google.com NS       # nameservers (who manages their DNS)
    dig google.com TXT      # text records (SPF, verification keys)
    dig google.com ANY      # everything at once
- DNS Enumeration is a specific recon technique where you extract as much DNS information from a target as possible — all their subdomains, mail servers, nameservers, IP addresses — by querying their DNS records aggressively.

**Key takeaway today:**
- Today's i have learned a lot of things, Google Dorking, WhOIS and dig, all of them are important especially dig because it helps a lot to collect as much possible information from DNS as you can and that's the very basic step recon

---

## Day 5 — Tuesday, 24 March 2026
*(Notes to be added after class)*

---

## Day 6 — Wednesday, 25 March 2026
*(Notes to be added after class)*

---

## Day 7 — Thursday, 26 March 2026
*(Notes to be added after class)*

---

*Updated daily. Part of the 2.5-month cybersecurity roadmap.*