- Networks exist for the sake of traffic
- Most attacks involve the network
- We need to analyze the traffic for bad stuff

### Capturing traffic

- The first step
- It's not meant for us, but we need to look at it...
- Furthermore, how do we do it for an entire network or segment?
- We need to know exactly what to capture and analyze
	- Well, everything of course!
		- Inline capture (placing your own device in between two hosts on the same network/segment)
			- It works, but it's hard to implement, it's intrusive, and you have to do it for each cable/connection, meaning it doesn't scale at all
		- Make a copy of all traffic on the switch
			- SPAN (Switched Port ANalyzer) for Cisco switches (here's [some documentation from Cisco](https://community.cisco.com/t5/networking-knowledge-base/understanding-span-rspan-and-erspan/ta-p/3144951))
			- Port mirroring for other vendors
			- A device that can be configured to take all traffic from one or more **ports**, create a copy of it, and route it to a different port, where packet analysis software is located
			- Downside: switches must support this type of thing (aka managed switches)
		- Perform a MitM
			- Only gonna work if network is not properly secured - and why isn't it?
			- Just not a desired way
	- Just a summary, please (for when it's not feasible/possible to capture everything)
		- Who was talking to who, for how long, how much data was transferred
		- Like a phone call log - you don't get to hear the actual convo
		- In networking, this type of summary is called a **flow**
		- **Network flow**: a sequence of packets that have some parameters in common (like src & dst addresses and ports) - like an overview of a conversation, but only from the standpoint of who spoke to who, not what was said
		- Lots of different implementations, standards, and naming conventions:
			- **Netflow**, Flexible Netflow by Cisco
			- sFlow
			- J-Flow by Juniper
			- IPFIX
			- and others
		- Lately these standards have become interoperable, so we can grab flow info from different vendors, collect it into the same database, and get a coherent view regardless of how many different vendors' devices that network uses
		- We need networking devices for this kind of summary, with netflow (meaning the generic term) being implemented on them - most likely on switches
		- Switches can be configured to look at all the traffic passing through them and upload flow data to a centralized application at regular periods of time
			- Broken down by top talkers, top destinations and sources, showing how much data was transferred - lots of details and ways to sort it
			- How much Netflix has the sales department watched in the last month?
			- Gather it to a centralized console, generate smart reports with stats and security-related information (any data streams in the network that shouldn't be there, such as VMs communicating when they shouldn't or clients trying to access restricted areas on the network or suspicious destinations on the web)

### Full packet inspection

- Inspecting everything - requires a copy of all traffic (inline capture is impossible, don't even attempt it)
- Hardware:
	- Managed LAN switches + SPAN/port mirroring
	- Unmanaged switches + sniffer/TAP (Test Access Port), which is a dedicated device that can be passive or active, generating copies of traffic that comes in and out of one or more ports
		- SPAN functionality from a switch, but implemented in a completely separate device
		- Use this only for critical network assets/segments, or at critical network access points
		- The problem is that you'll be capturing ALL TRAFFIC, which is way too much and not feasible for analysis or even storage as the amount of space it'll take up will also be massive
- Software:
	- `tcpdump`
		- CLI utility, but very powerful
		- Allows us to choose how captured traffic is presented and what data we want to see (e.g. include MAC addresses, don't convert IP's after DNS resolution, and much much more), saves packet dumps to `.pcap` files
	- Wireshark
		- Does the same thing, but with a very well-implemented GUI and some awesome functionality such as packet breakdown by layer, following conversations, creating stats, displaying raw packet data in hex, and much more

### Packet analysis vs protocol analysis

- **EXAM!!**

##### Packet analysis
- Deep packet inspection, frame by frame; looking at headers, looking at contents, all the way to the smallest detail
- From the security standpoint, this helps us identify things like C2 traffic, malicious URLs, data exfiltration attempts, and other traces of network-based attacks
- Looking at network traffic in real time, searching for proof of a breach if we suspect one 
- Traffic itself is not enough, however - we still need tools to reconstruct larger pieces of data
- These tools are called **file-carving tools**
	- Give it a pcap, it'll try to extract anything that was transferred: files, images, emails, anything that's broken down into smaller chunks
	- **NetworkMiner**: live sniffing, PCAP parsing, extracting certificates, extremely useful for network-based forensics
	- **Suricata**: network IDS, IPS, security monitoring engine. Open-source, tons of functionality, rule-matching, etc. [Full documentation](https://docs.suricata.io/en/suricata-6.0.12/)
	- **Zeek** (formerly Bro): open-source security monitoring tool, can also work with pcap files, generate log files with info it finds within traffic
- Great - I can look at the entire traffic and rule the world! Not really, here are some issues:
	- Massive datasets being generated by traffic captures. On a home network, you can get thousands of packets per second. Imagine an enterprise network, even if it's a small segment.
		- Filtering out things before capturing is possible but not a good idea - you might miss things, and the entire capture becomes useless
	- Most of it is encrypted, silly! If you don't have a way of decrypting it, not much analysis can be done
	- File extraction tools try to identify content by searching for specific types of file headers (first 64 bytes of any file) - and this info can be forged; malware can be obfuscated by making it look like a completely innocent type of file that you may not even pay attention to 
	- Unknown/non-standard protocols your tools may not know how to work with
	- Sadly, we need a bit of luck in addition to our intelligence and intuition

##### Protocol analysis
- Only deals with looking at data pertinent to the protocol in use
- Layers 2, 3, 4, a little bit of basic info from layer 7
- Protocol info: headers, some payload data
- Detecting protocol anomalies (packets you'd expect to follow a specific protocol, but they don't have the right flags or exhibit the right structure) - potential IOC
- Detecting unknown protocols - if you're unable to determine what type of traffic in your network you're looking at (and you know for certain that it doesn't belong), that's bad news. These days you can tell the difference between two different online games being played just by looking at traffic they generate, so any unknowns on this front are not good
- Detecting statistical anomalies - certain sequence or frequency of packets that might indicate an attack pattern (scanning traffic, for instance)
- Some insight into the actual application within those packets, even with encrypted contents: certain exchanges that may stand out, large data transfers (especially outbound - might suggest exfiltration)
- Cisco offers [ETA: Encrypted Traffic Analysis](https://www.cisco.com/c/en/us/solutions/enterprise-networks/enterprise-network-security/eta.html) - claimed to be able to find cybersecurity threats just by looking at encrypted traffic and its patterns. Relies on advanced modeling, ML, behaviour analysis, etc. - basically anything that can be done outside of trying to decrypt what can't be decrypted
- Wireshark offers stats for basic protocol analysis and provides some insight into what may require further investigation

##### Flow analysis
- Full packet capture is very costly in terms of configuration and resources in a large environment with tons of traffic traversing the network every 100th of a second
- Flows can provide a good summary without requiring a lot resources for storage and analysis
- Flow data can be provided by a variety of networking devices such as switches, routers, firewalls, IDPS, or even wireless LAN controllers
- Data is sent to a centralized database
- An aggregation app is therefore required for storage and processing - it can be a SIEM, but that's not required
- Refer to [IPFIX Entities (by IANA)](https://www.iana.org/assignments/ipfix/ipfix.xhtml) to see what kind of information is included in flow data
	- What protocol
	- TCP/IP info
	- src/dst IP and ports
	- Interface info
	- BGP metric parameters
	- Start/end of flow
	- Smallest/largest packet size
	- Sampling intervals and algorithms
		- Sampling is an important concept because most flow technologies don't look at every single packet - instead, they look at one in every 10 or 100
		- This is because switches usually don't have more CPU power than what's needed for their primary task, which is forwarding traffic
		- Enough for most situations, but not for security applications, where we need to have a high level of precision in pinpointing attack evidence
		- Recently, though, network devices have been becoming more beefy, so unsampled netflow is becoming a reality
- The concept of netflow has been introduced by Cisco
- Nowadays all vendors use netflow as it is standardized
- Pros:
	- Low resource requirements
	- Newer versions capture certain fields or routing protocol information
	- No dedicated hardware required! This is a big one - no new installations/configurations are required. All we need is a centralized storage component
	- Unified view among vendors, aggregated information is standardized and looks the same
	- Great for reporting and visualization
- Cons:
	- No payload visibility - this ain't packet analysis! Malware will go undetected
	- Cannot be reviewed/processed manually - have a physical/virtual machine running on the network with enough resources to collect this data, but it can't be processed by a network engineer
	- Sampled (you only look at 1 in N packets), but unsampled solutions are becoming more widespread
- Tools:
	- [Solarwinds Network Traffic Analyzer](https://www.solarwinds.com/netflow-traffic-analyzer)
	- [ManageEngine NetFlow Analyzer](https://www.manageengine.com/products/netflow/)
	- [Cisco Secure Network Analytics](https://www.cisco.com/c/en/us/products/collateral/security/stealthwatch/datasheet-c78-739398.html) - security intelligence based on collecting flow information, formerly Stealthwatch
	- [Silk](https://tools.netsa.cert.org/silk/), open-source
	- [Argus](https://qosient.com/argus/argusnetflow.shtml), open-source
	- [MRTG (Multi-Router Traffic Grapher)](https://oss.oetiker.ch/mrtg/) - agent-based, works with individual network devices (not just the entire network), uses SNMP; not really flow info, but it can give alerts about resource usage or easily identifiable traffic patterns (floods, DoS, scanning)

---

### Exam

Understand the importance of capturing traffic, explain how it can be done (`tcpdump`, Wireshark). Discuss the difference between packet and protocol analysis, what goes into each, what the limitations are. Define flow information, what it's for, and what tools are used to collect it.