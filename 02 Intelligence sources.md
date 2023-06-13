- Threat intelligence: collecting, analyzing, and drawing conclusions from information about bad actors that threaten the well-being of our business and its data
- Cybersecurity intelligence: **how secure are we?**
- CTI: **how threatening is the world outisde?**
	- How bad is our neighbourhood (the internet)?
	- Who's out there trying to get us?
	- How can they do it?
	- What can we do about it?

### Sources

- **Narrative sources**
	- Read about what attacks are out there
	- Common methods, vuln types, etc
	- Comes in handy when we need to 
		- buy some new equipment
		- decide on a new vendor
		- build new features
		- configure stuff on our network (e.g. when scaling up)
	- Lots of manual effort!
- **Treat feeds**
	- Online source of data designed to be queried automatically
	- When more automation is needed
	- Continuous flow of info
	- Minimizing manual work
	- Always up to date
	- Sometimes provide real-time info
	- Can be free, but good feeds cost money

### Historical/trend analysis

- Info from the past
- Logs are a good source
- Look it and search for patterns
	- And this is where we can learn from mistakes
- Correlation is key - build cause/effect chains between different types of events

### Reconnaisance - OSINT and closed-source

- A great starting point
- We think like an attacker here
- What could a potential attacker find out about us?
- Where would they look?
	- **OSINT**
		- Our website
		- LinkedIn - be careful with job descriptions, avoid giving up info about your tech stack
		- Dedicated tools
		- Free feeds
			- Alienvault
			- MISP
			- NCAS
			- VirusTotal
			- Blogs (DigitalGuardian)
	- **Closed-source**
		- Very well-maintained
		- IBM X-Force
		- FireEye
		- Fortiguard by Fortinet
		- Cisco Talos
	- Don't forget about basic tools
		- `whois`
		- `nslookup`/`dig`
		- Google's `dig` service
		- `host`
		- zone transfers
	- Dedicated OSINT tools also
		- FOCA (document metadata)
		- theHarvester (searches for emails, domain names, IP addresses, etc - all from public info)
		- Shodan (search engine for anything connected to the internet: servers, IoT devices, home routers, webcams)
		- Maltego (establishing relationships between companies using OSINT)
		- Recon-ng (Python tool for subdomain enumeration and other web recon)
		- Census (device search engine)
		- Website rippers (cloning entire webpages to be downloaded locally, can't grab backend code though)
		- Google dorking
			- `"<exact match>"`
			- `-exclude`
			- `<something> [AND|OR] <something_else>`
			- `filetype:`
			- `allintitle:`
			- `allinurl:`
			- ...and many others - [Google Hacking Database](https://www.exploit-db.com/google-hacking-database)
			- Use Advanced Search also
- Follow OPSEC principles!

### Confidence levels

- Is this source trustworthy? 
	- Not just based on consensus - you have to establish it for yourself
- Confidence can be established based on the following criteria
	- **Timeliness**: is the source up to date?
	- **Relevancy**: does it even apply to us? If target tech of threats/attacks we're looking at is absent from our tech stack, we have nothing to worry about!
	- **Accuracy**: is it totally generic, or is it specific enough to be used to quickly and efficiently build up defenses against the threat?
	- **Is it fake news?** It can be clickbait.

### Admiralty system

**A method of evaluating the reliability of a source and the credibility of information it provides**.

- Governments are more in touch with this than the private sector, but this is on the exam.

### Exam

Know intelligence sources (OSINT, closed-source), know tools, know methods of source evaluation. As cybersecurity analysts, we have to always be on top of all the publicly available information because hackers have access to it too.