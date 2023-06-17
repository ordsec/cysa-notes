- Email may be one single communication channel, but it's so widespread that there are a lot of attack vectors, which we need to know how to deal with

### Phishing

- An attack that's unique in how it can be executed without relying on any technology at all
- The human link is the weakest link - it's a social engineering attack
- Tricking the receiver of a malicious email into thinking that it comes from a legitimate source and providing personal data to the attacker as a result
- **Embedded links** are often used as part of a phishing scam because many users don't check where the link leads and just click
	- The link can easily differ from the text in the email, and it can include all sorts of malicious crap or lead to a malicious site
	- Email security tools scan for these links and block a lot of emails including them
	- Others can be recognized via URL analysis - see 29
- Subcategories:
	- Spear phishing - targeted at a specific person or group of people 
	- Whaling - targeted at high-level management, C-suite, etc.
	- Pharming - anything that contains a malicious link that leads to a form which looks like a legitimate login form. Users enter their logins and passwords, attackers capture it, you know the drill
- Email **forwarding** is not really an attack, but rather a technique of crafting an email subject or body to make them look like they belong to a chain of emails and replies
	- Tricking someone into thinking they were just added to a group email
	- A type of pretexting where all the forwarded/quoted emails contain names and info that look legitimate, lots of it can be discovered via OSINT
	- Utilizing consensus and authority to elicit sensitive info
- Business Email Compromise (BEC)
	- Compromising a real email account in the company so that everything coming from that address looks perfectly legitimate

### Email headers

- Contain lots of useful metadata about an email message
- Sender, recipient, transit servers that handled the delivery of the email
- If you know what to look for, this info is enough to determine whether an email is a bad one
- This process is largely automated, but we can analyze it ourselves and look for:
	- Display from: the sender's email address, what you see in your email client under "From". Can be spoofed
	- Envelope from aka return-path: the return address, where replies are going to go, used by the receiving server if it needs to reject the email
	- Received from/by: servers that process the email in transit
	- Delivered-to: the recipient
	- Date and time
	- Return-path authentication-results: related to the sender's authentication method
