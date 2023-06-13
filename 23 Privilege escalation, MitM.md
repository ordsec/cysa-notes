### PrivEsc

- You gained access to a system, but your account is `www-data` - boring!
- You want to **escalate** your privileges to gain access to more of the system
- The goal is escalating to `root` on Linux or `SYSTEM` on Windows
- There's no legitimate way to do it, so some sexy hacking is needed
- Different methodologies:
	- Using a variety of exploits, depending on OS and its configuration, to get `root`/`SYSTEM` shell. Often relies on weak configuration and doesn't exploit actual CVE's
	- Injecting your code into an app that runs with higher privileges
	- Finding a way to run something as `sudo` or "as Administrator"
	- Getting all creative with social engineering, finding a human vulnerability, tricking them into running something that'll give the attacker elevated privileges
- Mitigation:
	- Strong passwords, MFA, least privilege/last function for users and apps, input sanitization, patching, hardening, no default credentials (latter can be found all over the place, by the way, for instance on [DefaultPassword.us](https://defaultpassword.us))
	- Being very vigilant with configurations. There are so many ways to escalate privileges, and many of them don't rely on bugs in application/OS code

### Pass the hash

- For authentication, most services check the two hashes for a match (stored and a hash of a password provided by the user)
- Older versions of Kerberos allowed an exploit where you could present a valid password hash without knowing what the underlying password is
- Password hashes can be sourced by dumping/harvesting
- Older versions of Windows (with LM hashes) are vulnerable
	- But also newer ones that have backwards-compatible configurations for legacy systems within the network

### Golden ticket

- Works within an AD/Kerberos environment (Kerberos uses ticket-based authorization)
- Can grant Admin permissions in AD
- On vulnerable versions, dumping AD data store (`NTDS.DIT`) reveals `krbtgt` hashes
- These hashes can be used to create a "golden ticket" for any user
- It's a master key for every single resource
- **Silver ticket**: used to create multiple TGS tickets for a specific service without communicating with a DC

### Man-in-the-Middle (MitM) attacks

- Affects networks
- Involves traffic interception
	- Attacker is able to inspect traffic and make changes to requests and responses
	- They can impersonate the service the traffic is meant for
	- Or forward it to the final destination, making the attack invisible to the victim
- Affects software
	- Code injection
	- Intercepting whatever the user does
	- Common type: Man-in-the-Browser
		- A browser is compromised
		- All input and mouse clicks and even screenshots of activity can be forwarded to an attacker
	- Keyloggers are a type of MitM as well
		- Installed as part of a trojan, for example
		- Grab all keystrokes, forward it to the attacker
- Ways to perform MitM in a network:
	- ARP poisoning
		- ARP is used to get a device's MAC address from its IP on the network
		- These ARP requests ("Who has 10.0.0.1?") are **broadcast** - all hosts get them
		- A malicious host can craft their own ARP reply and pretend to be a different host
		- Now they're getting all packets meant for a different host and can forward them between the legimiate source and destination
		- Mitigation:
			- DAI (Dynamic ARP Inspection), configured on the switch, looks at all ARP requests/replies, makes sure they come from legitimate switch ports
			- Relies on DHCP snooping, which looks at DHCP requests (upon first connection) and knows which IP belongs to which switch port
			- The two make faking ARP replies impossible, but your switches need to support this technology

### DNS attacks (against servers)

- Buffer overflows - can have many different consequences
- DNS amplification - the initial is request disproportionately small compared to the reply, but in order for the attacker to not DoS themselves, they can use a spoofed IP address to perform this
- This IP address can be a different DNS server
- Mitigation: 
	- Patching
	- Blocking malicious sources using a blacklist based on reputation

### Rootkits

- Kernel-level malware
- Infecting/replacing OS-level files with malicious code (think of a malicious `cmd.exe` or `notepad.exe`), can happen to DLL's as well
- This malicious code will then run with the highest possible privileges
- But installing a rootkit requires root access as well
- Mitigation:
	- Antivirus solutions
	- File integrity monitoring (FIM) for all system files
		- Watching system files to see if there are any changes, in which case it throws an alert
		- Creates handles that attach to these files
	- Baseline clean OS installation with everything signed
- **Exam**: File integrity check solutions:
	- **TripWire**
		- Designed for Linux
		- Uses a local database to keep track of all the files
		- Monitors not just hashes, but also metadata
	- **File Sight**
		- More complex, agent-based, agents communicate to a centralized console
		- Works on Windows

---

### Exam

Discuss privesc, MitM and ARP poisoning as an example, know attacks against AD/Kerberos (pass the hash, golden ticket) as well as attacks against DNS and rootkits. Know mitigation methodologies for all of these. FIM and TripWire may be asked about as well.