- Data Loss Prevention or Data Leak Prevention
- A rather complicated set of policies, procedures, and controls to prevent loss from sensitive/confidential data from the company
- Keeping everything inside - nobody else needs to see it!

### DLP methods

- The idea is to have some type of engine that performs two functions
	- Classification: identifying what data is considered sensitive
	- Enforcement: blocking and denying any action that could result in data loss
- Manual method (for small companies only)
	- This only goes as far as classification - we can't manually block traffic when a user tries to upload a sensitive document on RapidShare or send it in an email attachment
- On-premises appliance method
	- It'll actually look at traffic, and it can rely on an agent for traffic inspection
	- Knows to look for application-level info such as finding files within HTTP requests
- Cloud service method
	- Much easier to implement if your entire data-storing infrastructure is also in the cloud, preferably with the same provider
- Host agents method
	- Small apps that are installed on every endpoint in order to inspect all operations the user is performing and catch any action that might threaten the confidentiality of sensitive data
	- Very big-brother like, but has to be done when the existence and reputation of the company are at stake
- Client-side implementations are considered the best for protecting against DLP risks because they also address data exfiltration via peripherals such as USB
	- Downside: lots of admin overhead
- On the server side, we'll have to rely on some type of proxy device that can intercept, decrypt, and inspect all outbound connections 
	- Downside: where do you draw the line? Inspecting traffic can become unethical very quickly, and personnel may not be all that comfortable with it
- All DLP products will have a policy server or a management dashboard where all the small details can be configured
	- What to scan
	- What to do when sensitive data is found
	- What to do if something cannot be scanned (a password-protected archive, for instance)
- Some solutions manage the classification part, others can integrate with a content management database
- Examples of DLP solutions:
	- Office 365
	- Digital Guardian
	- McAfee
	- Symantec
	- Druva
	- Cisco

### DLP rules

- Look for:
	- File names
	- File types
	- File content (pattern matching)
	- Can the clipboard be inspected?
- Actions:
	- Block the transfer/connection
	- Notify the user (and IT/management if needed)
	- Quarantine - placing the file in a restricted area, and it can only be freed from there by an admin
		- Mostly done to avoid too many alerts for that one file, especially if it was mistakenly placed somewhere with public access
	- Tombstone - a replacement file instead of the actual sensitive file, usually something generic with a note saying that the original file was replaced due to a policy violation

### DLP discovery and classification

- Classification is crucial for DLP to work properly - rules can't be defined without it
- We have to know exactly what data shouldn't be leaking out
- A number of ways to do it:
	- **File tags** - tagging specific files wherever they're stored, specifying whether they're confidential; ideally this is automated (which relies on another policy)
	- **Dictionary** - searching using keywords or regex patterns for anything that constitutes sensitive data
	- **Policy templates** - basically do everything for us because they're pre-defined in most DLP solutions to match certain regulations if they apply to the org (HIPAA, GDPR, PCI DSS, the whole lot). Load the policy, it already knows everything, so it'll just go ahead and scan your file storage
	- **EDM (Exact Data Matching)** - scanning outgoing data and/or feeds for exact values: SSN's, phone numbers, CC numbers, contact information, really any PII. This doesn't get uploaded to the DLP solution, but instead this data is hashed one way
		- In other words, an SSN or a passport number should not be allowed to leave the company; that info can simply be hashed, and whenever the DLP solution finds one of those bits of data in the outgoing stream, it can calculate a hash from it and compared with a stored hash
		- This way sensitive data isn't even shared with the DLP solution, which makes sense for regulatory requirements
	- **Document matching** - providing samples of specific documents that shouldn't leak: what an invoice looks like, or a CV, or an agreement
		- Works better for humans than for machines - the latter might fail if there are minor changes that make these documents unrecognizable; deviations can even include file formats
- EDM is the most difficult one to implement of all these methods, but it generates the smallest number of false positives
- DLP solutions are not only technical controls against insider threats, but also against human error and unintentional data leakage
	- Anyone can attach a wrong file to an email by mistake, or attach the right file but send it to the wrong person

---

### Exam

Understand different DLP methodologies, rules, and actions. Be able to discuss discovery and classification methodologies as that's the crucial first step before loss prevention can occur.