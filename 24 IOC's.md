>**IOC** or IoC

**Indicator of Compromise**

- A piece of data that can be found during or right after a cyber attack that can serve as proof that an organization or a system has indeed been attacked and potentially breached

---

# Network-based IOC's

### Network DoS

- Compromises **availability**
- An attack aimed at resource exhaustion of a server or load balancer and bringing a service down
	- Maxing out CPU/RAM/disk
	- Maxing out **bandwidth consumption** or the number of simultaneous connections the server can handle
	- Requests are not processed anymore, server needs a breather
	- Uses synthetic traffic
- **Indicators**:
	- An unexpected surge in traffic on the local network
	- Excessive numbers of `TIME_WAIT` connections in a load balancer's or web server's state table
	- High numbers of HTTP 503 Service Unavailable log events
	- Large amounts of outbound traffic can indicate that some hosts on our network are compromised and used to DDoS someone else
- Metrics
	- Bandwidth consumption (bytes sent/received, % of the link utilization)
- Mitigation/prevention
	- Continuous monitoring and alerting for **unusual traffic spikes**
	- Either setting a specific threshold, or baselining "normal" network behaviour so any anomalies can be caught
		- Baseline: if normal server load is 5-6%, then when it goes to 7%, it might be an indication of something suspicious; no need to wait for 99%
	- Real-time log analysis
	- Using geolocation and IP reputation data to redirect or ignore suspicious traffic. Geolocation has to be decided based on what places the org is relevant for
	- Aggressively close slower connections by reducing timeouts on affected servers
	- Use caching and backend infrastructure to offload processing to other servers
- When a DoS attack is carried out by high number of compromised hosts, all of them under the same attacker's control, it's a **Distributed DoS (DDoS)** attack
	- The set of compromised hosts sending requests is a **botnet**
	- The attacker controlling it is a **bot herder**
	- Even the largest companies with the most powerful and secure networks occasionally fall victim to these when the scale of attack is big enough
	- Detection is possible, but mitigation is very difficult
		- You can try and filter bad traffic on the ISP side
		- Or send it to a sinkhole (might affect regular users)
		- Either way you have to react, and hopefully you'll still have enough resources to redirect the bad traffic
- **DRDoS: Distributed Reflection DoS**
	- A type of amplification attack
	- Spoof the victim's IP address, send small requests to public servers on their behalf, and those requests will then generate disproportionately large responses that will be sent back to the victim
	- Their bandwidth becomes completely taken up
	- Protocols well-suited for this type of approach: HTTP, DNS, NTP
		- These can all generate responses that are hundreds of times larger than the request
- **Slashdotting**: using a widely available link to generate so much traffic to a resource that it's almost like a true DoS
	- Used to be done on Slashdot, now it can be done on any type of social media
	- You have a million followers, you post a link, they all click it and also share it with others
	- DoS by "going viral"
	- May not be malicious in and of itself, but it can serve as a distraction for an actual DoS attack

### Beaconing IOC's

