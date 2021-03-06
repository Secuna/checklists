# Web Application Pentesting Checklist

## Preparation
### Planning and Scoping
- [ ] Finalize the Time of Engagement
- [ ] Finalize the Assets in Scope
- [ ] Finalize the Point of Contact for each party

## Reconnaisance
### Passive Recon
#### Information Leakage
- [ ] Google Dorks
- [ ] theHarvester / Hunter.io
- [ ] Employee / Website Breach (haveibeenpwned, raidforums, leakedsource)
- [ ] Shodan.io / Censys
- [ ] Censys / Facebook Transparency / Google Transparency
- [ ] Get subdomains from CRT.SH or tools.secuna.io
- [ ] LDAP info in SSL/TLS certificate
- [ ] WHOIS Records
- [ ] Wayback Machine
- [ ] StackOverflow
- [ ] Whitepaper/Documentation
- [ ] Cloudflare DNS check
- [ ] Find endpoints in javascript files (grep for HTTP methods)

#### Misconfiguration
- [ ] Missing SPF Record
- [ ] Missing DMARC Record
- [ ] Missing DKIM Record
- [ ] Missing Security Headers
- [ ] Public Github repositories
	- [ ] Check comments on source code
	- [ ] Check issues on repositories
	- [ ] Check for API keys
	- [ ] Check commits

### Active Recon and Analysis
- [ ] httpx to identify technologies (https://github.com/projectdiscovery/httpx)
- [ ] nuclei for scanning (https://github.com/projectdiscovery/nuclei)
- [ ] wafw00f to identify web app firewall (Cloudflare, AWS, Fastly, etc...)
- [ ] Test for [SSL/TLS](https://github.com/drwetter/testssl.sh)
- [ ] Directory and File Bruteforcing using Wfuzz, gobuster, dirsearch
	- [ ] robots.txt
	- [ ] sitemap.xml
	- [ ] crossdomain.xml
	- [ ] clientaccesspolicy.xml
	- [ ] /.well-known/
	- [ ] /server-status/
	- [ ] .git folder
	- [ ] backup files
- [ ] Burp Active Scanner
- [ ] Check for version numbers in the website
- [ ] Sublist3r / wfuzz for subdomain enumeration

## Test cases in common features
### Authentication
#### Registration
- [ ] Username
	- [ ] Special Characters (exclude \<,\>,\[,\],\",\'\{,\} because of XSS issues and dot (.) to avoid username like index.php)
	- [ ] Username Uniqueness (Admin, ADMIN, admin, etc)
	- [ ] Reserved Usernames (admin, privacy, security must not be used as user's username)
- [ ] Password
	- [ ] Minimum of 8 characters with no maximum number
	- [ ] Must not allow dictionary word as password
	- [ ] Does the application know your old password (if tried again)
- [ ] Email
	- [ ] Must not accept any special character other than `.-+`
- [ ] Check for XSS (single quotes) in input fields
- [ ] SQL Injection (sqlmap)
- [ ] HTTP Method when sending credentials (GET or POST)
- [ ] Missing Rate Limit
- [ ] Prensence of CAPTCHA
	- [ ] CAPTCHA provider?
	- [ ] Check if bypassable by OCR
- [ ] Check with existing account
- [ ] Check for Parameter pollution
#### Login
- [ ] Check for default credentials
- [ ] Enumeration
	- [ ] Username doesn't exist
	- [ ] Password is incorrect
	- [ ] Email does not exist
- [ ] Missing Rate Limit
- [ ] HTTP Method when sending username/password (GET or POST)
- [ ] Account Policy
	- [ ] How many invalid requests until user gets locked out?
- [ ] Check for XSS in input fields (\<,\>,\[,\],\",\'\{,\} characters)
- [ ] SQL Injection (sqlmap)
- [ ] Check for Parameter pollution
#### Forgot Password
- [ ] Enumeration
	- [ ] Username doesn't exist
	- [ ] Email does not exist
- [ ] Missing Rate Limit
- [ ] HTTP Method when sending username/password (GET or POST)
- [ ] Account Policy
	- [ ] How many requests until user gets locked out?
- [ ] Can reset token be used again?
- [ ] Check and Monitor the Expiration of Token
- [ ] Leakage of Reset Password Token
- [ ] Check for Parameter pollution
#### Change Credentials
- [ ] check if you can change whitelist/blacklisted domain
- [ ] does application require to input current password when changing?
- [ ] does application require to input OTP when changing?
- [ ] Does the application know your old password (if tried again)
- [ ] What happens if you input null instead of a value?
- [ ] does the application accept the (+) character when changing?
- [ ] Check for Parameter pollution
#### Logout
- [ ] Check cookie if it's still usable after logout
- [ ] Cache issue, Logout then click back button
### Authorization
- [ ] Check for broken access control
	- AutoRepeater for automation of IDOR
	- [ ] Horizontal privesc
	- [ ] Vertical privesc
- [ ] What happens if user has no activity for 15 minutes? (OWASP compliance)
- [ ] Check length of cookie (9 characters or more)
- [ ] Best Practices
### Accounting
- [ ] Does the application log unauthenticated requests
- [ ] Does the application log authenticated requests
- [ ] Does the application log authorized requests
- [ ] Does the application log unauthorized requests
- [ ] Check for verbosity of logs
- [ ] Check for ban bypass
	- [ ] change something you are (either username, MAC Address, user agent)
	- [ ] change where you are (either IP, Location, ASN)
### Parameter Testing
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
- [ ] Insecure Deserialization 

### Misc
- [ ] XML External Entities
- [ ] File Uploader

### Business Logic Errors
- [ ] Check how discounts are computed.
- [ ] Race Condition bugs.
- [ ] Check if AAA (Authentication, Authorization, Accounting) is properly implemented.
