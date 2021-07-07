# Web Application Pentesting Checklist
Secuna Software Technologies, Inc.
<nathan.golez@secuna.io>
July 6, 2021
### Preparation
#### Planning and Scoping
- [ ] Time of Engagement
- [ ] Wildards
- [ ] Subnets
- [ ] Web and/or mobile applications
#### Tools
- [ ] Burp Suite Professional
- [ ] FoxyProxy
- [ ] Any JS Deobfuscator
- [ ] [SSL/TLS](https://github.com/drwetter/testssl.sh)
- [ ] gobuster/ffuf
- [ ] nmap scripts (only on http)
### Reconnaisance
#### Passive Recon
- [ ] Google Dorks
- [ ] theHarvester
- [ ] Employee  / Website Breach (haveibeenpwned, raidforums, leakedsource)
- [ ] Shodan.io
- [ ] Get subdomains from CRT.SH or tools.secuna.io
- [ ] LDAP info in SSL/TLS certificate
- [ ] SPF Checklist
- [ ] DMARC Checklist
- [ ] WHOIS records
- [ ] Wayback Machine
- [ ] Public Github repositories
	- [ ] Check comments on source code
	- [ ] Check issues on repositories
	- [ ] Check for API keys
- [ ] Facebook, Instagram, Twitter Accounts
- [ ] Whitepaper/Documentation
#### Active Recon and Analysis
- [ ] WhatWeb (Apache, nginx, PHP, etc...)
- [ ] What CDN/WAF is target using (Cloudflare, AWS, Fastly, etc...)
- [ ] Test for [SSL/TLS](https://github.com/drwetter/testssl.sh)
- [ ] common directories
	- [ ] robots.txt
	- [ ] sitemap.xml
	- [ ] crossdomain.xml
	- [ ] clientaccesspolicy.xml
	- [ ] /.well-known/
	- [ ] /server-status/
- [ ] .git folder
- [ ] Burp Spider
- [ ] Burp Scanner
- [ ] Find endpoints in javascript files (grep for HTTP methods)
- [ ] Check for version numbers in the website
- [ ] Sublist3r / ffuf
- [ ] nmap scripts (only for web servers)
- [ ] Best Practices
### Test cases in common features
#### Authentication
##### Register
- [ ] Username Policy
	- [ ] Minimum of 4 characters
	- [ ] Maximum of 32-64 characters
	- [ ] Special Characters (exclude \<,\>,\[,\],\",\'\{,\} because of XSS issues)
	- [ ] Username Uniqueness
- [ ] Password Policy
	- [ ] Minimum of 9 characters
	- [ ] Special Characters (96 character combo)
	- [ ] Does the application know your old password (if tried again)
- [ ] Email Policy
	- [ ] does the application accept the (+) character
		- username@secuna.io
		- username+1@secuna.io
	- [ ] whitelist/blacklist domain
		- gmail only, corp email only, etc..
- [ ] Check for XSS (single quotes) in input fields
- [ ] SQL Injection (sqlmap)
- [ ] HTTP Method when sending credentials (GET or POST)
- [ ] Rate Limiting
	- [ ] Burp Intruder for **username+\*@secuna.io**
- [ ] Prensence of CAPTCHA
	- [ ] CAPTCHA provider?
	- [ ] Check if bypassable by OCR
- [ ] Check with existing account
- [ ] Best Practices
##### Login
- [ ] Default passwords
- [ ] Enumeration
	- [ ] Username doesn't exist
	- [ ] Password is incorect
	- [ ] Email does not exist
- [ ] Rate Limiting
	- [ ] Burp Intruder for **username+\*@secuna.io**
- [ ] HTTP Method when sending username/password (GET or POST)
- [ ] Account Policy
	- [ ] How many requests until user gets locked out?
- [ ] Check for XSS in input fields
- [ ] SQL Injection (sqlmap)
- [ ] Best Practices
##### Forgot Password
- [ ] Enumeration
	- [ ] Username doesn't exist
	- [ ] Email does not exist
- [ ] Rate Limiting
	- [ ] Burp Intruder for **username+\*@secuna.io** forgot password
- [ ] HTTP Methold when sending username/password (GET or POST)
- [ ] Account Policy
	- [ ] How many requests until user gets locked out?
- [ ] Can reset token be used again?
- [ ] Check for Parameter pollution
- [ ] Best practices
##### Change Credentials
- [ ] check if you can change whitelist/blacklisted domain
- [ ] does application require to input current password when changing?
- [ ] does application require to input OTP when changing?
- [ ] Does the application know your old password (if tried again)
- [ ] What happens if you input null instead of a value?
- [ ] does the application accept the (+) character when changing?
- [ ] Check for Parameter pollution
- [ ] Best Practices
##### Logout
- [ ] Check cookie if it's still usable after logout
- [ ] Best Practices
#### Authorization
- [ ] Check for privilege escalation
	- AutoRepeater
	- [ ] Horizontal privesc
	- [ ] Vertical privesc
- [ ] What happens if user has no activity for 15 minutes? (OWASP compliance)
- [ ] Best Practices
#### Accounting
- [ ] Does the application log unauthenticated requests
- [ ] Does the application log authenticated requests
- [ ] Does the application log authorized requests
- [ ] Does the application log unauthorized requests
- [ ] Check for verbosity of logs
- [ ] Check for ban bypass
	- [ ] change something you are (either username, MAC Address, user agent)
	- [ ] change where you are (either IP, Location, ASN)
- [ ] Best Practices
#### Parameter Testing
- [ ] Local File Inclusion
- [ ] SSRF
- [ ] Open Redirect
- [ ] SQLi Testing
- [ ] Null (Byte) Testing
- [ ] CLRF Testing
- [ ] XSS Testing
- [ ] HTML tag injection testing
- [ ] SSTI Testing
- [ ] `;` testing 
- [ ] Remote Code Execution commands
- [ ] IDOR Testing
#### Best Practices
- [ ] X-XSS-Protection
- [ ] Strict-Transport-Security
- [ ] Content-Security-Policy
- [ ] Public-Key-Pins
- [ ] X-Frame-Options
- [ ] X-Content-Type-Options
- [ ] Referer-Policy
- [ ] Cache-Control
- [ ] Expires
- [ ] CSRF Tokens
#### Business Logic Errors
- [ ] Check how discounts are computed.
- [ ] Race Condition bugs.
- [ ] Check if AAA (Authentication, Authorization, Accounting) is properly implemented.
- [ ] Best Practices
