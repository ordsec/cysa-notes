- Every org has a lot of applications, VMs, internal services, cloud services. There are also a lot of users, and each one needs to have permissions assigned
- How do we keep track of all this in an organized fashion?
- The answer is **IAM: Identity and Access Management**

### What it is

- IAM is a framework that basically creates order out of the huge mess of user accounts, devices, and permissions inside of an org
- IAM also enables workflows: how do we add new users? How do we offboard them? How do we audit them? 
- Applies to on-prem and cloud environments, and IAM is especially important for cloud environments because even more precision and granularity of control is needed

### Account types

- Not all accounts are born equal!
- **User accounts**
	- Personnel accounts
	- They're the most risky - regular users can be pretty careless with their credentials/permissions
- **Endpoint accounts**
	- Devices on the network
	- Can be managed through MAC addresses, but in an environment with a lot of devices (including personal) and a low risk appetite, a better idea is to use certificates for devices. This means a lot of administrative overhead, though
- **Server accounts**
	- Especially mission-critical ones
	- Need certificates to prove that users and other devices can trust them
-  **Software accounts**
	- Can be identified by digital certs - these prove that the software comes from a legitimate vendor; certs also uniquely identify each software component
	- This is a good solution when you need a more strict control over what apps your users can install and run
	- Code signing - this is why the OS will occasionally ask the user whether they want to continue with the installation of a piece of software as its legitimacy can't be established (no cert)
- **Roles**
	- Identities, sets of permissions that apply to multiple assets
	- Instead of assigning it to an individual user or app, we can define a role that characterizes what a group of users/apps should be allowed to do
		- Apply permissions to the role, then assign it to entities

### IAM system responsibilities

- Store and keep track of accounts
- Onboarding and offboarding (creating and disabling/deleting accounts)
	- Applies to people who have a digital identity in the org
- Day-to-day management tasks
	- Password resets, support tickets, cert renewals, updating permissions and group memberships
	- Auditing - the IAM system has to keep track of who does what, especially for privileged users as they have a lot of power
- Scanning for threats - ideally
	- Mainly identity-based threats
		- Compromise
		- Account sharing
		- Weak passwords
		- Brute force attempts
- Maintaining compliance - especially with regulations that have to do with personal data

### Two big problems

