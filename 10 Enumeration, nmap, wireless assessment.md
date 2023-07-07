- In order to successfully scan our infrastructure for vulns, we need to correctly list everything that's running on our network - which is the purpose of the enumeration process
- Enumeration is also part of the recon phase for attackers

### Enumeration

- Scanning networks and hosts
- Listing everything running on the network
	- Not just hosts and servers, but also IoT, cameras, IP phones - literally everything
- Defenders: identify and describe the attack surface
- Attackers: gather more information about the target

### Active vs Passive

- Active enumeration
	- Noisy!
	- Interacts with targets 
	- Can be discovered if the target knows what to look for
- Semi-passive
	- Looks like normal traffic
	- "Low and slow", takes a long time, but the idea is to hide in plain sight
	- Sensitivity needs to be tuned way down in order to discover it - likely not feasible during normal operation because it will create way too many false positives
- Passive
	- Scanning without sending any traffic
	- No noise
	- Intercepting traffic (not very easy - needs physical interaction such as port mirroring)
	- Tools:
		- [p0f](https://lcamtuf.coredump.cx/p0f3/)
		- [Zeek (formerly Bro)](https://zeek.org/)

### Active scanning categories

- **Footprinting**
	- Discovering the layout of the network
	- Subnets, IP addresses, routing, HTTP options, API endpoints
- **Fingerprinting**
	- Discovering what is running on a specific host
	- Open ports, VM, public shares, services, versions

### `nmap`

- **Exam**: know the following techniques:
	- Host discovery
		- ICMP ping sweep (`-sn`)
	- Scanning
		- TCP SYN (`-sS`) - half-connect (resets after SYN/ACK), faster, "stealthier" (but a fine-tuned IDS will discover it right away), used by default
		- TCP Connect (`-sT`) - full 3-way handshake
		- UDP scan (`-sU`)
		- Null, FIN, Xmas (`-sN`, `-sF`, `-sX`)
		- Zombie host (`-sI`), aka idle scan, also a decoy (a "dead" host is doing the scanning)
	- Other stuff
		- `-T<0-5>` to set scanning speed, 0 is slowest
		- `-F` for fast mode (scan fewer ports than default scan)
		- `-p` to set port/ranges
		- `-D` to use a decoy for firewall evasion
		- `-S` to spoof the source address
		- `-f` / `--mtu` to scan using fragmented packets
		- `-O` for OS detection
		- `-sV` for service/version detection
		- `-A` combines `-O` and `-sV`, aka "aggressive" - also includes traceroute and standard scripts
	- NSE: `nmap` Script Engine
		- Extends scans with specially created scripts for a variety of services, vuln scanning, etc.
	- `nmap` port states
		- `open`
		- `closed` - responds with RST, no connection possible, or blocked with FW
		- `filtered` - likely FW in between
		- `unfiltered` - can't determine if open/closed, but it is reachable
		- `open|filtered` - also can't determine, usually comes back from UDP scans
		- `closed|filtered` - same, comes back from idle scans (zombie scans)

### `hping`

- A younger brother of `nmap`
- Very powerful packet crafting tool, great for recon
- Run `hping3 -h` to view options
- Example scan: `hping3 -S -p 80 -c 1 192.68.1.111`
	- SYN scan, port 80, only send one request
- `-A`: ACK scan (receives RST back if port is open - **exam!**)
- `--tcp-timestamp` - get system timestamp from the network to see how long the device has been up for
- Can do much more! Even DoS attacks and ping of death

### Responder

- MitM tool
- Helps determine if target is vulnerable to LLMNR (Link-Local Multicast Name Resolution) attack
	- Usually happens if Windows falls back to another type of name resolution other than DNS (DNS -> LLMNR -> NetBIOS), then attacker can intercept those requests and inject spoofed responses - this is what Responder does!
	- Sounds like a type of downgrade attack?

### Wireless assessment tools (thank you GPT!)

##### Aircrack-ng

Aircrack-ng is a suite of tools for auditing wireless networks. The primary role of Aircrack-ng is to capture data packets in a wireless network and then analyze them for weak passphrases. Its capabilities include:

- **Packet Capture and Export:** Collects wireless traffic for analysis. Supports exporting the data to text files for use with other auditing tools.
- **Key Cracking:** Can crack WEP and WPA-PSK keys once enough data packets have been captured.
- **Testing Network Security:** Can test the strength of your wireless network by attempting to break its encryption using various algorithms.

Components:
- `airmon-ng` - enable/disable monitor mode on a wifi card
- `airodump-ng` - capture wireless frames, info about the access point based on its MAC addresses, identify clients based on their MAC addresses
- `aireplay-ng` - inject frames, perform attacks to obtain authentication creds for an AP; deauthenticates a device from a wireless network, try to connect when re-authentication is caught
- `aircrack-ng` - extra the authentication key, try to retrieve the plaintext version of the password for the network

##### Reaver

Reaver is a tool specifically designed to exploit a vulnerability in the WPS (Wi-Fi Protected Setup) feature found in many routers. With this tool, you can:

- **Brute Force WPS PINs:** It performs brute force attacks against WPS registrar PINs to recover WPA/WPA2 passphrases.
- **Attack WPS-Enabled Routers:** Reaver has been designed to be a robust and practical attack against WPS, and has been tested against a wide variety of access points and WPS implementations.

Please note that the WPS vulnerability exploited by Reaver has been patched in many modern routers, limiting its effectiveness.

##### oclHashcat

oclHashcat (now known as Hashcat since version 3.00) is a password cracking tool that uses GPU-accelerated computing to crack passwords. It supports many different algorithms and is considered one of the most powerful and flexible password cracking tools. Its features include:

- **Support for Numerous Hash Types:** oclHashcat can crack a wide variety of hashed passwords (md5, sha1, Windows NTLM, etc.), including those used by WPA/WPA2.
- **GPU-Acceleration:** Uses the power of GPUs to achieve faster password cracking.
- **Customizable Attacks:** Supports various modes, including brute force, dictionary, and hybrid attacks.

All these tools are powerful in their own right and are frequently used in penetration testing and security assessments to identify weaknesses in wireless network security configurations. As powerful as these tools are, they should only be used in a legal and ethical manner, and only on networks for which you have explicit permission to test.

---

### Exam

Know `nmap` and `hping` flags, for `nmap` know about SYN, connect, and decoy scans. Be able to discuss active vs passive scanning and footprinting vs fingerprinting, know general info about Responder. This stuff is 95% likely to be on the exam! Also know wireless assessment tools and be able to summarize what they do.