- Some tools for analysis:
	- [Microsoft Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/OutboundSMTP/input) - checks inbound/outbound connections, can look at whether an address passes validity checks or whether it's blacklisted
	- [Mail Header Analyzer](https://mha.azurewebsites.net/) - paste raw email data, view all headers in a pretty format
	- [Email Header Analyzer by MX Toolbox](https://mxtoolbox.com/EmailHeaders.aspx)

### Malicious payloads

- The payload isn't just the text of the email, it's also what's in the attachments
	- These attachments can easily be malicious, all the way to consisting of executable code 
- Malicious scripts can be embedded in the HTML code in message body - sometimes they can be executed without any user interaction
	- Email clients often include preview functionality that just does it for you
- Malicious links - what phishing primarily relies on, more likely to work than an attachment
- **Email signature block** (the text signature - what's included in every email)
	- Can help identify phishing attacks, but more sophisticated attackers will copy legitimate signatures
	- Can contain embedded links and images, also dangerous elements that can be part of an attack or tell attackers if the email was opened
	- Worth looking at for analysis: what if it isn't there when it should be? Or it contains something that doesn't belong? Or it contains wrong contact info? This could be an indicator of a malicious email where the attacker tried to craft the signature to make it look real

>**SPF**

**Sender Policy Framework**

- We can't trust all email users to validate incoming email headers, check email contents, and harden email clients
- Email security can be assigned to the server so that bad stuff can be caught before the email reaches the recipient
- **SPF is a very misleading term** - it doesn't quite represent what this technology does
- An SPF entry is published in a company's DNS TXT record
	- Only one SPF entry per DNS entry
	- It's a list of hosts that are known, valid email servers that belong to the company, authorized to send emails on behalf of that company
	- Email servers that are responsible for the company's domain
- Someone with an email server on the receiving side can configure SPF checking
	- A return-path header is used to validate the SPF record, which is looked up in the sender's DNS field
- What the recipient can do with emails received from servers that are not on the list:
	- `-all` (rejecting)
	- `~all` (flagging)
	- `+all` (accepting)
- With SPF enabled on the receiving side, it becomes much more difficult for the attacker to spoof the email and make it look like it came from the CEO of Google
- SPF checks can be seen as part of email headers

>**DKIM**

**Domain Keys Identified Mail**

- Again with the poor naming...
- Replaces or augments SPF using PKI! Cryptography rulez
- A company publishes their public key in its DNS server's TXT record - just like SPF
- For any sent messages, specific header fields will be hashed
- That hash is signed with the **private** key of the server - which is not unlike a digital signature
- This digital signature is attached to the email that's being delivered
- On the receiving side, this digital signature can be decrypted, checked, and validated using the sender's public key. The decrypted value from the hash has to match what's in the sender's TXT record
- Makes any kind of spoofing even more difficult

>**DMARC**

**Domain-Based Message Authentication, Reporting, and Conformance**

- Naming keeps getting *better*...
- This isn't another security tool, but rather a **policy** for implementing SPF & DKIM
- Also published as a DNS record
- The domain owner can create a custom DMARC policy, which is not unlike an authentication procedure telling the receiver of the email the following things:
	- What technologies are in use in terms of the security mechanism: SPF or DKIM
	- How to validate the sender
	- What to do if any of the checks fail, providing a reporting method
		- A failure doesn't mean something malicious is happening - it could be a different type of error

>**S/MIME**

**Secure/Multipurpose Internet Mail Extensions**

- SPF/DKIM and DMARC are great to have for security, but they only help against **impersonation** - so this is about **integrity**
	- Impersonation attacks are becoming increasingly common, often involving an email purporting to be from a trusted coworker or manager. The recipient is often asked to perform a certain action, and the attacker leverages social engineering principles of familiarity, authority, or urgency to try and get the victim to do what the attacker wants
- But how do we ensure **confidentiality** of what's being sent?
- Enter S/MIME - it digitally signs your emails and encrypts them
- If someone wants to send a secure email to us:
	- They'll need a digital certificate (not mandatory for the receiver unless encryption is needed for the reply as well)
	- Our email client needs to be S/MIME-aware 
	- Their email content is hashed, and the hash is encrypted with **their private key** - this is the **digital signature** and it takes care of integrity and non-repudiation
	- Their email content is also encrypted with **our public key** - this ensures confidentiality
	- The message is sent to us **in a blank email with an S/MIME attachment**
- What we do now as the receiver:
	- Decrypt the message using **our private key**
	- Decrypt the hash of the message with **their public key** to validate the digital signature - if there's a match, then the integrity has not been compromised
		- And it proves the authenticity of the message - as in, this message could only have been sent by the one sender

### Email logs

- Always remember to inspect email logs
- Pay attention to repeated errors in terms of sending emails - that means either a bad configuration or some kind of DoS happening
- Email servers communicate in a request-response manner
	- SMTP is very similar to HTTP
	- Responses are identified by a special code - sometimes similar to HTTP

### [SMTP response codes](https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes)

- **Exam**: be familiar with categories below

The first digit denotes whether the response is good, bad, or incomplete:

- **2yz** (Positive Completion Reply): The requested action has been successfully completed.
- **3yz** (Positive Intermediate Reply): The command has been accepted, but the requested action is being held in abeyance, pending receipt of further information.
- **4yz** (Transient Negative Completion Reply): The command was not accepted, and the requested action did not occur. However, the error condition is temporary, and the action may be requested again.
- **5yz** (Permanent Negative Completion Reply): The command was not accepted and the requested action did not occur. The SMTP client SHOULD NOT repeat the exact request (in the same sequence).

The second digit encodes responses in specific categories:

- **x0z** (Syntax): These replies refer to syntax errors, syntactically correct commands that do not fit any functional category, and unimplemented or superfluous commands.
- **x1z** (Information): These are replies to requests for information.
- **x2z** (Connections): These are replies referring to the transmission channel.
- **x3z**: Unspecified.
- **x4z**: Unspecified.
- **x5z** (Mail system): These replies indicate the status of the receiver mail system.

---

### Exam

Understand what phishing is and how it can be dealt with. Discuss the importance of email headers, be familiar with secure email technologies: SPF, DKIM, DMARC on the integrity side, S/MIME on the confidentiality side. Know the importance of email logging and be familiar with SMTP response codes.