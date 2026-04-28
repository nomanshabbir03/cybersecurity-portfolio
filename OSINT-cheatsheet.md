# Google Dorking Reference | COMSATS Lahore | Cybersecurity Portfolio

## Section 1 — Core Operators

### 1. site:
Restricts results to a specific domain or website only.  
Example: `site:facebook.com`

### 2. filetype:
Filters results by a specific file extension.  
Example: `filetype:sql`

### 3. intitle:
Returns pages where the term appears in the title.  
Example: `intitle:login`

### 4. inurl:
Returns pages where the term appears in the URL.  
Example: `inurl:admin`

### 5. intext:
Returns pages where the term appears in the content.  
Example: `intext:password`

### 6. cache:
Shows Google’s cached version.  
Example: `cache:example.com`

### 7. link:
Finds pages linking to a URL.  
Example: `link:example.com`

### 8. related:
Finds similar websites.  
Example: `related:nytimes.com`

### 9. info:
Shows info about a URL.  
Example: `info:example.com`

### 10. allintext / allintitle / allinurl:
Require all words to appear.  
Example: `allintitle:admin login`

---

## Section 2 — Combination Dorks

- `site:gov.pk filetype:pdf`
- `intitle:index.of passwords`
- `filetype:xls intext:username intext:password`
- `site:pk inurl:admin intitle:login`
- `filetype:sql intext:INSERT INTO`
- `inurl:wp-config.php filetype:php`
- `intext:"Warning: mysql_fetch" OR intext:"Warning: mysqli_fetch"`
- `site:edu.pk filetype:xls intext:email`
- `intitle:"Elasticsearch" inurl:9200`
- `site:targetcompany.com -www filetype:log`

---

## Section 3 — OSINT Workflow (6 Stages)

1. `site:targetcompany.com`
2. `site:targetcompany.com -www`
3. `site:targetcompany.com filetype:pdf OR filetype:doc OR filetype:xls`
4. `site:targetcompany.com inurl:login OR inurl:admin`
5. `site:targetcompany.com inurl:wp-admin`
6. `site:targetcompany.com filetype:log OR filetype:bak OR filetype:sql`

---

## Section 4 — GHDB

Google Hacking Database:  
https://www.exploit-db.com/google-hacking-database

---

## Section 5 — Legal Boundaries

- Recon = legal  
- Access without permission = illegal  
- Follow PECA 2016 in Pakistan
