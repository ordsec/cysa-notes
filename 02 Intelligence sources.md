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
- **Threat feeds**
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
- Look at it and search for patterns
	- And this is where we can learn from mistakes
- Correlation is key - build cause/effect chains between different types of events

### Threat intelligence sources

##### Open-source intelligence
- A great starting point
- Threat intelligence that's acquired from publicly available sources
- There are quite a few, sometimes the challenge is around deciding which sources to use (usually it's a combination)
- Free feeds
	- [Good list on Senki.org](https://www.senki.org/operators-security-toolkit/open-source-threat-intelligence-feeds/)
	- Alienvault OTX
	- Threatfeeds.io
	- MISP
	- NCAS
	- VirusTotal
	- Blogs (DigitalGuardian)
- Government resources
	- [CISA](https://www.cisa.gov/)
		- [Automated Indicator Sharing (AIS)](https://www.cisa.gov/topics/cyber-threats-and-advisories/information-sharing/automated-indicator-sharing-ais)
		- [Information Sharing and Analysis Organizations](https://www.cisa.gov/information-sharing-and-analysis-organizations-isaos)
	- [DoD Cyber Crime Center (DC3)](https://www.dc3.mil/)
- Vendor websites
	- [Microsoft's threat intel blog](https://www.microsoft.com/en-us/security/blog/topic/threat-intelligence/?sort-by=newest-oldest&date=any)
	- [Cisco Security](https://sec.cloudapps.cisco.com/security/center/home.x)
- Public sources
	- [SANS ISC](https://isc.sans.edu/)
	- [VirusShare](https://virusshare.com/) - details about malware uploaded to VirusTotal
	- [Spamhaus](https://www.spamhaus.org/) - blocklists for spam, hijacked/compromised computers, policy block list, Don't Route or Peer lists (DROP) listing netblocks you may not want to allow traffic from

##### Closed-source/proprietary
- Commercial, comes from companies' own information gathering and research using custom tools, analysis models, proprietary methods
- Orgs sometimes want to keep their threat data secret, so they put it behind a paywall to discourage at least a certain percentage of attackers
	- Also orgs' methods and sources are their trade secrets
- Often part of a service offering that can be a compelling resource for security professionals
- Allows to zoom in on what's actually relevant without combing through tons of open-source data
- Saves time overall
- Some sources:
	- IBM X-Force
	- FireEye
	- Fortiguard by Fortinet
	- Cisco Talos

### OSINT and reconnaissance
- OSINT is not to be confused with open-source threat intel
- We think like an attacker here
- What could a potential attacker find out about us?
- What would they use to find this info?
	- Our website
	- LinkedIn - be careful with job descriptions, avoid giving up info about your tech stack
	- [Lots more sources - look at the OSINT Framework](https://osintframework.com/)
- Basic tools
	- `whois`
	- `nslookup`/`dig`
	- Google's `dig` service
	- `host`
	- zone transfers
	- Banner grabbing can be performed with `nc`, `wget`, `telnet` (`ftp` cannot grab a banner)
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
- Follow OPSEC principles, don't give up more info about the company than needed!
	- This includes job descriptions - make them secure, don't disclose your tech stack

### Confidence levels

- Assessing threat intelligence, regardless of the source, since in the end it's us who decide whether something is a threat or not
- Is this source trustworthy? 
	- Not just based on consensus - you have to establish it for yourself
- Confidence can be established based on the following criteria
	- **Timeliness**: is the source up to date?
	- **Relevancy**: does it even apply to us? If target tech of threats/attacks we're looking at is absent from our tech stack, we have nothing to worry about!
	- **Accuracy**: is it totally generic, or is it specific enough to be used to quickly and efficiently build up defenses against the threat?
	- **Is it fake news?** It can be clickbait.
- [ThreatConnect's rating system for intel](https://threatconnect.com/blog/) - just one example of a rating system for threat feeds
	- **Confirmed (90-100)** - the threat is real, proved by independent sources and analysis
	- **Probable (70-89)** - relies on logical inference but doesn't directly confirm the threat
	- **Possible (50-69)** - some info agrees with the analysis, the assessment is not confirmed and is somewhat logical to infer from given data
	- **Doubtful (30-49)** - possible but not the most likely option, assessment can't be proven or disproven by available info
	- **Improbable (2-29)** - assessment is possible but not the most logical option, or it is refuted by other information
	- **Discredited (1)** - confirmed to be inaccurate/incorrect

### Admiralty system

- **A method of evaluating the reliability of a source and the credibility of information it provides**
- Governments are more in touch with this than the private sector, but this is on the exam.
- The rest is explained by GPT:

The Admiralty System (also known as the Admiralty Code) is a method used in intelligence circles for evaluating the reliability of the information or intelligence gathered. It's a two-dimensional system evaluating both the reliability of the source and the credibility of the information itself. 

This system originated from the British Navy and is commonly used by NATO and other military or intelligence organizations.

It generally consists of a two-letter grading system:

1. **Reliability of Source:**
    - A: Completely reliable
    - B: Usually reliable
    - C: Fairly reliable
    - D: Not usually reliable
    - E: Unreliable
    - F: Reliability cannot be judged

2. **Credibility of Information:**
    - 1: Confirmed by other sources
    - 2: Probably true
    - 3: Possibly true
    - 4: Doubtful
    - 5: Improbable
    - 6: Truth cannot be judged

For example, an intelligence piece with a rating of "A1" would mean that it came from a completely reliable source and the information itself has been confirmed by other sources. Conversely, a piece of intelligence rated "E5" would be considered unreliable and improbable. 

This evaluation system provides a quick way to assess the potential validity and trustworthiness of intelligence. It can be especially useful in scenarios where decision-makers need to make quick judgements based on the available information.

---

### Exam

Know intelligence sources (open-source, closed-source/proprietary), know about OSINT and tools, know methods of source evaluation. As cybersecurity analysts, we have to always be on top of all the publicly available information because hackers have access to it too.