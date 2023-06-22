- To understand what risks you're facing, you have to define in exhaustive detail what is important to your org
	- This is what we want to protect!
	- Systems that support our business
	- Assets, most valuable resources we rely on
- Once defined, understand how all these things can be threatened from the CIA perspective
- **Threat modeling**: how we describe all the threats and cybersecurity risks our business faces

### Threat modeling questions

- How secure are we currently?
- What future risks are we facing?
- What would an attack look like?
- Where would it originate? We need this to decide where to focus our attention
	- Internal threats? External threats?
	- Internet? Mobile?
- What is the risk of harm being done to any of the CIA attributes? 
	- And which ones are we NOT concerned about in a particular situation
- How likely is a specific threat?
	- Do we know for sure that there's a specific exploit/attack we're vulnerable to?
	- Or are we just worried that there *might* be something?
	- Prioritize what we need to be prepared for. A meteorite strike isn't very likely, so maybe we don't have to worry about it?
- What defense capabilities do we have?
	- I.e. what is our current security posture? What can we use to defend ourselves right this moment? Can it be improved? If so, then how?
	- Your defenses are your assets too - inventory them!
	- Repurpose currently existing security controls if need be
- What is missing?
	- This determines all of the next steps

## Threat modeling aspects

### Adversary capabilities

- Bad guys' ability to develop and execute an attack
- We need to have this info available
- The more threat intel we have, the more we already know about this!
- How do we determine this? By answering the following
	- Who are the most likely attackers?
	- What kind of resources do they have access to?
	- How well are they funded?
