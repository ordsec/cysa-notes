- The focus is on protecting your hardware
- There is malware that targets hardware - we have to defend against it

### Supply chain assessment

- A lot of companies rely on off-the-shelf products - so they have to trust that the hardware didn't become compromised somewhere along the way
- We can assume that whatever we buy is safe, but it may not be enough for some orgs and especially governments
	- They always have to make sure that hardware and software that comes in doesn't include something undesirable: backdoors, spyware, etc.
- [A pretty horrific example of supply chain compromise](https://www.zdnet.com/article/microsoft-fireeye-confirm-solarwinds-supply-chain-attack/)
- Vendor assessment and due diligence. Check the following at the very least:
	- Vendor's risk management protocols with regards to development and shipping
	- How well the products are supported during their lifetime - can we expect security patches down the line?
	- The presence of forensic assistance in incident investigations if necessary
	- Historic reliability - have they been around for a while? Have they been good that whole time?
- Just because you trust a vendor, it doesn't mean you can trust the second-hand/grey market
	- It's a totally different supply chain, and it has nothing to do with the official one
- **Trusted foundry**
	- Set up by the US DoD
	- A list of accredited suppliers who have proven that they have a secure supply chain, all the way down to manufacturing
	- Established to keep out vendors that can't or won't prove this

### Hardware root of trust

- Aka Trust Anchor
- Securing hardware in our machines using a secure subsystem providing **attestation**
- Attestation means proving that something is true 
- Within our computer, attestation is provided by a device that, whenever the computer connects to a network, digitally signs a report that says that the machine's integrity hasn't been compromised
	- Boot metrics
	- OS files
- Physically, RoT is implemented as a **TPM** chip
	- Hardwired into the motherboard or a separate chip
	- Stores everything related to cryptographic operations
	- Digital certificates, keys 
	- Hardcoded, unchangeable private key aka **endorsement key**
		- Used to generate any other keys that can be used to sign emails, encrypt disks, etc.
		- BitLocker's key is derived from the endorsement key
	- Provides remote attestation allowing hardware and software configurations to be verified
	- Keeps cryptographic content out of reach
	- Tamper-proof - will self-destruct if messed with
		- PUF (Physical Unclonable Function) - unique signature made up of distinct features of the device the TPM is running on; it's based on the unique features of a microprocessor that are created when it is manufactured and are not intentionally created or replicated
		- Engages the crypto-shredding function on all TPM contents in the event of physical tampering
	- Allows a very limited amount of interaction, namely erasing it
	- On Windows, `tpm.msc` is the interface

### HSM

- Alternative to TPM - basically a removable one
- Can store the exact same kind of information
- Comes in the form of a PCI-E card, which can be removed and kept somewhere safe - or even safer (otherwise, keys have to be kept in key escrow)
- Useful for key retention when rebuilding the infrastructure
- Example solutions:
	- [Entrust nShield](https://www.entrust.com/digital-security/hsm/products/nshield-hsms)
	- [Thales](https://cpl.thalesgroup.com/encryption/hardware-security-modules)
- Enabled for cloud environments since a lot of customers use a shared infrastructure
	- We need a secure storage space where everybody can access their cryptographic information
	- Physical HSM's can be rented 
	- Otherwise you can use a cryptographic **vault**, which can be software-based or HSM-based for private clouds

### Trusted firmware

- Firmware always runs first and with the highest possible privilege level
- Performs hardware checks and initialization, allows user interaction
- If exploited, everything is lost
- **BIOS**
	- Motherboard firmware
	- Initializes a computer when switched on
	- No security built in, though
- **UEFI** (Unified Extensible Firmware Interface)
	- BIOS with security (and other cool stuff)
	- Now the standard for all PC's
	- Required for TPM
	- **Secure boot**: uses a digital cert and digital signatures from OS vendors
		- Ensures boot integrity - can't trick the computer into booting a malicious OS of some sort, for instance from a CD or USB drive
	- **Measured boot and attestation**: using the TPM chip's key to hash and sign the system's state
		- The OS kernel gathers all this information and sends it to an external server (like a NAC server) when a PC boots up and connects to the network
		- NAC server checks it and decides whether to allow access to the network depending on the security report
		- Allows for comparison vs known good states
		- Detecting IOC's before they can even manifest themselves
- **Trusted firmware updates**
	- Trusted firmware is usually described in the context of **Trusted Execution Environments (TEE's)**
		- Signed by a chip vendor or other trusted party
		- Used to access keys to help control access to hardware
		- TEE's like those used by ARM processors use these technologies to protect the hardware by preventing unsigned code from using privileged features
	- Trusted/measured boot ensures OS integrity
	- But what if the firmware itself gets replaced with something malicious?
	- Like any piece of code, it can be digitally signed - but the CPU needs to know which signature is valid
	- You can hardcode these keys into the CPU!
	- **Intel Boot Guard** takes public keys for all valid firmware and fuses them into the device, so they cannot be changed. No other firmware will be accepted as a result of this
	- This is extremely restrictive, and that's the whole point
- **eFuse**
	- A hardware fuse that's meant to lock programs in place so they cannot be changed
	- As firmware is updated, the device actually blows a microscopic electrical fuse every single time 
	- The number of blown fuses has to match what's expected
	- If different, that means the integrity has been compromised
	- One of the reason why jailbreaking devices is difficult nowadays - they rely on this type of hardware security that can't be bypassed so easily

### SED

- Hard drives or SSD's
- Offloading crypto operations to the drive itself
- OS resources are not used in this case
- Uses a **Media Encryption Key (MEK)**
- The MEK is stored securely using a **Key Encryption Key (KEK)**
	- This one is generated from the user's password that's entered at the OS level
	- It's not the actual `password123` you have entered ;-)
- SED keys are not stored in TPM - it's assumed that you might want to move the drive to another system and still be able to decrypt it (you can't remove the TPM and move it along with the drive)

### Secure processing

- A lot of exploits are designed to access unauthorized areas in the memory and exfiltrate data from there
- This is **data in use** - most of the time it's unencrypted
- Secure processing counteracts that
- **Trusted execution**
	- Uses the TPM and secure boot attestation to make sure only the valid OS is running and no malicious code gets executed at boot]
- **Secure enclave**
	- Common in modern Apple mobile devices
	- A secure area or partition, kind of like a container (but not a Docker container), created by a trusted process, where it stores the cryptographic keys, if the OS is trusted
	- Designed to remain secure even if the OS is compromised
	- Generates an encryption key at boot, pairs it with the user ID to encrypt, validate, and use the secure enclave's portion of the system memory
	- Handles FaceID, allowing authentication to be handled in a secure partition
	- Makes BOF attacks almost impossible - you can't overwrite any information from the secure enclave
- **Processor security extensions**
	- Low-level CPU instructions that enable secure processing
	- **SME** by AMD (Secure Memory Encryption)
	- **TXT** by Intel (Trusted eXecution Technology)
	- **SGE** by Intel (Software Guard Extensions)
- **Atomic execution**
	- This is a real interesting one - refers to **indivisible operations**
	- They're executed entirely or not at all - cannot be divided, just like an atom
	- Protects against race conditions, BOF, TOC/TOU
- **Bus encryption**
	- Securing data in transit, but not on the network
	- This is about the internal circuitry of the PC - between PCI cards, from the GPU to the screen via HDMI cable, between internal/external drives, etc.
	- Makes sure that the device at the other end of the bus is trusted
	- Used for encrypted HDMI links for DRM (Digital Rights Management) for protecting end-user content 
	- Transmitting encrypted information and encryption keys over the same link is security by obscurity - there's no way to hide the key somewhere else, otherwise the dumb TV on the other end of the HDMI cable will never be able to show the actual content
- **Anti-tamper**
	- Comes in many varieties from mechanical means to electronic detection methods
	- Tamper-proofing processors: encasing electronics or otherwise securing them, 

---

### Exam

Know and be able to discuss all concepts from above, recognize them from scenarios, apply them correctly. 