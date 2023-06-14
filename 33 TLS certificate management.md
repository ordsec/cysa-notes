- This is just about certificate management. Everything that has to do with cert lifecycle, how certs are generated, what formats there are, etc. is within the scope of Sec+
- Digital certs are the #1 method of authenticating or proving one's identity in a trustworthy way. It can be applied to a person, a piece of code, a website, a device, or anything else that needs to prove its digital identity 
- They're also used to facilitate digital signatures and assist with encryption

### Trust

- In order for anyone to be able to trust a digital cert assigned to anyone or anything, it must be signed by a trusted entity
- A **Certificate Authority (CA)** is such an entity - it issues certificates
- The CA has to be trusted by all parties involved
- Anyone can become a CA and start issuing/signing certificates, but they can't be trusted because they're basically from a self-proclaimed CA, which can easily be malicious
- **Trust has to start somewhere** - and that's what **root CA's** are for
- In the context of an OS, the root CA is hardcoded into the TPM, which exists on the motherboard. This root CA can be trusted by default, so anything it signs can also be trusted via **transitive trust** (A trusts B, B trusts C, so A trusts C)
- In web PKI, the chain of trust is: root CA -> intermediate CA (could be several) -> end entity
	- The root CA is kept offline for security purposes - if compromised, a huge chunk (if not all) of the internet becomes unsafe
- Use cases: 
	- Identity verification
	- Non-repudiation (only the entity that has signed a message could've signed that message - they can't deny that they have done so)
	- Electronic signatures: messages, code
	- Encryption for PKI, securing data sent over an encrypted connection by authenticating the two parties involved and providing a secure environment for encryption method negotiation
- Some apps maintain a separate certificate store just for themselves: VPN clients, some browsers
- [Certificate Transparency Framework](https://certificate.transparency.dev/): a group of companies agreed to publish their own CA logs so that everybody can see what domains and subdomains they have issued certs for
- Certs can be viewed with the following tools:
	- `certmgr` on Windows
	- `sigcheck` from Sysinternals
		- `.\sigcheck.exe -tv` will show all installed certs 

### Special certificate types

- **SAN: Subject Alternative Name**
	- Used with other domains closely related to the parent domain for which the cert was first issued
	- Good for situations when a website is hosted on several different IP addresses
	- When you have a cert for your business, but you don't want to purchase another cert for every resource that you're hosting
- **Wildcard certificate**
	- More broad, covers a larger spectrum of resources under the same domain
	- A cert issued for `*.xyz.com` will cover all third-level domains (subdomains) for xyz.com
		- But it won't cover 4th-level domains
	- Since it can apply to a lot of completely unrelated entities (even though they're in the same domain), forensics and logging can become pretty difficult

### Certificate management tasks

- The day-to-day for cybersecurity analysts
- Installing certificates on company devices and internal systems
- Updating/replacing/renewing certificates
- Validating root certificates in the event of compromise
- Installing/renewing/revoking user and machine certs
	- Treat code-signing certs with special care - if compromised, they can be used to push malware with properly signed code
		- [This can happen](https://venturebeat.com/mobile/software-pirates-hijack-apples-enterprise-certificates-to-put-hacked-apps-on-iphones/)
- Managing self-signed certs
	- Very convenient, but should only ever be used internally - never in a production environment
	- Be careful when accepting and installing captive portal certs - they are often self-signed, and they give the owner permission to inspect your traffic. Anybody can create a captive portal
- Must revoke untrusted certs ASAP, make sure systems abide by cert revocation policies
- Cert status can be checked:
	- By looking at the CRL (should be periodically downloaded and checked)
		- But this isn't a great method because various delays can occur
	- By using **OCSP (Online Certificate Status Protocol)** to check online, and very quickly, whether a cert is valid
		- No need to download the entire CRL
		- Run a simple query, get an immediate response
		- This can involve plenty of traffic, so it's possible to cache these responses on the web server, which is referred to as **OCSP stapling**
- Utilities for cert management
	- `openssl` on Linux
	- `certutil` on Windows

##### From Sybex: cert management
- Keep all related secrets such as private keys very secure
- Come up with a cert-specific IR plan in case secrets get compromised: who to contact, how to replace secrets ASAP

### SSL & TLS

- Widely used in HTTPS for securing website traffic
- Pretty much any other protocol is supported since TLS is on layer 4
- To implement TLS, the server needs to have a valid certificate
- The latest version is TLS 1.3, released in August 2018. Only ever use this or 1.2, take precautions against downgrade attacks and SSL stripping
	- So if the client doesn't support these two versions, sucks for them
	- TLS 1.3 directly prevents any sort of downgrade

---

### Exam

Understand cert management tasks, know the tools listed above. Know what the CT framework is, remember special cert types (SAN/wildcard and what the difference is)