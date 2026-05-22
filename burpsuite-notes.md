# Burp Suite — Complete Notes
**Author:** Muhammad Noman (Nomi)  
**Institution:** COMSATS University Islamabad, Lahore Campus — 7th Semester CS  
**Purpose:** Practical Burp Suite reference for web application security testing  
**Portfolio:** [github.com/nomanshabbir03/cybersecurity-portfolio](https://github.com/nomanshabbir03/cybersecurity-portfolio)

---

## Table of Contents
1. [What is Burp Suite](#1-what-is-burp-suite)
2. [Dashboard](#2-dashboard)
3. [Target — Site Map and Scope](#3-target--site-map-and-scope)
4. [Proxy](#4-proxy)
5. [Repeater](#5-repeater)
6. [Intruder](#6-intruder)
7. [Sequencer](#7-sequencer)
8. [Decoder](#8-decoder)
9. [Comparer](#9-comparer)
10. [Logger](#10-logger)
11. [Extensions](#11-extensions)
12. [Key Practical Findings](#12-key-practical-findings)

---

## 1. What is Burp Suite

Burp Suite is an integrated platform for web application security testing developed by PortSwigger. It acts as a proxy between your browser and the target server, allowing you to intercept, read, modify, and replay HTTP/HTTPS traffic.

**Editions:**
- Community Edition — free, core tools available, no automated scanner
- Professional Edition — paid, includes automated scanner, advanced Intruder, and more
- Enterprise Edition — for large scale automated scanning

**Practice Targets Used:**
- `https://ginandjuice.shop` — PortSwigger's deliberately vulnerable demo app
- `https://portswigger.net/web-security/all-labs` — official Web Security Academy labs

---

## 2. Dashboard

The first screen you see when Burp opens. Contains three panels.

**Tasks Panel**  
Shows background jobs running in Burp. When you launch an automated scan it appears here as a task. You can pause, resume, or stop tasks from here. Mainly relevant in Professional Edition.

**Event Log**  
Logs every internal Burp action — proxy start, connection events, errors. First place to check when something is not working as expected.

**Issue Activity**  
Shows vulnerabilities detected by the automated scanner. Mostly empty in Community Edition. In Professional Edition this fills up with findings like XSS, SQLi, and information disclosure, each clickable with full details.

---

## 3. Target — Site Map and Scope

### Site Map

Burp automatically records every request passing through the proxy into the Site Map. You do not have to do anything manually — just browse the target and the map builds itself.

**Left panel** — domain tree showing all hosts and paths discovered  
**Right panel** — full request and response for whichever item you click

**What to look for in Site Map:**
- Requests with a tick in the Params column — these have parameters and are highest priority for testing
- POST requests — carry submitted data like login credentials
- API endpoints — usually return JSON and interact directly with the database
- Third party domains — these load automatically from the target's code (Google fonts, Facebook widgets, analytics) and appear in Site Map even though you never visited them directly

**Filter bar** — use it to show only parameterized requests, only in-scope items, or filter by response code or file type.

**Right-click any request** → Send to Repeater, Send to Intruder, or other tools directly from Site Map.

### Scope

Scope defines which targets Burp should focus on. Without scope, Burp records traffic from every domain your browser touches including third party noise.

**How to add scope:**  
Right-click a domain in Site Map → Add to scope  
Or go to Target → Scope Settings → manually add a URL

**Simple vs Advanced mode:**  
Simple mode uses URL matching. Advanced mode uses regex and is more flexible. If the Remove button does not work in Simple mode, switch to Advanced mode — the bug does not occur there.

**Best practice:** Always set scope before starting a test. Then enable "Show only in-scope items" in the Site Map filter. This keeps your workspace clean and prevents accidentally testing systems outside your authorized scope.

---

## 4. Proxy

The most important tab in Burp Suite. Sits between your browser and the server, capturing all traffic.

### Intercept

Main panel with an on/off toggle.

**Intercept ON** — every request freezes here waiting for your action:
- Forward — send the request to the server as-is or with your modifications
- Drop — discard the request entirely, browser receives nothing
- Action — send to Repeater, Intruder, or other tools

**Intercept OFF** — requests flow through automatically. Burp still logs everything in HTTP History but does not freeze anything.

**Practical tip:** Keep intercept OFF while browsing normally. Turn it ON only when you want to catch and modify a specific request.

### HTTP History

Every request and response passing through Burp is logged here automatically, even with intercept off.

Each row shows: Method, URL, Status Code, Response Length, Content Type, and whether parameters are present.

**How to use it:**
1. Browse the target with intercept OFF
2. Come to HTTP History
3. Look for POST requests and requests with parameters — these are your testing targets
4. Right-click anything interesting → Send to Repeater

**Filter bar** — filter by in-scope only, parameterized requests, response codes, file types. Essential for cutting through noise.

### WebSockets History

Captures WebSocket traffic separately from regular HTTP. Relevant for modern applications using real-time features like chat or live updates.

### Proxy Settings

- **Listener** — default is 127.0.0.1:8080. Burp listens on this port for browser traffic.
- **Built-in browser** — pre-configured, no manual setup needed.
- **Manual browser setup** — point Firefox or Chrome proxy settings to 127.0.0.1 port 8080.
- **Response interception** — disabled by default. Enable it to intercept and modify server responses before they reach your browser. Useful for removing security headers or modifying page content.

---

## 5. Repeater

Manual request modification and resending tool. The core tool for manual vulnerability testing.

**Core workflow:** Modify request → Send → Read response → Repeat

### Interface

- **Left panel** — your request, fully editable
- **Right panel** — server response after you click Send
- **Send button** — fires the current request
- **Back/Forward arrows** — navigate through your request history within the tab. Every send is saved so you can go back to any previous version.
- **Tab renaming** — double-click a Repeater tab to rename it. Essential when you have 10+ tabs open during a test.

### Response Panel Views

- **Pretty** — formatted, readable HTML or JSON. Use this most of the time.
- **Raw** — complete unformatted response including all headers exactly as the server sent it.
- **Render** — renders HTML visually like a browser. Useful after modifying responses.
- **Hex** — hexadecimal view for binary data analysis.

### Practical Testing Pattern

**Parameter manipulation example on ginandjuice.shop:**

| Request | Response | Finding |
|---------|----------|---------|
| `productId=5` | 200 OK — product page | Normal behavior baseline |
| `productId=1` | 404 Not Found — custom error page | Product does not exist, app handles gracefully |
| `productId=999` | 400 Bad Request — `"Invalid product ID"` (JSON) | API behind frontend doing validation, returns JSON errors |
| `productId=5'` | 400 Bad Request — `"Invalid product ID"` | Input sanitization present, special characters rejected |

**Key finding:** The `productId` parameter has input validation. The 400 JSON response on invalid values reveals an underlying API architecture. The rejection of the single quote means basic SQLi is blocked on this endpoint.

### What to test in Repeater

- Change parameter values to probe database behavior
- Add a single quote `'` to parameters to test for SQL injection
- Change User-Agent header to bypass WAF filters or test bot detection
- Modify cookie values to test session handling and authorization
- Remove headers to test what the application requires vs assumes

---

## 6. Intruder

Automated payload injection tool. Used for fuzzing parameters with many different values automatically.

### Attack Types

**Sniper** — one payload position, one wordlist. Tests each payload in the list one at a time against a single parameter. Most common attack type. Use for username enumeration, password brute force, or parameter fuzzing.

**Battering Ram** — one wordlist, multiple positions. Inserts the same payload into all marked positions simultaneously. Use when you want the same value in multiple fields at once.

**Pitchfork** — multiple wordlists, multiple positions. Uses wordlist 1 for position 1 and wordlist 2 for position 2 in parallel. Use for credential stuffing where you have matching username and password pairs.

**Cluster Bomb** — multiple wordlists, multiple positions. Tests every combination of every payload across all positions. Use for brute force attacks where you do not have matched pairs.

### Positions Tab

After sending a request to Intruder, go to the Positions tab. Burp auto-marks parameters with `§` symbols. You can clear all markers and manually add your own by highlighting any value and clicking Add.

### Payloads Tab

Where you define what values to inject. Payload types include:
- Simple list — paste or load a wordlist
- Numbers — generate a numeric range automatically
- Brute forcer — generate character combinations
- Runtime file — load from a file during the attack

**Important:** In Community Edition, Intruder is rate-limited and runs slowly. This is intentional. Professional Edition removes the rate limit.

### Results Analysis

Sort results by Status Code or Response Length. Responses that differ from the baseline are your signals. A different status code or a noticeably different response length on one particular payload means something interesting happened with that value.

---

## 7. Sequencer

Analyzes the randomness and quality of tokens — session cookies, CSRF tokens, password reset tokens, and any other values that need to be unpredictable.

**Why it matters:** If session tokens are predictable or follow a pattern, an attacker can forge valid tokens without stealing them.

**How to use:**
1. Find a request that generates a token (login response, page with CSRF token)
2. Send to Sequencer
3. Click Start live capture — Burp sends the request hundreds of times and collects each token
4. After collection, click Analyze to get an entropy report

**Reading results:** The analysis shows bits of effective entropy. Anything above 100 bits is generally considered secure. Low entropy means the token is weak and potentially predictable.

---

## 8. Decoder

Encodes and decodes data in multiple formats. Essential for analyzing and manipulating encoded values found in requests and responses.

### Supported Formats
- Base64 encode/decode
- URL encode/decode
- HTML encode/decode
- Hex encode/decode
- ASCII Hex
- Gzip
- Hashing — MD5, SHA1, SHA256, SHA512

### Practical Use Cases

**Analyzing cookies:** Cookies with values ending in `==` are typically Base64 encoded. Paste the value into Decoder and decode from Base64 to read the original data.

**Modifying tokens:** Decode a token, change the value, re-encode it, paste it back into your request in Repeater. This is the workflow for cookie manipulation attacks and JWT testing.

**URL encoding payloads:** When injecting payloads that contain special characters, URL encode them first so they pass through the request without breaking the syntax.

**Chain encoding:** Decoder lets you chain multiple operations. For example decode Base64 first and then URL decode the result. This handles doubly-encoded values which are common in complex applications.

---

## 9. Comparer

Compares two pieces of text or binary data and highlights the differences.

### When to use it

**Response comparison:** Send the same request with two different parameter values to Repeater. Copy both responses and compare them in Comparer. If there is a vulnerability, the two responses will differ in meaningful ways that Comparer highlights clearly.

**Blind SQL injection:** When the application does not return obvious errors, compare responses for a true condition versus a false condition. Even subtle differences in length or content confirm the injection is working.

**Authorization testing:** Compare the response you get as a normal user versus what you get as an admin. Differences reveal what data or functionality is being hidden.

**How to send data:** Right-click any request or response in Repeater or HTTP History and click Send to Comparer. Or paste text directly into the Comparer panels.

---

## 10. Logger

Records all HTTP traffic passing through Burp in one unified view, across all tools, not just the Proxy.

**Difference from HTTP History:** HTTP History only shows Proxy traffic. Logger shows traffic from every Burp tool including Repeater sends, Intruder requests, Scanner requests, and everything else.

**Use cases:**
- Debugging — find out exactly what request a tool sent when something unexpected happened
- Audit trail — keep a complete record of all requests made during a test session
- Finding missed requests — sometimes a tool makes background requests that do not appear in HTTP History but do appear in Logger

**Filter and search:** Logger has filter and search functionality similar to HTTP History. Filter by tool, method, status code, or search by keyword in URLs or responses.

---

## 11. Extensions

Burp's extension system allows you to add extra functionality beyond what ships with Burp by default. Extensions are installed through the BApp Store inside the Extensions tab.

### BApp Store

Built-in marketplace for Burp extensions. Community Edition can install many free extensions. Some advanced extensions require Professional Edition.

### Essential Extensions to Know

**Active Scan++** — adds additional scan checks on top of Burp's built-in scanner. Finds vulnerabilities the default scanner misses.

**Autorize** — automatically tests for broken access control and IDOR vulnerabilities by replaying requests with a lower-privileged session token.

**JWT Editor** — decode, modify, and re-sign JSON Web Tokens directly inside Burp. Essential for API security testing.

**Turbo Intruder** — replaces the rate-limited Community Edition Intruder with a high-speed Python-scriptable fuzzer. Free and extremely powerful.

**Logger++** — enhanced version of the built-in Logger with better filtering, searching, and export options.

**Param Miner** — discovers hidden parameters in requests that are not visible in the UI. Useful for finding undocumented API parameters.

### Installing an Extension
Go to Extensions tab → BApp Store → search for the extension → click Install. Burp handles the rest. Most extensions are written in Java or Python.

---

## 12. Key Practical Findings

Findings from hands-on practice sessions documented for portfolio reference.

### Target: ginandjuice.shop

**Finding 1 — API Architecture Identified**  
The product catalog endpoint `/catalog/product?productId=` returns JSON error messages on invalid input, revealing that an API layer sits behind the HTML frontend. This is valuable reconnaissance that informs further testing strategy.

**Finding 2 — Input Validation Present on productId**  
Injecting a single quote (`productId=5'`) returned a 400 Bad Request with `"Invalid product ID"`. The application sanitizes special characters on this parameter. Basic SQL injection is blocked here. Deeper testing with encoding bypass techniques would be the next step.

**Finding 3 — AWS Infrastructure Identified**  
The presence of AWSALB and AWSALBCORS cookies confirms the application runs behind an AWS Application Load Balancer. This is useful infrastructure intelligence.

**Finding 4 — Encoded TrackingId Cookie**  
The `TrackingId` cookie contains a Base64 encoded value. Encoded cookies warrant investigation as they sometimes contain exploitable data like user IDs, roles, or serialized objects.

---

## Quick Reference — HTTP Status Codes

| Code | Meaning | Security Relevance |
|------|---------|-------------------|
| 200 | OK | Normal response — baseline behavior |
| 301/302 | Redirect | May reveal internal paths |
| 400 | Bad Request | Input rejected — may indicate validation |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Authenticated but access denied — test for bypass |
| 404 | Not Found | Resource missing |
| 500 | Internal Server Error | Application crash — may indicate injection vulnerability |

---

## Quick Reference — Burp Workflow for a New Target

1. Set scope in Target → Scope Settings
2. Browse target with Proxy intercept OFF
3. Review HTTP History — filter for parameterized requests
4. Find interesting endpoints in Site Map
5. Send interesting requests to Repeater
6. Manually probe parameters in Repeater — change values, add quotes, modify headers
7. Use Intruder for automated fuzzing when manual probing finds something worth expanding
8. Use Decoder to analyze encoded values found in cookies and tokens
9. Use Comparer to spot differences between responses
10. Document all findings with request/response evidence

---

*Notes maintained as part of the cybersecurity-portfolio GitHub repository.*  
*Last updated: May 2026*
