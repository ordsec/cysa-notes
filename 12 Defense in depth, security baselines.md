- How would an attacker try and break into our office? 
	- Scope out security guards, figure out personnel schedules
	- Look for ways to override alarms, avoid security cameras and other forms of detection
- All these things are defense layers an attacker has to overcome to gain access
- This is what defense in depth is all about

# Defense in depth and its levels

- Designing our physical and cyber security to have all sorts of layers working together
- If one layer is bypassed, the next one might succeed in protecting us from the attacker
	- Example: multiple firewalls at different levels
- **Vendor diversity**: if one vendor's solution gets exploited, it won't affect others
- **Device diversity**: when possible and feasible, look for different solutions that can help achieve one specific goal
- Assume compromise while designing your security solutions
- "If this particular control gets bypassed, what's our next line of defense?"

### Personnel

- Once again, people first
- Layers of security for personnel:
	- Training and awareness
	- MFA
	- Separation of duties 
		- At least 2 people involved in performing a task that has critical impact (e.g. one initiates, the other approves/reviews); no single person has too much power
		- Be careful with third parties and what level of access they have, watch it closely, audit, follow least privilege/function
	- Mandatory vacations
		- Critical to avoid fraud and misuse of resources
		- Every once in a while a specific worker **must** go on vacation
		- Replaced by someone temporarily
		- Great for detecting any nefarious activity, insider threat prevention
	- Succession planning
		- Plan for situations when a key employee is gone, for whatever reason
		- This is basically "personnel backup" - for instance, there shouldn't be just one sysadmin with access to everything; if something happens to them, it becomes a huge problem
	- Job rotation
	- Secure job descriptions

### Three directions

- Business processes
	- How the data flows within the company
	- Which people are involved in how data is stored, transported, and secured
	- Learn from trends
	- Continuous search for weaknesses, continuous improvement
- Technology
	- What specific software/hardware can be improved/upgraded to strengthen our security posture?
	- Security as a Service - outsourcing the security effort (not responsibility or liability as that's still on your org!) 
- Network
	- Constantly review the network design
	- Come up with a clear layout of the network: which departments need to communicate with each other, what traffic flows are required, 
	- Segmentation - separate anything that requires extra care into a different segment, which sets up another line of defense. Use VLANs, access rules, firewall rules, dedicated firewall devices, what users need access to what devices
	- Always be in control of how the traffic is flowing within the org
	- In case of an incident, a well-designed network will help isolate what's compromised and stop the attacker 
	- Consider SDN - best way to design a segmented network
		- On-the-go reconfiguration, quick deployment, same outcome every time
		- Automation
	- Consider an air-gapped network for extra security if something needs to be kept offline. Pretty extreme, but may be necessary
	- Don't overcomplicate your network design! The more complex, the harder to manage, the easier for an attacker to hide somewhere

### Configuration baselines

- Baseline: your own definition of what is "normal"
- Know exactly what your normal operations look like, use it to discover illicit activity
- Use the baseline to:
	- Detect misconfigurations in your infrastructure
	- Detect anomalies
	- Detect attacks before they succeed
	- Prove something is incorrect because you know what the correct way is
- Don't overcomplicate it, start with a simple inventory
	- List IPs and MACs in use on the network
	- List services that are expected to be running at any point in time during operation
	- Average CPU/memory/network usage
	- Usual processes that run every day
	- Whitelisted apps

### System hardening

- Reducing the attack surface as much as possible
	- Disabling devices/services that are no longer needed
	- Securing what's left running, especially if exposed to the outside
- Keep in mind that not all systems can actualy be updated/patched - EOL, EOSL, custom software
	- Take other precautions if not possible (think compensating controls)
- Configuration hardening
	- Deactivate non-critical components, services running by default that you don't need
	- Change default credentials
	- Disable unused accounts
	- Least function
	- Patch, update
	- Restrict access to peripherals
	- User permissions - least privilege
	- ACL on resources (file servers, databases)
	- Install security suites, network- or host-based (IDPS)

### Patching

- #1 solution for most remediations
- A lot of exploits target known vulns that should've been patched since patches are available
	- All you had to do was patch it! Don't let these things slide
	- Example of how the human factor creates the biggest vulnerabilities
- Patch management should be an integrate configuration management process in every single company
- In a company with a large network, include a testing environment for patches to avoid breaking stuff that's been working before the patch
- **Exam**: know automated patching solutions
	- Windows Update
	- [Windows Server Update Services (WSUS)](https://learn.microsoft.com/en-us/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus)
		- Automated patch installation method
		- Repository for patch information, especially useful for larger networks
	- Linux package managers: `apt`, `yum`
- Patch management solutions
	- Monitor the state of your system
	- Throw alerts when noncompliance or outdated components are detected
	- Dedicated solutions include:
		- [Microsoft Endpoint Confugiration Manager](https://learn.microsoft.com/en-us/mem/configmgr/core/understand/introduction)
		- [Solarwinds Network Configuration Manager](https://www.solarwinds.com/network-configuration-manager)

### Exam

Understand the importance of configuration baselines - how are they important for the security posture, how do they help detect anomalies/attacks before anything bad happens? Be able to discuss the importance of configuration hardening, with examples of steps taken in this process, particularly regarding desktops and servers.