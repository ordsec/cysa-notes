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
	- Utilities: **[CISA](https://cisa.gov)** (Cybersecurity & Infrastructure Security Agency), guidance for a lot of industry sectors
- All this info sharing fits very well within the **dissemination** phase of the Threat Intelligence Cycle!
	- Use cases:
		- Risk management and security engineering
			- Upgrade yourself!
			- Choose the right security controls (hardware, software, etc)
			- Reduce your attack surface
			- Software development security - make sure secure code is written while weak code is not used
			- Incident response - what do we do when under attack? How do we respond? We need secint for this, sharing comes into play here
		- Vulnerability management
			- This is on the **strategic** (big-picture) level
			- Keeping a close eye on the infrastructure, identifying any new vulns/weaknesses ASAP
				- Either due to changes or due to a newly discovered threat/attack
			- Vuln management is **proactive**: fixing gaps before something bad has a chance to happen
				- Integrating secint into the org and its processes
				- Always up to date on the security posture
				- Yes, it's time-consuming, but it saves money by avoiding very costly breaches
		- Detection and monitoring
			- Using secint to improve our own ability to detect future threats
			- More info is better!
			- Beefing up our firewalls, IPS's and EDR systems to better match real threats
			- **Avoid false positives as much as possible** - fine-tune your defenses, remember the boy who cried wolf
			- **Avoid false negatives too**
			- Keep it running and up to date - this is ongoing, not "set it and forget it"
			- Everything should be monitored

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

### Exam

Know about ISACs, what they are, what they do. Understand how information sharing is useful for risk management, incident response, vuln management, threat detection - i.e. why we do this in the first place. It's about our own ability to monitor and detect threats before they turn into incidents.