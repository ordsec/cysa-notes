- This isn't about malware signatures and antivirus updates
- It's about identifying malware by looking at its behaviour
- How does it interact with our system? 
- In this context, special attention has to be given to endpoint and mobile devices
- A lot of diversity among endpoint/mobile devices in terms of models, OS, etc.
- No single unified solution, but several techniques can be combined
- Users are always the biggest vulnerability
- Signature-based scanning shouldn't be overlooked - it's an essential part

### Endpoint security solutions

- Smarter solutions for lesser known threats are based on **heuristics**
- We don't exactly know what to look for when malware becomes very advanced
- Attackers know about signatures and try to evade them

##### Signature detection - but already pretty smart
- **HIDS/HIPS**
	- Usually packaged as software suites that detect/prevent intrusions on a local machine
	- Monitoring the file system and OS files, drivers, app executables, log files
	- Network traffic, what apps are listening on what ports, where outbound connections go; also monitoring for apps installing virtual network interfaces
	- Advantage: these can look at encrypted traffic - or rather traffic that has not yet been encrypted before travelling across the network, or has already been decrypted
- **File integrity and monitoring tools**
	- Keeping an eye on OS files, throwing alerts about or even blocking attempts to violate the integrity of these system files
	- Maintaining a db of file hashes 
	- Also a type of behaviour analysis - we don't know what the signature of the specific malware is, but we can look at what it tries to do and confirm that it's up to no good
	- **Exam**: one tool used for this is [Tripwire FIM](https://www.tripwire.com/products/tripwire-enterprise/tripwire-file-integrity-manager)
- **EPP: Endpoint Protection Platforms**
	- More comprehensive
	- More of an all-in-one software, "complete security": IDS, antivirus, anti-rootkit, traffic filter, search engine result filter, etc.
	- Most of the time it slows down the computer quite a bit
	- But they're good at what they do
	- In an enterprise environment, control over EPP can be centralized
	- Lots of visibility

##### More advanced solutions
- **EDR**: Endpoint Detection and Response
	- Continuous monitoring and response to advanced threats
	- Identifying malicious behaviour based on ML rather than signatures
	- Looking for endpoint IOC's or suspicious behaviour, detecting anomalies
	- Lots of vendors developing these solutions
		- Cisco Secure Endpoint (formerly Cisco AMP)
	- Catching unknown malware based on things that look out of context
		- Duplicate processes
		- Processes that don't need a network connection initiating such connections
		- Processes with no attached information or metadata
	- Behaviour info can be correlated between a larger set of hosts
		- E.g. an unknown process on one host that then pops up on another dozen hosts - even if there's no signature, this is worm-like behaviour and it needs to be looked into
	- Can handle ransomware, malware, data exfiltration
- **UEBA**: User and Entity Behaviour Analysis
	- CompTIA really needs you to know this one
	- What do users do and how? When? What are the usual actions and what stands out?
	- A little intrusive and big-brothery
	- Using ML to build a baseline by job role or user type, which will then help us identify abnormal behaviour happening on a user's workstation
	- Can be implemented as part of a SIEM
	- Solutions:
		- [Splunk User Behaviour Analytics](https://www.splunk.com/en_us/products/user-behavior-analytics.html?301=/en_us/software/user-behavior-analytics.html)
		- [Microsoft Advanced Threat Analytics](https://learn.microsoft.com/en-us/advanced-threat-analytics/what-is-ata)
- **Heuristics**
	- Again, this is all about the behaviour - of systems and apps
	- Tracking it requires looking at the CPU, memory, network, and disk usage
	- Manual review, automated tools
	- Some tools provide the ability to use heuristic detection methodologies
- **Known-good behaviour**
	- The "normal" baseline behaviour
	- Established as a result of monitoring a system over a period of time and comparing behaviours
	- What allows us to distinguish **anomalous behaviour** right away
	- Tools for detecting anomalous behaviour rely on heuristics and signatures, also checklists of configuration information and likely problems

### Low budget? 

- All these highly advanced smart solutions are great, but they are very expensive
	- Not just money wise, but also in terms of manpower that's needed for configuration and continued usage
- Some things we can do when these tools are not available:
	- Always remember that you won't know when something is suspicious until you know what "normal" is
		- Check for `idle` and `system` PID's (0 and 4 respectively) - there should be no duplicates
		- `smss.exe` - first user-mode process, can only be a child of `SYSTEM`
		- Only one `wininit.exe`
		- Only one `services.exe`, others should be children of `services.exe` or `svchost.exe`, which is a wrapper for multiple services
		- All system services must be digitally signed
		- Services should only be launched by `SYSTEM`, `LOCAL SERVICE` or `NETWORK SERVICE` accounts and nothing else
		- Only one `lsass.exe` - handles authentication and authorization
		- Only one `winlogon.exe` - handles GUI for each user session, so if there's more than one, then someone else is connected to your machine
		- `userinit.exe` should not persist - it launches `explorer.exe` and then exits, so if it keeps running, there's something fishy
			- `explorer.exe` is the file manager but also the parents process for any other processes manually launched by the current user
- Use [Sysinternals](https://learn.microsoft.com/en-us/sysinternals/) for all sorts of Windows monitoring
	- ProcMon is great for exploring the process tree, also tracks FS and registry changes
- As far as Linux:
	- Monitor `/etc/passwd`, `/etc/shadow`, `.ssh` folders (FIM solution?)
	- `/etc/xinetd.conf`, which is what older versions use to initialize processes - should be monitored for backdoors
	- Newer systems use `initd` and `systemd` - the latter has logs that can be viewed via `journalctl`

##### Examining suspicious process behaviour by hand
- Use the **Process Explorer (Sysinternals)**
- Read/write file access
- Where it was launched from - a system-like process launched from your home folder is sus
- Is anything obfuscated of compressed? 
- What's the parent process? There's usually only one proper parent process - any others are sus
- What if you try and kill the process? Does it come back? How does it come back?
- Any network traffic and active connections? Does it listen on a port? Is it initiating connections?
- Command-line utility for this is `tasklist`

##### Autoruns
- Another Sysinternals tool
- Can identify any user-mode malware that's set to run on system boot 
- Looks over scheduled tasks
- Look over everything, make sure it all corresponds with software you have installed
- Anything unexpected? Investigate

---

### Exam

Compare and contrast signature and behaviour analysis, know solutions for each, especially concepts such as HIDS/HIPS, EPP, EDR, and UEBA. Know how manual investigation can be performed and what tools are used. Be able to identify what info we should be looking for from a given scenario.

---

# From the Sybex book

### Layered host security

- Same layered approach as for larger systems
	- Strong passwords and associated policies
	- Host-based firewalls
	- DLP
	- Whitelisting/blacklisting
	- Antimalware, antivirus solutions
	- Patch management, vuln assessment
	- System hardening - see 12
	- Encryption either for files/folders or FDE
	- File integrity monitoring
	- Logging

### Windows tools

- `wmic` (WMIC - Windows Management Instrumentation Command-line)
	- Managing and interacting with various components of Windows
	- Provides a scripting interface for automated administrative tasks and system management
	- Querying system settings, managing services and processes, obtaining system info
	- Can be used locally or remotely - no need for GUI
- `sc`
	- Command-line tool for communicating with the Service Control manager and services
	- Managing services (creating new one, stopping/starting/deleting existing ones)
	- Querying status information for services
- `services.msc`
	- Microsoft Management Console (MMC) snap-in
	- GUI for managing services - pretty much the same functionality as `sc`
- `secpol.msc`
	- MMC snap-in for configuring local security policies and settings
	- Password/audit policies, user rights assignments
	- Typically used for computers that are not part of a domain (otherwise it's under Group Policy)
	- Does **not** manage services unlike everything above!