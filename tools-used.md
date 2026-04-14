# Tools Used — Cybersecurity Portfolio

## theHarvester

**What it is:**
theHarvester is a passive OSINT tool built into Kali Linux used 
during the reconnaissance phase of a penetration test.

**What it does:**
It automatically collects emails, subdomains, hosts, and IP 
addresses associated with a target domain by querying public 
sources like Bing, Google, and others.

**Basic syntax:**
theHarvester -d [domain] -l [limit] -b [source]

**Example command I ran:**
theHarvester -d google.com -l 100 -b bing
theHarvester -d comsats.edu.pk -l 100 -b bing

**Output I got:**
- Emails found: No Email found
- Subdomains found: No subdomains found
- IPs found: No IPs found
And all of this because bing ahs blocked scrapping and to that i need to use an API which I don't have for now

**Why attackers use this:**
It reveals employee emails for phishing, subdomains for 
finding less-secured entry points, and IPs for further 
scanning — all without directly touching the target.

**Ethical boundary:**
Legal to use on domains you own or have written permission 
to test. Using it against targets without permission is 
illegal under Pakistan's PECA 2016 and internationally 
under computer misuse laws.