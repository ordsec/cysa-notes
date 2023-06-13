- For the exam, we only need to be aware of some tools oriented towards web application vulnerability scanning
- These are also known as "interception proxies"
- Testing how secure a web app is when it's up and running

### Nikto

- Open-source web app scanner, Windows or Linux
- Looks for vulns, misconfigurations, weak configurations
- Attempts to identify which apps are running on the web server (virtual hosts for instance)
- Only offers non-credentialed scans
- Output can be generated as HTML - pretty!
- Vuln references are provided using OSVDB, which is deprecated - [here's a mapping of OSVDB entries to CVE](https://cve.mitre.org/data/refs/refmap/source-OSVDB.html)
	- Just Ctrl+F the OSVDB code and it may be in there
- Can run passive with the `-findonly` flag, which only discovers HTTP/S ports, reports the server header, but doesn't perform a security scan
- Guess/brute-force values such as subdomain names or directory names using the `-mutate` flag (but there may be better tools for that)
- Can try and check specifically for some known vulnerabilities with `-Tuning`
	- Does not try to exploit them

### Arachni

- Free, open-source
- Multiplatform
- Audits HTML/JS forms and any other inputs the website can receive
- Looks for weaknesses (code/SQL injection, XSS, etc.)
- Can generate reports about what was attempted and what was found
- Runs as a Docker container
- Has a web interface, which provides are more intuitive way of configuring a scan

### Interception proxies

- Scanners, but with additional functionality
- Sits between the user and the website and intercepts requests, which can then be modified, automated, etc.
- Inspecting and manipulating data in transit
- **Burp Suite**
- **OWASP ZAP** (Zed Attack Proxy)
	- Both are huge tools with tons of functionality
	- Both have free community editions with limited functionality (but still quite powerful)
	- Capture requests, examine them, modify them, see what comes back, perform fuzzing, brute force things, examine encryption
	- ZAP has automated scans that combine vulnerability discovery and lightweight exploitation (SQLi and such), can impersonate user-agents, has credentialed and non-credential scanning options

--- 

### Exam

Just know the above tools and what they do - no deeper knowledge required, but it's never a bad idea to mess around with them and get some hands-on experience.