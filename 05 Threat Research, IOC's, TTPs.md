- Threat research: understanding what is threatening our org from the cybersecurity perspective
	- What can be attacked?
	- How can it be done?
- Two things we can do to get this information:
	- Detect threats and attacks
	- Describe what we've found so it can be published and shared

# Threat Modeling

Encompasses all of the below. 

### Signature-based detection

- This is how our AV/anti-malware works
- Strings, byte patterns
	- Found in files on disk, processes, packets
	- Harder to do when it's encrypted
	- But we can still observe packets: how big, how often they come in, what are some anomalies?
		- E.g. requests without a response
		- Misplaced flags
		- Weird packet structures
- Signatures are efficient and easy to update

#### But how can malware avoid detection?

- Thinking like an attacker...
	- Create a new piece of malware that is not yet known (detection is a matter of time though)
	- Create a malware that is so advanced that it's next to impossible to create a signature for it
		- Really complex
		- Requires context and multiple signatures
		- Obfuscation techniques
		- Anti-reversing techniques (armoured virus)
		- Making it behave like a legit app
		- Make it act slowly, stealthily
		- Make it report good things back to an antivirus
		- This is very difficult to pull off, of course - lots of money and time expenditure is needed

## Indicator management

### IOCs

- Time to think like a cybersecurity analyst again...
- What can we do if we're facing malware that doesn't get detected via regular means?
- Stop looking for signatures, start looking at abnormal behaviour! 
- We need proof that malware is there and it's doing something nasty - surely it can't do bad stuff and leave absolutely no trace?
- This proof is known as **Indicators of Compromise**
	- An indication/artifact that serves as proof that we have been breached
- There are different kinds of strange stuff we may encounter:
	- URLs in history/access log/proxy log
	- Files of weird origins
	- Processes being executed
	- RAT: if nothing with remote access is installed on a machine, what's it doing communicating with someone out there?
	- File hashes - kinda like signatures, but we have to look for them
	- Foreign/new registry entries, especially ones concerning startup
	- Excessive resource usage (RAM/CPU/disk/network)
	- New apps
	- Unexpected protocols that shouldn't be found on our network
	- New devices
	- Weird usage in user accounts suggesting data exfiltration
	- Security policy changes
	- And many many more
	- Don't forget mobile devices and IoT
