If every org had to look for CTI and secint on their own, they'd have no time for anything else because this is a massive effort. Thankfully, there are well-established ways to share this info.

### Information sharing communities

- Governments worldwide have been gathering cybersecurity knowledge in the fields of utilities, healthcare, finance, air transport, and others, in order to be able to intervene if cyber attacks happen against these industries - this can endanger lives
- **IMPORTANT**: human life is the #1 priority! As far as CompTIA exams are concerned, and everywhere else of course. There may be questions about saving lives - and saving lives is the best answer.

> **ISACs**

**Information Sharing and Analysis Centers**

- Non-profit orgs that share this secint information
- Gather info directly from people that work in specific industries
- Info is very specific and very relevant
- Example ISACs
	- Healthcare: [h-isac.org](https://h-isac.org), focused on prevention of PHI loss
	- Financial sector: [fs-isac](https://fsisac.com), focused on companies that are obvious fraud targets
	- Aviation: [a-isac.com](https://a-isac.com), focuses on intelligence concerning terrorist attacks
	- Federal gov-t: MS-ISAC (multistate ISAC), EI-ISAC (elections infrastructure)
	- Utilities and critical infrastructure: **[CISA](https://cisa.gov)** (Cybersecurity & Infrastructure Security Agency), guidance for a lot of industry sectors
- All this info sharing fits very well within the **dissemination** phase of the Threat Intelligence Cycle!

### Threat intelligence sharing - use cases

- **Risk management** and **security engineering**
	- Upgrade yourself!
	- Choose the right security controls (hardware, software, etc)
	- Reduce your attack surface
	- Software development security - make sure secure code is written while weak code is not used
	- Incident response - what do we do when under attack? How do we respond? We need secint for this, sharing comes into play here
- **Vulnerability management**
	- This is on the **strategic** (big-picture) level for now
	- Keeping a close eye on the infrastructure, identifying any new vulns/weaknesses ASAP
		- Either due to changes or due to a newly discovered threat/attack
	- Vuln management is **proactive**: fixing gaps before something bad has a chance to happen
		- Integrating secint into the org and its processes
		- Always up to date on the security posture
		- Yes, it's time-consuming, but it saves money by avoiding very costly breaches
- **Detection and monitoring**
	- Using secint to improve our own ability to detect future threats
	- More info is better!
	- Beefing up our firewalls, IPS's and EDR systems to better match real threats
	- **Avoid false positives as much as possible** - fine-tune your defenses, remember the boy who cried wolf
	- **Avoid false negatives too**
	- Keep it running and up to date - this is ongoing, not "set it and forget it"
	- Everything should be monitored
- **Incident response** (more details on IR in [53](https://github.com/ordsec/cysa-notes/blob/master/53%20Incident%20response%20phases%20and%20communication.md))
	- Threat intel is a crucial part of IR as it allows us to respond to threats more effectively and efficiently
	- Sharing info about threats with other orgs and communities
	- Helping others prepare for and maybe even prevent similar attacks
	- Ways threat intel can be used in IR:
		- Contextualizing incidents: comparing an incident against shared threat intel, understanding the nature of the attack and who might be behind it, TTP's, motives, targets; helps inform the response strategy
		- Identifying IOC's - see [05](https://github.com/ordsec/cysa-notes/blob/master/05%20Threat%20Research%2C%20IOC's%2C%20TTPs.md) and [24](https://github.com/ordsec/cysa-notes/blob/master/24%20IOC's.md)
		- Prioritizing responses: determining the severity of an incident, allowing for better prioritization of of IR efforts. Example: if a piece of malware detected in the network is known to be part of widespread ransomware campaign, it would have to be prioritized over less significant incidents
		- Updating defenses: using threat intel to update preventative controls and defenses to protect against known threats. Got word of a new vuln being exploited that's relevant to the org? Go beef up your defenses!
		- Enhancing predictive capabilities: over time, threat intel can help us identify trends and patterns in cyber threats, allowing us to try and predict what future attacks might involve

### Vulnerability management - the process

- **Assign responsibilities**
	- Your employees have to know exactly what they're responsible for
- **Document EVERYTHING properly!** 
	- Don't rely on people's memory
- **Management support**
	- Make sure company higher-ups (and subsequently the users) understand the reason behind vulnerability management!
- **Inventory**
	- Devices, assets, configurations - back up everything that's important, especially configs
- **Assign risks & prioritize**
	- Based on inventory, determine how badly the org would suffer if a specific item were to be compromised, be it a server, or a router, or a storage device
- **Select the right tools** for assessing vulnerabilities
	- This is where we actually find the vulns - manually or with dedicated automated tools (e.g. Nessus, OpenVAS)
		- Think about configuring everything properly, tweak your tools to match your environment, customize everything - we do want to avoid false positives/negatives!
- **Scan for vulnerabilities**
	- Fix ASAP
	- Do not procrastinate!

---

### Exam

Know about ISACs, what they are, what they do. Understand how information sharing is useful for risk management, incident response, vuln management, threat detection - i.e. why we do this in the first place. It's about our own ability to monitor and detect threats before they turn into incidents.