- [A document by MITRE for describing adversary capabilities](https://www.mitre.org/sites/default/files/2021-11/prs-18-1174-ngci-cyber-threat-modeling.pdf) - summarized below

### Adversary capabilities, according to MITRE

- **Acquired**
	- Beginner hacker, not a lot of experties or resources at their disposal
	- Uses commodity malware (i.e. tools developed by others)
	- Plays by the book, follows a standard recipe, barely any creativity or innovation
	- Does not rely on human deception or social engineering, except for maybe some basic email scams
- **Augmented**
	- Has more experience, uses known vulns or researches them beforehand
	- Might use custom malware, but tools are still fairly standard
	- Even if they develop their own 0-days, their tools are still purchased from others (perhaps on the dark web)
	- Limited deception/human involvement
- **Developed**
	- Can develop their own malware
	- Has expertise to discover completely new vulns
	- May rely on stronger human interaction (remember social engineering techniques such as coercion)
- **Advanced**
	- Able to influence commercial products/services
	- Can manipulate the supply chain, even create new vulnerabilities
- **Integrated**
	- Barely any limit at this level
	- Pretty much bottomless resources
	- God-level skills
	- Generate their own opportunities
	- Own 0-days and always work on finding new ones
	- Can even leverage political/military assets
- And yet... no matter how advanced a threat is, attacker capabilities are strongly affected by what defenses we have in place!
	- We want to make attacking us so difficult that bad guys simply go look for victims elsewhere - unless they're quite determined to go after our org specifically
	- Make it not worth the effort to attack us

### Total attack surface

- Everything that you own can be attacked - by someone inside or outside
	- Buildings, employees, systems, endpoints, mobile devices
- Asset inventory steps:
	- Physical and non-physical, put everything you can think of on this list, including business processes as they rely on the human factor
	- Look at the corporate network. What ports are open? What wireless networks are accessible inside/outside of the building? What cloud infrastructure/apps are we running? Check the web presence! All existing points of entry must be considered
	- Connections to the outside world, how we ingest data, our online presence, our APIs we're exposing
	- Internal apps
	- Buildings and people
- **Try to be as "small" as possible when viewed from the outside!** This is in terms of how opened up we are to the world. 
	- Some online presence is needed - but apply least function to this. Reduce, harden.

### Attack vectors

- An attack vector is the actual method of how an attack is executed on an attack surface
	- How is it being delivered?
- Attack vector categories defined by MITRE:
	- **Cyber**
		- IT systems
		- Social media
		- E-mail
		- USB drives
		- Open ports
	- **Physical**
		- Doors
		- Locks
		- Access cards
		- Surveillance
		- Don't forget bollards
	- **Human**
		- Impersonation
		- Phishing
		- Coercion
		- Blackmail
	- These categories can be combined (e.g. DoS'ing a surveillance system falls under cyber and physical, or the way phishing combines the cyber and the human aspects)

### Likelihood

- Is it a real threat or a probable one?
	- Is something out there definitely threatening us, or is it just in theory?
- Has it happened to us before? Has it happened to anyone else before, whether in our industry or in general?
- Really this is just an educated guess. We can take all these things into account, but no one can predict the future. Historic results don't guarantee future success!

### Impact

- Potential consequences or damage that could occur if a threat is realized
- Financial loss, reputational damage, regulatory penalties, operational disruption, even physical harm
- Questions to ask when assessing impact:
	- Could the threat impact physical safety and well-being of people? **Human life is always first!**
	- What's the potential financial loss?
	- How much downtime or disruption could occur?
	- Could sensitive data be compromised?
	- What would be the effect on the org's reputation?
- This ties with risk assessment if you think about it - [see 52 for more](https://github.com/ordsec/cysa-notes/blob/master/52%20Continuity%20planning%20%26%20risk%20assessment.md)

### Final threat modeling questions

- What's the motivation behind an attack?
	- Who might want to hurt us and why? Be self-aware, brainstorm this.
- How do other companies in our industry/field defend themselves? Have they been exploited before? Learn from others' mistakes.
- How frequent are attacks in our industry? 
- Accept the fact that one day we will be a victim of an attack. Keep calm, model your threats, set up your defenses! Beef up your incident response, come up with contingencies, find all possible ways to minimize damage.

**Threat modeling**: the theory
**Threat hunting**: the practice

---

# Cyber threat hunting

- Actively searching for cyber threats that might otherwise go undetected
- Says Infosec Institute:
	- "Cyber threat hunting is very similar to real-world hunting: it requires skill, patience, creativity, and a keen eye for spotting the prey."

### Threat hunting approach

- We kind of assume an attacker's role - attackers are mostly active, after all
- In our case, the "prey" is the malware
- The approach is to **be proactive**
- Start investingating **before** you have the proof of being compromised
- Assume you have been breached - don't wait for it to happen
	- We wait as we deploy countermeasures - not to say this shouldn't be done, but the best way is to combine it with threat hunting
- Completely different from incident response 

### Threat hunting methodology

- You **start from scratch** - there's no proof of compromise yet!
- If you start with proof (IDPS alerts, an endpoint exhibiting strange behaviour), it's no longer threat hunting - it's incident response because 
- **Establish a hypothesis**
	- A valid assumption
	- What's typical for our business? Are we most likely to be attacked?
- **Look for threat actors** and profile them
	- This is done using threat intel
	- Decide - as an assumption - which of them apply to us
	- Narrow down the search
- **Look for IOCs**
	- This is where we start the actual threat hunting
	- If there's been a compromise, evidence will be there
	- Investigate logs, network activity, workstation behaviour, file integrity

### Executable process analysis (thank you GPT!)

In the context of threat hunting, executable process analysis refers to the practice of examining and analyzing the processes running on a system to identify potentially malicious activities. This analysis involves looking at various attributes of each process, including but not limited to:

- The nature of the process: Is it a system process, part of an application, or something that should not normally be running?
- The parent-child relationships between processes: Is there a process that was spawned from another process in a manner that's unusual or indicative of known malicious behavior?
- The resources that a process is accessing or using: Is it accessing files, network resources, or system calls in a suspicious way?
- The behavior of the process over time: Is it acting in a way that's consistent with its intended function, or is it exhibiting anomalous behavior?

By monitoring and analyzing the state and behavior of running processes, threat hunters can identify unusual or suspicious behavior that may indicate a security threat. This helps in proactive detection and mitigation of threats, rather than waiting for a breach to occur or relying solely on automated alert systems. It's an important part of an overall threat hunting strategy.

### Integrated intelligence (thank you GPT!)

Integrated intelligence in the context of threat hunting refers to the use of multiple sources of intelligence, both internal and external, that are integrated into a coherent and coordinated system, to provide a comprehensive view of potential threats.

Here's a bit more detail:

**Internal Intelligence:** This includes logs, alerts, and other data generated by an organization's systems and security controls. Examples include network logs, firewall logs, intrusion detection/prevention system (IDS/IPS) alerts, system logs, application logs, etc. This data can be used to detect suspicious activities within the organization's network.

**External Intelligence:** This includes information about threats and threat actors obtained from outside the organization. Examples include threat intelligence feeds, reports from cybersecurity firms, information sharing groups, open-source intelligence (OSINT), etc. This data provides information about known threats, vulnerabilities, attack methodologies, and threat actors.

By integrating both internal and external intelligence, organizations can create a more comprehensive and accurate picture of the threats they face. This integration often involves the use of Security Information and Event Management (SIEM) systems, Threat Intelligence Platforms (TIPs), or other technologies that collect, normalize, analyze, and present data in a way that's useful for threat hunters.

Integrated intelligence allows threat hunters to correlate events across different sources, identify patterns, detect anomalies, and prioritize threats based on their potential impact and the organization's specific context. This approach enhances the ability of threat hunters to proactively find and mitigate threats before they can cause significant damage.

### Justifying threat hunting

- The caveat is that we're spending a lot of time and money on something that may or may not be real, so how do we justify it? Here's what we can tall the upper management if they're in doubt:
	- We may have already been compromised!
	- We'll discover new, unknown attack surfaces
		- The bigger the company, the more we're not aware of
		- Run a permission audit - you never know what you may find
	- Drastically improves our threat detection abilities
		- We might discover new weaknesses! Remember unknown unknowns
		- It'll likely be a quite instructive process - we'll learn more by doing it
	- Provides additional security intelligence
		- Demonstrates just how useful these intel sources are
		- Contribute to these sources also!
	- Identifying critical assets
		- Though you should already know these at this point
		- Some areas can surely be improved - adding/improving controls, hardening configurations
	- It's good to have this process ongoing
		- If the org hasn't done it in the past, they may not see the value
		- But if it's done at least once, we won't have to do all this convincing

---

### Exam

Understand threat modeling, what it is, keep in mind adversary capabilities and how to determine attack surfaces. Be able to discuss threat hunting, how the process works, and what the benefits are