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
And all of this because bing has blocked scraping and to fix 
that I need to use an API which I don't have for now

**Why attackers use this:**
It reveals employee emails for phishing, subdomains for 
finding less-secured entry points, and IPs for further 
scanning — all without directly touching the target.

**Ethical boundary:**
Legal to use on domains you own or have written permission 
to test. Using it against targets without permission is 
illegal under Pakistan's PECA 2016 and internationally 
under computer misuse laws.

---

## Docker

**What it is:**
Docker is a containerization tool that lets you run isolated 
environments on your machine without dealing with manual 
installation and dependency conflicts.

**What it does:**
Instead of spending 30-40 minutes configuring Apache, PHP, 
and MySQL by hand every time, you run a container and 
everything is ready in seconds. Each container is isolated, 
clean, and disposable.

**Basic commands I used:**
docker pull [image]        — download an image from Docker Hub
docker run [image]         — create and start a container
docker ps                  — list all running containers
docker stop [containerID]  — stop a running container
docker rm [containerID]    — delete a stopped container

**Practical I did:**
Cloned DVWA (Damn Vulnerable Web Application) from GitHub 
and ran it inside Docker using its compose.yml file with:

sudo docker compose up -d

One command and the entire vulnerable web app was live on 
localhost — ready to practice SQL injection, XSS, and other 
web attacks against it.

**Manual Dockerfile approach:**
For tools that don't come with a Dockerfile, you write one 
yourself based on what the tool needs:

FROM php:7.4-apache
COPY . /var/www/html/
EXPOSE 80
CMD ["apache2-foreground"]

Then build and run it:
docker build -t mytool .
docker run -p 80:80 mytool

**Important note:**
Docker only runs the environment locally. To share it 
externally, you need Ngrok on top of it to tunnel the 
localhost port to a public URL.

**Why it matters for security work:**
Spin up vulnerable apps like DVWA or Juice Shop in seconds, 
practice attacks in a clean isolated environment, and tear 
it all down just as fast. No leftover configuration, 
no broken installs.

**Ethical boundary:**
All practice done inside isolated local containers on my 
own machine. No interaction with any external or live systems.