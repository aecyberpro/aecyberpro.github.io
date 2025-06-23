---
layout: page
title: Welcome!
sitemap: true
---
## Whoami
I'm Steve Campbell, also known as @lpha3ch0. Check out my [blog](https://www.aecyberpro.com/blog/) while you're here.

I'm a retired U.S. Navy veteran where I worked as an Aviation Electrician (AE ~ @lpha3ch0) before switching to Information Technology. I have more than 20 years of combined IT and penetration testing experience. Before changing my focus to penetration testing, I worked in various IT and security engineering roles. This experience adds context to my pentest report deliverables and remediation recommendations.

I'm also a father and husband. In my spare time I enjoy learning new things related to hacking and taking my wife on motorcycle rides.

I'm the author of the book "Bash Shell Scripting for Pentesters". Part 1 focuses on the basics, building a strong foundation. Part 2 covers using Bash scripting for automation in penetration testing scenarios. Part 3 covers advanced topics, including evasion and obfuscation, interfacing with AI in pentest workflows, and DevSecOps using Bash. Buy it on [Amazon](https://a.co/d/cZhrRqA)
 or [Packt](https://www.packtpub.com/en-us/product/bash-shell-scripting-for-pentesters-9781835880838) 

## Skills

- Pentesting:
    - Internal, External, and WiFi network pentesting
    - Web, API, and Mobile application pentesting
    - Secure code review
    - Active Directory enumeration and exploitation
    - Thick client testing
    - Limited IoT pentesting experience
    - Physical security testing
- Programming: Bash, Python, Go, Ruby, Nim, C#, PowerShell
- Networking: I was a former CCNA cert holder (expired) and have a good understanding of networking and network technologies.

## Experience

I currently work as a Technical Lead, AppSec Lead, and pentester on an offensive security consulting team. I have planned, scoped, lead, and performed penetration testing engagements on various major enterprises, such as: Fortune 50, government institutions, banking, finance, healthcare and insurance, ecommerce, legal, and energy sector clients. These engagements provided deep insights to the organization on their weak points, how they can be connected to a full attack vector, how they can be monitored, detected, quarantined, and remediated.

I share my experience and insight through training and mentoring consultants, creating methodology documentation, and ensuring the quality of report deliverables.

## Public Speaking

- "Red Team Rising: Breaking Into Offensive Security" - CyberForge 2025
- “Cross-Site Scripting – Beyond Alert()” – DefCon 401, 09/2019
- “Introduction to the OWASP Top Ten and Web App Pentesting”, ECPI University alumnus presentation to Cyber Security students, 08/2016

## Published Vulnerabilities

- (No CVE), Account Takeover in Danphe Health Hospital Management System EMR due to Broken Object Level Authorization in password reset, https://github.com/hospital-management-system-emr/hospital-management-system-emr-opensource/issues/12
- CVE-2023-33254, LDAP bind credentials exposure on KACE Systems Deployment and Remote Site appliances
- CVE-2022-23436, Multiple Stored Cross-Site Scripting (XSS) vulnerabilities in Nakivo Backup and Replication
- CVE-2020-9330, LDAP bind credential exposure in Xerox WorkCentre printers
- CVE-2019-5648, LDAP bind credential exposure in Barracuda Email Security Gateway and Web Application Firewall
- CVE-2018-5550, Epson AirPrint XSS
- CVE-2016-10108, Unauthenticated Remote Code Execution as root in Western Digital MyCloud
- CVE-2016-10107, Unauthenticated Remote Code Execution as root in Western Digital MyCloud

## Published Software

- I've published tools/scripts in Python, Bash, Go, Ruby, Nim, and C#. I know enough C to read and modify source code, but not write it from scratch.
- Metasploit and Nuclei contributor
- Created private pentest team tools in C# for loading shellcode and .Net assemblies into memory and bypass detection.
- My project highlights:
    - [san-resolver](https://github.com/sdcampbell/san-resolver): Go; Uses input from tlsx to uncover CDN origin hosts and web applications that don't resolve in DNS from TLS SAN certificates.
    - [xfil](https://pypi.org/project/xfil/): Python; Blind web app XPath data exfiltration tool.
    - [dnsrecon](https://github.com/sdcampbell/dnsrecon): Bash; This isn't your typical subdomain enumeration tool. This tool has the ability to expand your reach by finding subdomains owned by a common organization, then dive down into the details to find those that are both live and match to your scope.
    - [Content Security Policy Generator](https://github.com/sdcampbell/Content-Security-Policy-Generator): Python; A Python script that analyzes a web page and generates a Content Security Policy (CSP) header with an optional reporting mechanism. It's designed to help website administrators create an initial CSP for their site.
    - [Bash for Pentesters](https://github.com/sdcampbell/bash_for_pentesters): Bash; My most frequently used Bash code snips for common pentest data parsing.
    - [Goscan](https://github.com/sdcampbell/goscan): Go; Single file TCP port scanner for use in scanning from compromised pivot hosts
    - [Nessusploitable](https://github.com/sdcampbell/Nessusploitable): Ruby; Script which parses .nessus files for exploitable vulnerabilities and outputs a report to stdout or file in TSV format for import to Excel
    - [Nmapurls](https://github.com/sdcampbell/nmapurls): Go; Tool which parses Nmap xml reports from either piped input or command line arg and outputs a list of http(s) URL's to be used in an automation pipeline
    - [scope-resolver](https://github.com/sdcampbell/scope-resolver): Go; Expands a pentest scope by taking an input list which includes IP addresses and parses DNS and TLS SAN hostnames and merges the results with the original scope list
    - [phpLFI](https://github.com/sdcampbell/phpLFI): Nim; Tool which tests for LFI in PHP apps and automates the process of abusing LFI's to download source code and discover new files via includes and recursively download additional source code files
    - [Bash TUI menu example](https://github.com/sdcampbell/Bash-TUI-Example): Bash; Easily build interactive shell command menus with fuzzy search so you can avoid having to memorize many complex commands.
- Contributions to public repositories:
    - [m0bilesecurity/RMS-Runtime-Mobile-Security](https://github.com/m0bilesecurity/RMS-Runtime-Mobile-Security/commit/4bb9e1be580c85e41b08c7f90f480f8e4daefc31)
    - [moohax/pypipal](https://github.com/sdcampbell/pypipal)
    - [projectdiscovery/nuclei-templates](https://github.com/projectdiscovery/nuclei-templates/blob/main/http/technologies/cisco-asa-detect.yaml)
    - [rapid7/metasploit-framework](https://github.com/rapid7/metasploit-framework/pull/18077)

## Education

Bachelor of Science in Computer and Information Science, ECPI University, 2009.
- Graduated Magna Cum Laude while simultaneously working overtime on a full time job, carrying a full-time courseload, and working a part-time side business.
- Sacrificed a lot of sleep!

## Certifications

- SANS GIAC Exploit Researcher and Advanced Penetration Tester (GXPN)
- Zero-Point Security Certified Red Team Operator (CRTO)
- Offensive Security Certified Professional (OSCP)
- Offensive Security Certified Wireless Professional (OSWP)
- Security+
- Cisco CCNA (expired)
- (insert more old IT certs here)