- Remember that a **botnet** is a network of compromised machines under one attacker's control, all of them likely infected with the same malware
- **C&C** aka **C2** is **Command and Control**: infrastructure used by the attacker to control the botnet, send commands to it, and commence attacks using the botnet
- Beaconing IOC's point to this type of traffic, which is called **beaconing**: attacker is sending commands, and hosts that are part of a botnet are reporting to C2 that they're still up
- In theory, this traffic is pretty regular (like a heartbeat pulse) and has a minimal footprint, perhaps just a SYN. It goes out to an unknown IP address or to a changing variety of IP addresses (fast-flux/DGA - see [29](https://github.com/ordsec/cysa-notes/blob/master/29%20URL%20analysis.md)). Key detail: traffic going out from a bunch of hosts. 
- In practice, it's quite difficult to detect beaconing traffic. There are legitimate protocols and apps/services that perform their own beaconing: NTP, cluster sync protocols, periodic checks for updates. We need a reputation list for IP addresses to identify C2 traffic. 
	- Apply the baseline approach, know what's normal in order to recognize abnormalities
- Attackers can also use **jitter**, which is a random delay aimed at frustrating indicators based on regular connection attempt intervals
- **Exam**: look for stuff happening at **regular intervals** if given an example (outbound traffic sent every 3, 5, 10, 15 seconds, every midnight, etc)
- Protocols and services that can be involved in C2 beaconing:
	- IRC (sometimes automatically flagged by security devices since it's old and also most people don't need it for work)
	- HTTP/S: makes it easier to hide in plain sight, especially when using encrypted traffic
	- DNS: because it's most likely allowed by default for outbound connections; sometimes not even inspected
		- Tools like **`dnscat`** allow one to create encrypted C2 channels over the DNS protocol
		- It'll likely bypass any defenses - most companies don't expect anything to go through DNS other than harmless nameserver traffic
	- Social media can be used to issue commands to botnets by uploading certain comments or pictures that are automatically read by compromised machines
		- This is absolutely normal web traffic to a social network (as long as the org allows them) - what could go wrong?
		- **Blackgear** does this by posting magnet links for bots to read
	- Cloud services can host instructions for a botnet - great uptime, excellent performance!
	- Media files and documents storing **metadata** that can include commands. Steganography also possible for images? Security tools don't really look for this kind of stuff in metadata. 
- **Indicators, summarized**:
	- Same query is repeated several times when a bot is checking into a control server for more orders
	- Commands sent within request or response queries will be longer and more complicated than normal (but we have to know what's normal)
	- Fragmented queries for evasion purposes
- Mitigation: just don't get hacked ;-P

### Peer-to-peer communication IOC's

- P2P: hosts within a network establish connections over unauthorized ports and data transfers
- P2P traffic is usually already a red flag if you take your security seriously
- Used for illegal file-sharing
- Easy to identify - always happens within the network; worms work the same way
- Unfortunately, this type of traffic can be hidden within SMB, SPOOLSS (Microsoft print system protocol) or IPP (Internet Printing Protocol), which is used inside of a network to connect to a local printer and send documents there to print
	- We don't expect this type of traffic to be malicious, so it can be easily overlooked
	- For SPOOLSS, normal traffic can include executables (look for MZ in the hex), which means a client needs the driver to use the printer
- ARP gets a special mention here because it is essentially a P2P protocol. It is also used in MitM attacks (ARP poisoning/flooding), so it does need to be inspected
	- Baseline its behaviour, look for anything weird
	- Use an IDS to catch this by identifying suspicious patterns caused by ARP poisoning generating far more ARP traffic than usual

### Rogue device IOC's

- A rogue device is any device on a network that is either unaccounted for (we didn't plug that AP in, so who did?) or unwanted (i.e. a device we wouldn't normally allow)
- Examples include USB sticks, WiFi adapters and AP's, smartphones, printers, etc.
- Detection and prevention:
	- Physical - keep your eyes open
	- Run network mapping regularly (attackers can rely on a lack of this type of monitoring)
	- Wireless monitoring - pretty tricky to hide on this side of things since wireless signals go everywhere in their range
		- Use specialized tools for finding rogue APs
		- Use existing APs to find sources of interference (any new device emitting in the same spectrum/channel will cause some interference that can be detected). Fancier solutions from Cisco can even show where sources of interference are located
	- Traffic sniffing - this requires a baseline, but also some regular monitoring
	- NAC - configured at switch port level so that no devices from outside the company's control can connect in the first place
	- IDS - analyzes traffic patterns and who's generating traffic, from which VLAN to which destination, and figuring out if a particular source of traffic may be malicious
	- IP address management - keeping track of all IP addresses in the network. DHCP servers with dynamic allocation necessitate tools that track what addresses have been given out in the last days/weeks. This is especially important for restricted networks

### Network scanning and sweeping IOC's

- Sweeping - checking which hosts are up
- Scanning/fingerprinting - figuring out what services are running based on open ports
- Detection:
	- IDPS solutions based on known patterns that suggest sweeping/scanning activity, especially from outside the network. Normally this is easy to detect because a lot of IP addresses receive traffic in a short period of time, and all that traffic looks the same, especially when addresses/ports are scanned in a consecutive manner

### Nonstandard port usage IOCs

- HTTPS should run on port 443, SSH on port 22, etc.
- Well-known ports: TCP/UDP 0-1023
- Registered ports: TCP/UDP 1024-49151
- Dynamic and private range (anyone can use these): TCP/UDP 49152-65535
- All assigned by IANA
- **[List of common ports](https://nmap.org/book/port-scanning.html#most-popular-ports)** - know these!
- None of the above is set in stone, and any app can technically use any port number. However, clients will connect to 443 for HTTPS because they know that's the port.
- Frequent usage of the same dynamic port in network traffic is suspicious
- **Common protocols over non-standard ports** are pretty much an IOC
- Detection:
	- Tools for deep packet inspection, up to layer 7, to figure out what type of application is generating/receiving traffic on a specific port
	- Look for suspicious reverse shell traffic (reverse shells are usually easier to set up because outbound traffic wouldn't be blocked, whereas connecting from the outside would likely be disallowed)
		- `netcat` 
		- Cryptcat - transfers encrypted traffic or files from compromised hosts
		- `socat`
		- Pupy
- Companies that can't afford far-reaching detection solutions usually block all non-standard port traffic - not always the best solution as it may break some apps' functionality

### Data exfiltration IOC's

- Exfiltration means stealing presumably sensitive data
- Usually the goal of a cyber attack
- You can't always encrypt everything, and if the attacker gains high enough privileges, they can easily decrypt it anyway
- Data can only be used by apps/services while unencrypted (unless homomorphic encryption is used), so somewhere, at some point, sensitive data will have to be in plaintext
- So how can we lose data?
	- HTTP/S channels with public storage services (Google Drive, OneDrive, Dropbox) - attacker can either compromise the user account on that service or compromise a workstation and make it upload sensitive files to the attacker's own Google Drive
		- DLP solutions can prevent this - they inspect outbound traffic for this type of stuff
	- Web app attacks (SQLi and such) - a lot of data is hidden behind those apps. Any successful attack can potentially give the threat actor access to sensitive data
		- We must look at web app attacks from the perspective of data loss as well
	- DNS can be used as a data exfiltration channel! A compromised DNS server can be configured to hide confidential information (in TXT records, for instance), then someone simply requests it. The DNS protocol itself can be used to create an encrypted tunnel
	- Instant Messaging, P2P, email, FTP - any type of connection, as long as it's allowed within the company, can create a data exfiltration channel
	- Encrypted tunnels (IPSec, SSL) - the worst one. These are often required for off-site employees as well as customers and partners for remote access, support, assistance, etc. These encrypted connections are a necessity. 
		- Unfortunately, an attacker can use these to steal data upon successful compromise, and there would be no way to inspect that traffic and identify what's leaving the company via those channels
		- Host-based solutions exist for some inspection as long as sensitive data can be found in cleartext before being sent via encrypted channels

### Covert channel IOCs

- Covert means stealth (overt means "in plain sight")
- A covert channel is any method of sending data outside of an org stealthily, without raising any alerts
- How is it possible?
	- Outbound traffic is rarely filtered
	- If it is filtered and inspected, attackers can still come up with some techniques:
		- Encoding data in protocol headers, using protocols beyond their intended purposes (e.g. hiding it in TXT records of DNS services)
		- Fragmentation - sending data in multiple sessions, separate channels, in smaller chunks, over a longer period of time. "Low and slow" approach - good luck catching that
		- Encoding - simply for obfuscation, converting the data to a different charset so that it doesn't match signatures in place
		- Encryption - tools looking for that traffic won't be able to figure it out what it is. Sometimes even a password-protected archive is enough
		- Steganography and security by obscurity - hiding data within metadata or whitespace in an image or even a text file
		- Storage and timing channels. One process writes to a location, another one reads from it, no direct communication between the two, sort of like a dead drop. Example: using ping as morse code. NO ONE can detect this. Also a "low and slow" approach that requires a lot of time.

---

# Host-based IOC's

- Hosts/endpoints/workstations
- All big sources of IOC's - they're the ones being compromised

### **Malicious processes**

- It helps to have a predefined baseline for these - what do we expect to run and when? How do running services behave?
- Processes that don't match the baseline may be IOC's
- Scan running processes for malicious code - DLL's can be loaded in after code injection
	- This is hard to detect - processes don't give away the presence of a malicious library. We need to look at behaviours
- Unexpected changes to Windows registry
- Open files you normally don't see
- Unexpected network activity coming from processes that normally don't generate any traffic
- How do we look for it?
	- Windows:
		- Task Manager
		- Sysinternals Process Explorer
	- Linux:
		- `ps`, `top`, `htop`
	- MacOS:
		- Activity Monitor
		- All the UNIX stuff
	- Other devices such as a NAS can run a Linux distro and can also be compromised, so monitoring is necessary

### **High resource usage**

- Again, have a baseline of normal resource usage. Anything above it is suspicious, although not necessarily proof of malware running
	- **Processor consumption**
	- **Memory consumption**
	- **Drive capacity consumption**
	- Come up with "normal" ranges, monitor for spikes - you never know!
- When in doubt about a certain process, look it up on https://shouldiblockit.com - a resource that describes behaviours of legitimate and malicious processes

### Abnormal process behaviour

- Any abnormal behaviour in core OS processes can be an indicator of a rootkit or other malware exploiting an OS component (exploits that have to do with DLL's - see 43 under "Code injection")
- Attack tools injected into running legitimate processes - `meterpreter` can do this by taking over such processes
- Using names that are very similar to system processes for rogue ones
- DLL execution via `rundll32.exe` to run bad stuff as services thru `svchost`
- See [[37 Endpoint security - behaviour analysis#Low budget?]] - some good behaviour for Windows processes listed there. Anything other than that behaviour is abnormal and is an IOC.

### Disk and file system IOC's

- Malware is usually not just a single file on a drive 
- Fileless malware doesn't get persisted on disk (exists in memory), but it still has some interaction with the FS
	- **Data exfiltration** IOC's - appear after interaction with the FS, with the usage of the following techniques
		- Creating temporary folders
		- Masking data as log files
		- Archiving/encryption
		- Hiding in data streams (unique to Windows and NTFS)
		- Temporary recycle bin use
		- Unexpected changes in metadata
		- MITRE ATT&CK has this under Archive Collected Data and Data Staged
- File system analysis - FS can store much more info than visible files (for instance, metadata, access control lists)
	- Run `dir /AH` or `ls -la` (Windows and Linux respectively) to show all folders/files, including hidden. `dir /Q` will show who owns each file. 
- Disk space usage IOCs
	- Attackers could be storing data for exfiltration
	- Generating excessive log information (brute force attempts, scanning, etc.)
	- Downloading additional stuff from the internet (adware, spyware)
	- We need to be able to identify exactly where the malicious operation is happening, what processes are responsible, and who owns those processes
	- Tools for checking how much space stuff takes:
		- Total Commander (Win)
		- `du -sh` (Linux) - disk usage
- File handles IOCs
	- Files accessed by processes are tracked by the OS
		- `lsof` on Linux lists all open files, can be narrowed down to user (`-u` flag)
		- Security tools can keep track of open files and try to figure out right away whether there's suspicious behaviour, e.g. a process trying to open too many files or files it shouldn't normally access
	- High-volume writes and deletions, attempts to open all documents with a certain extension at once - might be due to cryptomalware running

### Unauthorized privilege IOCs

- Privesc is one of the phases of an attack - the adversary will attempt to get admin/root access
- This is needed to establish persistence, handle files that can't be accessed by regular users, pivot to other systems
- Privileged account usage must be monitored and logged to make it difficult for the attacker to cover up tracks
- Indicators include:
	- Unauthorized sessions (unprivileged user accounts accessing or attempting to access restricted areas)
	- Failed logins - should be logged, but are not by definition IOCs
		- But they are if there are too many of them
	- New user accounts out of nowhere, especially admin accounts
	- Guest account activity
		- Do you even need guest accounts?
		- If you absolutely do, make sure they have extremely limited privileges, monitor and log every step they take
	- Privileged usage/access outside working hours (or from a weird location)
		- Investigate any such activity when it's unexpected
	- Security policy and access list changes
		- Monitor the integrity of those very closely. If an attacker can change these lists/policies, the rest of their activity may end up being completely undiscoverable and it won't trigger any alerts

### Unauthorized software IOCs

- Detect presence of unknown/prohibited software - likely an IOC
	- That includes unexpected security tools installed on a server or workstation
	- It may be legitimate, its presence is still abnormal
- Use specialized tools that can create a timeline, if at all possible
	- When was the app downloaded and installed? How did it end up in our network? When was it launched? 
	- This data is there in the OS - we just have to find it
	- [Cisco Secure Endpoint](https://www.cisco.com/site/us/en/products/security/endpoint-security/secure-endpoint/index.html)
	- Prefetch files on Windows - lots of data about installed launched apps
		- Finding gaps in this data might indicate suspicious behaviour (malware covering its tracks)
		- Prefetch is disabled by default on SSD
	- App use info can be extracted from the registry on Windows (can either be opened with `regedit` or examined with forensic tools)

### Unauthorized changes and hardware IOCs

- System configuration changes
	- Attacker attempting to change security settings
	- Messing with the firewall, disabling security protocols
- Hardware peripherals
	- Attacks can be carried out by USB drives (rubber ducky - USB with overwritten firmware pretending to be a HID, which the OS will run without asking any questions)

### Persistence IOCs

- Persistence: a way for malware to survive a logout, reboot, etc.
- Attackers usually do it via **registry changes** 
	- Antimalware tools can scan the registry for any deviations from an approved template
	- No timestamp for registry changes provided by `regedit` 
	- Autorun is a very common method, and there are registry keys responsible for it (`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run (or RunOnce`, same for `HKCU`)
	- Services (`HKLM\SYSTEM\CurrentControlSet\Services`) - suspicious entries under this key can indicate that malware is running hidden as a background service, very often masquerading as Windows services such as `lsass`
	- File associations can be used to trick users into launching malware by changing the app associated with a specific file extension
		- You expect to open a `.txt` file in Notepad, but something totally different opens it instead 
		- File extension registry entries are in `HKEY_ CLASSES_ROOT (HKCR)` - merges the file extension entries in `HKLM` and `HKCU\SOFTWARE\Classes`
	- **Unauthorized scheduled tasks** - to run whatever the attacker may want every $N$ minutes/hours/days
		- Can be easily displayed using Task Scheduler
		- Get run history to see previous run attempts, when they were made, and whether they were successful
		- For Linux, this can be in the `crontab`

### Application IOCs

- Have a good baseline in order to study application behaviour and have good results
	- What does legitimate behaviour look like? How does it interact with the OS, other processes, the network, etc.?
	- Anything that doesn't look legitimate is potential **anomalous behaviour** and should be alerted on
	- Includes virtualized applications as well, cloud, mobile device apps
- Network connections
	- Start all the open ports locally (`netstat`)
	- Perform a remote scan with `nmap`, look for unusual ports
	- Look for apps listening on abnormal port numbers or apps with open ports that shouldn't have any open ports
		- `notepad.exe` does not need to listen on a network port
	- Look for outbound connections - maybe there's beaconing?
- **Unexpected outputs** and errors
	- Attempts to exploit an app or to scan it for vulns might produce errors. If it's more errors than usual, it's suspicious. These errors should be logged by the app
	- For web apps, request-based attacks (code injection, directory traversal, etc.) can be easy to discover as long as all requests and responses are logged
- Service defacement - pretty obvious, usually relevant to websites
	- The attacker's own content on your webpage is probably an IOC...
	- Performed by hacktivists usually
- **Service interruption**
	- Not every error/crash is a security incident
	- If happening more often, might be suspicious
	- Common patterns include manually disabled services, especially security services; compromised processes running malicious code and crashing due to errors because malicious code is often imperfect
	- A disabled service due to a DoS
- Running services can be viewed in Task Manager or in the command line using the `net start` command for `cmd` and `Get-Service` for PowerShell
	- For Linux, use `ps -aux`, `top`, `htop`, `service`, `systemctl`

### Application logs IOCs

- A goldmine of information
- Properly analyzing app logs can reveal a lot of useful things
	- DNS: queries (particularly malformed ones), list of destinations accessed by an internal host (can find stuff with a bad reputation there), anomalies (a very large number of resolution failures can indicate a DoS) and **unexpected outbound connections**
	- HTTP: status codes for errors
		- 4xx (client error)
		- 5xx (server error)
		- Servers can log request/response headers 
			- Find out what data was being sent to the server
		- Cookies
		- User-agent header fields to identify what HTTP client was initiating the connection
		- Logging requests can assist in finding past intrusion attempts
	- FTP: just log everything - clients, connections, commands
		- Can easily spot brute force attempts
	- SSH: only concerned with the protection of the data channel itself
		- Can't see what commands were run and what files were transferred
		- Can see session info, auth issues, failed login attempts
		- E.g. a large number of failed logins using multiple encryption methods might indicate a recon attack to figure out what type of encryption an SSH server uses
	- SQL: logging is available and is a must
		- But don't stop at that
		- Log access attempts
		- Log queries and their execution status

### New account IOCs

- An important method of persistence/backdoor is for the attacker to create a new account on the target machine
	- If there are hundreds of accounts that are used by people, services, and machines, what's one more account? It may easily be overlooked
	- Account creation and privilege assignment must be very well defined and accounted for; should be a part of a well-built change control process
- Windows: local vs AD accounts
- Linux: 
	- Active list of users
		- `w` command (for "who") lists currently logged-in users 
		- `lastlog` to show recent login info
		- `cat /var/log/auth.log` for past user creation commands
		- `faillog` to track authentication failures

### Virtualized apps IOCs

- Many apps are designed to run in virtual environments
- Lots of benefits from the admin perspective, but from the security standpoint it creates some challenges
- Process and memory analysis
	- Hypervisor VM introspection tools (i.e. the hypervisor can access the memory of every VM)
	- Suspending VMs to write all its memory contents to the disk (similar to hibernation on PC)
- Disk and persistent storage analysis
	- Quite a bit easier because because the VM disk is in an image format - it's just a file
	- VM files inside the VM disk
	- Recovering deleted files is easy 
- System logs
	- VM apps sometimes undergo re-creation processes
	- Entire apps might be destroyed and re-created as a new VM, from a new disk image, with its config re-applied
		- Convenient from a management perspective, but without centralized logging in place, any local logs will be lost, so make sure you don't lose them after an infrastructure rebuild (as a result of a DoS attack for instance)

### Mobile app IOCs

- Sometimes very difficult to look for IOCs in these
- Mobile devices hide stuff really well these days - not just a black rectangle, but also a black box
- Storage is soldered on board of the device - can't extract it, sometimes can't even access it without 3rd-party tools
- Bypassing phone security measures (like a screen lock) is very difficult]
- Devices are encrypted by default and have been for a while
	- JTAG interface may come in useful for reading contents
- Cloud data
	- Where mobile device backups live
	- And also device configs, data, login data, passwords, session data
	- Depending on which cloud that is, we can get plenty of info
	- Google stores a lot of data such as location history
- Carrier data
	- Everything related to calls, voicemails, SMS/MMS, network traffic activity for mobile data, geolocation info

---

### Exam

Overall, this is a huge exam topic, so it's important to know all the different IOC types, whether host-based or network-based, be able to recognize them, know ways of detecting them.