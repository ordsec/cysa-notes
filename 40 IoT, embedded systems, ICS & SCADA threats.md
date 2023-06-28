### IoT

- IoT: Internet of Threats (or Things, who even knows...)
	- Has grown considerably in the last decade
	- Internet-enabled fridges, stoves, TV's, alarm systems, dog feeders, and plant growers are omnipresent
	- Each one potentially enables an attack vector
	- Becoming more common in business environments
- Problem: **IoT devices are not built with security in mind**
- The easier to use, the more vulnerable, show up on the news all the time
- Hardware and OS's are quite limited - usually tuned-down versions of Android or Linux
	- Since resources are limited, ecurity features are either eliminated completely or are also severely limited
- Most people who buy IoT devices are not very technical, so security is eliminated to remove extra overhead
	- Unfortunately, this reduces awareness as well
- Oftentimes there is no admin interface at all
- [It can get quite bad...](https://mashable.com/article/casino-smart-thermometer-hacked)
- Subscribe to security bulletins from vendors to find out about updates

### Embedded OS

- What IoT devices are largely based on
- A type of OS designed for a device that only performs one specific function
	- From a heart rate monitor to a system that controls metal casting
- Usually static: can't be changed by the user, only by the manufacturer (makes vuln detection VERY difficult)
- Updates may or may not be available, sometimes there's no way to update at all
- **Embedded systems**: computers integrated into the operation of another device (vehicle, camera, MFP, etc.); very simple, one function per system
	- MFP: multifunction printer; related ports are 631 (CUPS) and 9100 (RAW aka direct-IP port); if found in a scan, then the device is likely a printer

>**PLC**

**Programmable Logic Controller**

- A controller in an industrial setting that has been ruggedized and adapted for controlling manufacturing processes (assembly lines, machines, robotic devices, etc.)
- High reliability, ease of programming and fault diagnosis
- These can be reprogrammed or updated, but only with the help of the vendor
- High-risk embedded systems can be found in POS - can be compromised to steal customer data
- **SoC**: System on a Chip
	- Tiny computer that saves space and power, has its own CPU and memory, connectivity, storage, and so on
	- Arduino, Raspberry Pi - not so much IoT anymore, but still SoC
- **RTOS**: Real-Time Operating System
	- Limited functions, extremely time-dependent execution
	- Focused on executing operations at a very specific point in time
	- High reliability and stability, but security features be lackin
	- Used for PLC's, IoT devices in a low-power environment
	- Can be attacked quite easily - delaying something will usually spell disaster
	- No room for handling attacks
	- [Example of RTOS attack](https://www.theregister.com/2019/07/29/wind_river_patches_vxworks/)
- **FPGA**: Field-Programmable Gate Array
	- Reprogrammable chips designed for simple, repetitive operations
	- Can only execute operations from pre-defined, pre-programmed sets
	- Can involve **ASIC's** (Application-Specific Integrated Circuits): non-programmable chips that come hard-coded from the manufacturer
	- Can be used for packet encryption and decryption, packet switching (within switches)
	- Vulnerable not just to physical access, but also to supply chain attacks
	- [FPGA vulnerability example](https://www.eenewseurope.com/en/starbleed-vulnerability-revealed-in-xilinx-fpgas/) - bug cannot be patched, all you can do is replace the chips

### BAS: Building Automation Systems

- It's like your smart home with the cool internet-enabled thermostat, but on the enterprise level
- Much more complex, handling a lot more systems
- PLC's can be found in this setting too - for instance, elevator controllers
	- To remediate vulns in these, isolation is likely the best answer since replacement/disconnection can trap someone in an elevator or create other major problems
- **Physical access control**, lights, climate control, fire suppression, video monitoring, etc.
- Similar to an ICS (also rely on embedded controllers and sensors with centralized monitoring)
- Often overlooked from the security perspective
	- Installed and maintained by 3rd parties, often ignored in a security assessment
	- Usually administered from a web interface, designed for internal use only, so security features are often lacking (lack of encryption, for instance, or hardcoded security credentials)
- **PAC**: Physical Access Control systems
	- A subset of BAS
	- Door locks, window locks, surveillance, etc.
- [Some BAS vulnerabilities](https://www.forescout.com/blog/vulnerabilities-in-building-automation-systems/)
- HVAC, fire suppression - tied into automated systems to reduce burden on human staff

### Vehicles and drones

- A very widespread category of embedded systems
- Car entertainment systems, self-driving systems, engine control, stability, driving assist, parking assist, all sorts of sensors...
- Each such system is called an **ECU**: Electronic Control Unit
- It's controlled by a **CAN**: Controller Area Network
	- It's a serial bus: a shared channel that transmits data, bit by bit, over a single wire or fiber
	- Ethernet operates the same way
	- No security built in, all ECU's receive the same messages; there's only broadcast, no source addressing, no authentication
	- No TCP/IP overhead
- The interface for these systems is the OBD
- You can cause DoS and crash car systems by accessing the OBD
- These days it's also possible to access the CAN remotely since many cars now have cellular connections (think remote start functionality)
	- Cars can also create mobile WiFi hotspots - another gateway for attacks
	- [Kill a Jeep's engine while being outside of the car](https://www.cnbc.com/2015/07/21/hackers-remotely-kill-jeep-engine-on-highway.html)

### ICS

- Industrial Control Systems: automating control machinery such as assembly lines, managing critical infrastructure: power, health, nuclear, communications, water, etc.
- Mission-critical for any country
- ICS describes a larger set of industrial sites
	- **DCS** (Distributed Control System) is a single site
- These systems run software on regular computers on PLC's
	- PLC's gather data from outside sensors and controllers called **field devices**
	- PLC's are linked by a **fieldbus**: an industrial network system for real-time distributed control
	- PLC's can also be linked via ethernet connections to actuators that operate the actual engines and to all necessary sensors
- **HMI** (Human-Machine Interface): an interface used by us humans to configure and monitor the output of a PLC

### SCADA

- **Supervisory Control and Data Acquisition**
- Controls large-scale ICS's with multiple sites
- Most often deployed on an air-gapped network
- Run on general-purpose computers
- Very strict requirements, updates are very rare and sometimes impossible - hence the air-gapping
- Lots of legacy hardware and software involved
- Sooo... security much?
	- Document and monitor all links and connections, whether existing or possible
	- Try to stay away from legacy OS's
	- If not possible, air-gap, air-gap, air-gap
	- Secure web-app-based interfaces: OWASP Top 10 applies
	- Physical security: all removable media has to go through security procedures to make sure there's no malware (remember [Stuxnet](https://en.wikipedia.org/wiki/Stuxnet)?)
		- USB, CD, floppy disks...
	- Audit periodically, hire good technicians who understand industrial security
	- Security solution examples:
		- [Cisco Cyber Vision](https://www.cisco.com/c/en/us/products/collateral/se/internet-of-things/datasheet-c78-743222.html)
		- [Data diodes](https://www.ciberseguridadlogitek.com/en/ot-networks-segmentation-and-fortification-the-data-diode/)

### Modbus

- A protocol used in ICS to read and update PLC configurations
- Old as mammoth turds - no security whatsoever until recently
- Originally designed for fieldbus (pre-dates Ethernet)
- Evolved now to use Ethernet and TCP/IP
- Modbus vulns: [paper 1](https://imt.uoradea.ro/auo.fmte/files-2015-v1/Gabor%20JAKABOCZKI%20-%20VULNERABILITIES%20OF%20MODBUS%20RTU%20PROTOCOL%20-%20A%20CASE%20STUDY.pdf), [paper 2](https://dione.lib.unipi.gr/xmlui/bitstream/handle/unipi/11394/Evangeliou_1508.pdf?sequence=1&isAllowed=y)
- It's not all grim, improvements have been made:
	- TLS has been introduced
	- [An article documenting some improvements](https://blog.se.com/industry/machine-and-process-management/2018/08/30/modbus-security-new-protocol-to-improve-control-system-security/)

---

### Exam

Lots to know here: what IoT is and what the implications are; embedded devices and their aspects: embedded OS, SoC, RTOS, FPGA, BAS; how embedded devices in cars work (ECU's, CAN, OBD); details about ICS and SCADA and underlying aspects such as Modbus and PLC's. Be able to discuss whether or not these can be secured, and if so, how. General awareness is enough - the exam doesn't go into too much detail.

---

### GPT explains workflow and process automation system threats and vulnerabilities

Workflow and process automation systems, which are increasingly powered by technologies like robotic process automation (RPA) and artificial intelligence (AI), can bring tremendous efficiency gains to an organization. However, like all technologies, they also come with their own set of potential threats and vulnerabilities:

1. **Unauthorized Access:** If an attacker gains access to an automated system, they can potentially manipulate it for their own purposes. This could include modifying workflows to create fraudulent transactions, or to create backdoors for future access.

2. **Data Leakage:** Automated systems often process sensitive data, such as financial details, personal data, or proprietary business information. If these systems are not secured properly, this data could potentially be exposed.

3. **Lack of Oversight and Control:** Automated processes can make decisions and perform actions at speeds far beyond human capability. This can sometimes lead to situations where errors or malicious actions go unnoticed until significant damage has been done.

4. **Software Vulnerabilities:** Like all software, automation systems can contain bugs or vulnerabilities that can be exploited by attackers. This could be in the automation software itself, or in any third-party libraries or components it uses.

5. **Insufficient Logging and Monitoring:** Automation systems may not have adequate logging or monitoring, making it difficult to audit their actions or detect malicious activity.

6. **Chain of Trust Vulnerabilities:** Automated systems often interact with multiple other systems, each of which may have its own vulnerabilities. An attacker who compromises one system in the chain could potentially use it to attack others.

7. **Weak Authentication:** If the automation system does not implement strong authentication, it may be possible for attackers to impersonate legitimate users or systems.

8. **Bypassing of Traditional Controls:** By their nature, automated systems are often able to perform actions more quickly and on a larger scale than humans. This means they can potentially bypass controls that were designed with human users in mind.

9. **Lack of Updates:** Failure to keep automation systems and their underlying software environments up-to-date can leave them vulnerable to known exploits.

To mitigate these risks, it's important to secure automation systems just as rigorously as any other part of an organization's IT infrastructure. This includes hardening the systems, implementing strong access controls, logging and monitoring their activity, regularly updating and patching them, and including them in the organization's overall security planning and risk assessments.