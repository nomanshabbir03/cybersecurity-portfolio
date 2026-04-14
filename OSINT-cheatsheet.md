# OSINT & Google Dorking Cheatsheet

## Google Dork Operators

| Operator | Example | What it finds |
|----------|---------|---------------|
| site: | site:gov.pk |I found all the website related to Government of Pakistan -- all their departments |
| filetype: | filetype:pdf cybersecurity |Notes for cyber security but in pdf format |
| intitle: | intitle:"login page" |log in pages of different websites which normally wasn't showing |
| inurl: | inurl:admin |url with admin text in them -- mostly admin auth |
| cache: | cache:bbc.com |i found nothing which means that possibly there'll be no cache |
| intext: | intext:"confidential" |different results with confidential word in them, mostly the results from dictionary |
| "index of" | "index of" /backup |different backfile files of website with some resources also which weren;t supposed to available |
| combined | filetype:xls site:gov.pk |xls files from different departmental websies of government |

## Notes
- All dorking is passive recon — no direct target interaction
- Read and observe only — never access exposed systems