- See [24](https://github.com/ordsec/cysa-notes/blob/master/24%20IOC's.md) for details on all types of IOC's necessary for the exam

#### A shift in perspective

- Some things to keep in mind
- Manual investigation/detection is good, but automated is even better!
	- IDPS, host/network-based
	- SIEM - correlation! A single anomaly/IOC may not be enough to prove the breach and could be open to interpretation, but when it's a clear pattern, it's a different story
	- We need to know what's good and what's bad. Computers create files/processes/connections of their own all the time
		- Triage process - determining which events are malicious and which are normal
		- **Baselining**

### Determining IOC's: reputational method

- Associate an IOC with some reputational data
- Has this indicator ever been connected in some way to an attack? We're looking for anything that has been previously identified as malicious
- Reputation sources:
	- Databases provided by major vendors (Fortinet, Cisco)
	- Online feed
	- Paid subscription
- Examples:
	- IP addresses associated with nefarious activities, such as spam or C2
	- URLs (pretty much ditto)
	- File hashes (of anything ever identified as malicious)
	- Email body/layout (an heuristic method to detect spam)

### Behavioural method

- Advanced threats are hard to detect - either we don't know enough, or there are many different IOC's that have to be correlated
- This is what this method is about: making connections between different attack patterns
- Example: your calculator app suddenly tries to connect to some IP address out there
	- This isn't normal behaviour - are we under attack?
- Another example: system settings are being changed (not a bad thing by itself) while no one with administrative privileges is logged in (and now it's a bad thing - correlation at work!)
- Behavioural methods can't work efficiently without a **baseline** - a clear and well-established definition of what is normal
	- Monitoring software can learn all day every day while observing "normal" activity
	- Once a baseline is created, behavioural analysis becomes much easier

### Attack TTPs

- What are the tactics, techniques, and procedures used by an attacker?
- We can only identify these from known attacks - the ones we had observed previously
- Predicting these is borderline impossible
- DDoS TTP characteristics
	- Unexpectedly high traffic
	- Random distribution of connections from all over the place
	- Resource exhaustion as a result
- Malware TTP characteristics
	- High resource usage (CPU, RAM, disk activity)
	- Abnormal local connections
- Port scanning characteristics
	- A lot of scanning traffic (e.g. half-open or short-lived connections)
	- Simultaneous connections to a lot of ports, especially in sequence
- APT attacks are much more advanced
	- But we can always partially identify them
		- C2 traffic
		- Port hopping: quickly and randomly changing ports for identical connections
		- Hosts attempting such connections
		- Fast flux DNS: quick changes in DNS-resolved addresses in order to evade IP blacklisting
			- DNS mappings should generally be stable, not much flucuation
		- Data exfiltration: sensitive data or files leaving the network
			- Look for alerts generated by access control systems
			- High bandwidth/database usage (could be a false positive)
		- Protocol anomalies: protocols behaving in ways other than what's described in their respective RFCs
			- High volume of normally low-payload protocols, i.e. a protocol that shouldn't normally generate a lot of traffic (ICMP, DNS, NTP)

### Communicating threat modeling data

- How do we describe all this stuff in a structured and meaningful way? How do we write it down and share it? Standardization of tooling would be great - we need a protocol of some sort...

>**STIX**

**Structured Threat Information eXpression**

- A standardized way of describing all findings in threat intel
- STIX v1 was based on XML, STIX v2 is based on JSON (both are data serialization formats) and now managed by OASIS (Organization for the Advancement of Structured Information Standards)
- Consists of **SDO**'s to maintain structure

>**SDO**

**STIX Domain Objects**

- **Observables**: things that can be detected automatically or manually by monitoring systems, e.g. IP addresses or file properties
- Indicators: patterns of observables
- **TTPs**: known behaviours of attackers. If this and that is happening to your system, you're most likely under attack
- **Campaigns** and **Threat Actors**: the actual adversaries doing the bad stuff. This includes the scope of the attack: is it just us being targeted? Is it also other companies in our industry? Does it spill over into other industries?
- **Course of Action (CoA)**: the instructions; what can we do to minimize the damage of an attack?
- There are others

>**TAXII**

**Trusted Automated eXchange of Intelligence Information**

- Protocol to transport this intelligence
- Layer 7, HTTPS
- Basically a REST API
- Two methods of sharing information, depending on implementation:
	- **Collections**
		- Interface to a threat information database or repository
		- Requested directly by the client using the request/response paradigm
	- **Channels**
		- Data is being pushed out to clients instead of being requested
		- Subscription-based
		- Once you've subscribed, you get your threat intel automatically, whenever it's sent out
- Freely available IOC definitions:
	- [OpenIOC](https://www.mandiant.com/resources/blog/openioc-basics) **by Mandiant**
		- Uses Mandiant's indicators for its base framework
		- XML format, logical statements
		- IOC Editor: Windows utility compatible with OpenIOC
			- Much nicer interface
			- Look over all observables with logical connections between them
			- Add your own
		- Metadata includes:
			- Author
			- Name of the IOC
			- Description
			- References to investigation/case
			- Maturity info
			- Definition, possibly with details of the actual compromise
	- [MISP](https://www.misp-project.org/)
		- Works with SDO's and OpenIOC
	- [IBM X-Force](https://exchange.xforce.ibmcloud.com/)
		- Works with TAXII
		- Automatic exports of known threats using the STIX standard

---

### Exam

Know IOC definitions and types, what we should look for, malware detection methods, be able to talk about STIX & TAXII