- These are the two problematic account types:
	- `root`/Administrator users - lots of privileges (you could even say it's too many)
		- Keep an eye on these, log everything they do
		- Great power, great responsibility
	- Shared accounts
		- Whether it's guest accounts used by more than 1 person or a "let me just use your employee account real quick" type of situation
		- No identity to speak of, accounting is not possible - no way to tie a specific person to actions performed with a shared account
			- Loss of non-repudiation as well
		- Just don't allow these

### Password policies

- A password policy is a guideline that tells users how to protect their credentials
- Account and password compromise is one of the most widely used attack vectors
	- Users can be careless not only about how they choose their passwords, but also how they store them
- These policies regulate password length, complexity, expiration
- [NIST SP 800-63B: Digital Identity Guidelines](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-63b.pdf) - actually advises against some of the more common password policies
	- Complexity restrictions: users should be able to use any character, and it's a poor security practice to list what characters are allowed because it helps attackers narrow it down
	- Password expiration: users should decide for themselves how often to change their passwords. Making password changes mandatory leads to poor security practices regarding complexity/entropy (i.e. most people will change from password123 to password1234 as a very rough example)
	- No more password hints or security questions - these help attackers more than they help users
		- [Anatomy of a password disaster](https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/) - an article about the huge Adobe breach that happened because of password hints
- Password policies can be enforced by the OS - we don't have to rely solely on the users
	- Group policies and such
	- Does not protect from password reuse and poor complexity
- Some things can't be enforced - so do a good job *recommending* those things
	- Aspects such as password reuse and using password managers can only be driven home with proper user awareness training

### Mitigating password reuse

- One person can have hundreds of online accounts
- Managing them is difficult, remembering a separate password for each is impossible
- Most people would therefore reuse passwords - it's a very logical and natural thing to do, unfortunately
	- Or use very similar passwords
- There are technologies that can help with this
- **SSO**: authenticate once, gain broad access to many resources
	- Seen in Windows domain settings (AD) - Kerberos. Once the user is authenticated inside of the domain, they can access everything they're authorized for
	- Pro: one password to remember, very convenient
	- Con: it's just one password, and passwords are inherently insecure. Convenience reduces security
- **MFA**: not just a user/pass pair, but also a key card, a certificate, a retina scan, gait analysis, and so on. 
	- An extra layer of security on top of your (supposedly strong) password. Relies on at least two of the following factors:
		- Something you know
		- Something you have
		- Something you are
		- Something you can do
		- Someone you know
		- Where you are
		- Behaviour analysis (happens after authentication as well) - is the user breaking their routine in any way? Time of day, location, device, user-agent, etc.
	- A password and a PIN is not MFA - make sure you choose from different factor categories

### Privilege management and access control

- Privileges are tied to the authorization function
- Now that we know who you are, what exactly are you allowed to do?
- Policies that describe technical controls that enforce what an account can do
	- **Least privilege**: a user shouldn't be able to do or access anything outside of what they need to do their work
	- **Separation of duties**: balancing responsibilities against the risk of insider threats; not giving too much power to a single person to perform a certain task, but rather splitting the task between two people
		- Lower chance that someone may act out of malicious intent

##### Implementations of access control
- **DAC - Discretionary Access Control**
	- Implemented on most operating systems
	- The creator of a resource is its initial owner
	- The owner can assign access to others **at the owner's discretion**
	- Read/write/execute permissions for files/folders/drives/etc.
	- Downside: centralized permission management becomes very difficult to implement
		- Separation of duties is also difficult to implement as it can be bypassed by the owner
- **MAC - Mandatory Access Control**
	- **Non-discretionary** - enforced by the system
	- Focuses on clearance levels and labels 
	- Each object receives a clearance level - and it can only be accessed by a user at or above that level
	- **Confidential < Secret < Top Secret**
	- Compartments can be used as well: subjects can only access objects within their respective compartment
	- Widely used in the military because it's centrally managed and because everything follows the chain of command
	- Examples: SELinux, AppArmor implement MAC on Linux systems
- **RBAC - Role-Based Access Control**
	- Discretion from DAC is transferred to the admins
	- Privileges are assigned for groups based on users' roles
	- Members of a group automatically inherit all appropriate privileges
	- Group memberships are governed by admins and cannot be changed by regular users
	- Example: group membership on Windows/Linux
	- RBAC permissions overwrite individual permissions!
- **ABAC - Attribute-Based Access Control**
	- The most granular type of access control
	- Based on multiple subject and object attributes, or compiled sets of attributes
	- Example: ABAC can be implemented in a NGFW that allows access based on **attributes** such as IP address, what kind of user you are, which group you belong to, your geolocation, the protocol you're using, and your user-agent type

### Directory services and federation

- So how do we organize all this info? Users, identities, permissions, etc.
- **Directory services**
	- Basically an IAM database 
	- AD stores information about objects and manages authentication and authorization by looking at users, their rules, and any policies that may be in place, and giving a yes/no verdict when a user tries to authenticate and access something
	- Can be queried (it's a AAA database after all)
	- Protocols are usually RADIUS, TACACS+, LDAP
	- Others include OpenLDAP, Apache DS, OpenDS, RedHat Directory
- **Federation**
	- Extending authentication across more than one company/service via SSO
	- Can interconnect IAM services
	- Used when you have people in one org that needs access to another org's systems, and they need to do it using the same account
		- Using a Windows domain account to access a cloud app
	- **Federation == trust**
	- The trust is mutual between your own domain and an outside service
	- SSO experience is provided based on that trust, without the outside service requiring a copy of your local directory
		- The application we're trying to access is the **Service Provider (SP)** (we're trying to access a service, and the application provides that service)
		- The entity that validates our access based on our SSO credentials is the **Identity Provider (IdP)**
		- The SP has to trust the IdP
	- So federation is pretty much the same as SSO
		- One difference is that with SSO, once the user authenticates, a unique hash will be shared between the two systems as a means to authenticate transparently, i.e. only two parties are involved
		- Whereas with federation, the entire authentication process is handled by the IdP; other systems trust that IdP with handling authentication **on their behalf**
	- Federation caveat: password reset/recovery may no longer be allowed because the IdP, being a 3rd party, doesn't actually know anything about the user's credentials aside from whether they're valid or not. Users and admins have to be aware of this through user training

### SAML, OAuth, OpenID

- We deal with these every day, for instance when signing into an app using our Google credentials
	- The app we're accessing is the SP
	- Google (or Apple, or FB) is the IdP
- **SAML - Security Assertion Markup Language**
	- XML-based framework
	- Used for exchanging security information: authentication, permissions, attributes
	- Allows SSO and federation
	- Relies on a trust relationship between the SP and the IdP
	- SAML info is communicated based on **assertions**. An assertion is basically a message of a certain type
		- Authentication assertions (used to validate the actual user's identity)
		- Attribute assertions (to provide more info about the user, such as group membership)
		- Authorization assertions (the user's level of access, what they can do)
			- Example: when an app asks you if it can access your profile, post on your behalf, etc.
- **OAuth - Open Authorization**
	- Standard protocol used for delegated **authorization** (it's not about who you are, it's about what you can do)
	- A 3rd-party app (aka the client) accesses resources from a resource server (such as an API) on behalf of a user (aka the resource owner)
	- The user grants the client an access token, which the client can use to request stuff from the resource server
	- The token is issued by an authorization server, which verifies the identity and consent of the user
	- Most recent version: OAuth 2.0, or OAuth2
		- Defines four roles: resource owner, client, resource server, authorization server
		- Defines four grant types: authorization code, implicit, resource owner password credentials, client credentials
		- These are used for different scenarios
- **OpenID Connect**
	- An extention of OAuth2 that adds an identity layer to the authorization framework
	- This is about **authentication**
	- A client can verify the identity of the user and obtain basic profile info
	- User logs into an IdP (such as Google or FB) using OpenID Connect
	- The IdP returns an ID token (which is a JWT) to the client - it contains info about the user such as their name, email, and profile pic
	- The client can request an access token and a refresh token from the IdP to be used to access other resources
- **OAuth2 vs OpenID Connect**
	- The main difference is that OAuth is concerned with authorization and OpenID is **also** concerned with authentication
	- Authorization: granting access to resources
	- Authentication: verifying the identity of a user
	- OAuth does not provide a standard way to obtain user information - OpenID does
	- OAuth relies on access tokens (strings that can only be validated by the resource owner) while OpenID relies on ID tokens which are self-contained and can be validated by the client
	- OAuth is more flexible and can be used for various types of applications - OpenID is more specific and **used for SSO and social logins**
- Once again...
	- ==OpenID and SAML are industry standards for federated authentication==
	- ==OAuth controls authorization to a protected resource such as an application or a set of files==
	- ==OAuth can be used simultaneously with OpenID or SAML==
- Example situations:
	- OAuth 2.0: you've signed up for a new app and agreed to let it automatically source new contacts via Facebook or your phone contacts - this is secure delegated access, i.e. the app takes actions to access resources from a different server on behalf of the user
	- OpenID Connect: you've used your Google account to sign into YouTube, or your Facebook account to sign into an online shopping cart. Google or Facebook serve as IdP's here and access other websites without the need for a dedicated set of creds
	- SAML: you've logged into your company's intranet, and this set of creds allows you to access numerous additional services such as Salesforce, Box, Workday, etc. - without having to authenticate into each. An IdP is also involved here - this is the service where you sign in.
- **This is tricky stuff - study it!**

### IAM monitoring and logging

- This is about **accounting** (third A in AAA) - logging all user actions
	- When and how they logged in
	- How long was the session
	- What did they access
	- When did they change something
	- etc.
	- Very useful for correlation
	- Needs lots of disk space and time to analyse
- We need to quickly react to unauthorized/compromised accounts and other incidents
- Manual review: reviewing accounts and permissions by hand
- Reviews should be performed periodically and when users change roles/departments, or leave - and when new users arrive
- Privilege creep: when users changing departments and moving up in the company "just so happen to" retain too many privileges - and over time they pretty much attain superuser powers
	- No malicious intent here, just a lack of proper accounting and auditing

---

### Exam

Be able to discuss all aspects of IAM: account types, password policies, federation/SSO, access control models, accounting, etc. Know the difference between OAuth/OpenID/SAML and be able to apply them to given scenarios. 