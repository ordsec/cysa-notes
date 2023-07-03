- How do we code an application in a secure manner?

### User input - the great danger

- If we managed to fix this in every single app we develop, we'd put a lot of cybersecurity analysts out of work...
- A lot of problems appear when you ask for user input and then trust it
- Input is the root of all evil - people out there are creative, and they will deliberately look for ways to mess with your app! (That's what attackers do for a living, after all.)
- You may think you don't have that much input in your application - but actually you do
- Even a static webpage has to accept HTTP requests, which are a kind of input
- Any kind of uploads, text fields, etc. ask for user input
- All of these are potential vulnerabilities!

### OMG - what do we do?

- Make clear decisions and describe expectations on what types of input you will be receiving
- Then make 100% sure that all input you receive is thoroughly checked and validated
- Yes, there are myriads of different types of input based on the nature of the app we're developing
	- Use guidance!
	- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
		- **Syntactic** validation: enforcing correct syntax (if it doesn't look like an SSN, it's not an SSN)
		- **Semantic** validation: enforcing correctness of values in the specific context (start date should be before end date, price should be within a certain range, can't have negative prices, can't have birth dates in the future, and so on)
		- Allow lists and block lists: it's easier to specify what's allowed and block everything else (sorta like for apps being installed in an enterprise environment)
		- Using regex
		- Sanitising HTML
		- And much, much more...
- Find every single point of entry in your app
	- All interfaces, all potential inputs, API endpoints
	- That includes third-party connections! Attackers can craft requests, so we have to validate those too.
	- Every single place where user input is accepted
- **NEVER** stop at client-side validation - this can be easily circumvented by an attacker (by crafting requests that come from the browser)
	- Use AJAX and send data to the server for validation upon entry
	- More work, but more secure too
- Check ranges, file sizes, formats, disallow anything that can be exploited or doesn't apply to the overall context
- Use a whitelist approach - implicit block
- Validate requests, validate everything in the address bar (avoiding IDOR, broken access control, SQLi, traversal, etc.), think of all ways nefarious things can be encoded (URL encoding, base64, null bytes, etc.)
- For web apps: keep the entire OWASP Top 10 in mind at every step of the way because a lot of it has to do with input validation
- Use **secure coding policies** as a foundation for secure development practices and standards

### Risk assessment

- Understanding what risks the application faces, how to prioritize remediation of those issues
- Continuous assessment is recommended at every stage of SDLC
- Regularly scheduled testing

### Output encoding

- What the app returns to the user
- Only display what's safe to display - don't give up anything unnecessary or unsafe
- Malicious output usually happens as a result of successfully exploiting input
	- But what if we can still prevent the app from showing what shouldn't be shown, even in this case?
- Don't process any dynamic code (that's not already in the app) when showing a page
	- XSS mitigation
	- Just display it! The world will see a failed XSS attempt
- In other words, remove any executable code from any output

### Communication security

- Securing data in transit
- Any transfer of data between the user and the app has to be performed securely
- Encypt it! TLS comes to mind
	- Relies on public key cryptography
	- Allows us to authenticate the integrity of the data and the identity of parties involved
	- Making sure app returns data to a legit user and not to an impersonator
- Valid certificates
- Do not allow HTTPS to fall back to HTTP (downgrade/SSL stripping)
	- No option to choose using HTTP
	- This includes cookies/JS/styles
- Specify encryption algorithms and key lengths for HTTPS (otherwise they're negotiated and a downgrade attack is possible)

### Session management

- A session is a random ID assigned to a user and stored on the server the first time a user communicates with that server (and the first time they authenticate)
- When sending subsequent requests, the session ID is included so you don't have to log in again
	- Server sees ID and knows that it's you
	- This is done to deal with the fact that HTTP is a stateless protocol
	- Your navigation history, shopping cart, and anything else that has to be persisted is stored in a similar fashion
- There are things to consider on the coding side
- How do we generate these session ID's? 
	- It should be done on a trusted system that's separate from the web app
	- It should use good algorithms - otherwise there's a risk of guessing these ID's
- Treat session ID's as passwords! They can't be guessable and they can't be visible.
	- In effect, they are a password - just not the one that the user enters
	- Don't include them in the URL
	- Don't make them visible anywhere in the app
- Sessions should expire, at which point the app requires that you authenticate again
	- Then longer it's valid, the higher the risk of compromise
- Different session ID's for different levels of privileges
	- Authenticate as a user, but if you have an admin login, it has to be a different session
	- Set privileged sessions to expire more often
- Implement logout, even if nobody uses it
	- It has to completely invalidate and destroy all session information on the back end
- Allow multiple logins? How do we handle that? Or should we just invalidate all other sessions once a new one is created?
- Secure your cookies using `Secure` and `HttpOnly` attributes
	- **Exam**: know what these do!
		- `Secure`: cookie only sent to the server via an encrypted request over HTTPS - no plaintext sessions!
		- `HttpOnly`: makes the cookie inaccessible to JS code (`Document.cookie` can't be used)
			- No malicious JS code injected in the page can read cookie info

### Authentication

- The process of proving who you are
- Require it for access to any resource or process that has to be protected
- 2FA, MFA
	- Great way to properly secure login credentials
- Centrally managed, separate system to store credentials so that if the app is compromised, credentials won't be
- Fail gracefully - don't give up info
	- That includes not saying whether the username or the password is wrong
	- "Could not authenticate - check your credentials"
- Detect failed logins, have account lockouts
- External systems - don't accept unauthenticated requests from any of them
- Hash and salt passwords for storage
	- No sending passwords in cleartext
	- Hash while in transit
- Password reset questions shouldn't rely on info that can be obtained via OSINT
- Password policies - at least suggest, if not require, that passwords are changed periodically
- Password complexity: good number of minimum characters, caps/numbers/symbols required for better entropy
- Password history - **exam**! Can't use previous passwords when resetting
- Report last/current login(s) to the user, keep track of geolocation when logging in
	- Complexity can go up exponentially, so be clear on what functionality you're willing and not willing to implement
	- Perhaps limit simultaneous logins? Or have a setting that can be enabled
- No default credentials! 
- Reauthenticate for critical operations (important settings changes, money-related operations, etc.)
- 3rd party code - make sure it matches your security requirements and standards

### Authorization and access control

- Authorization: what actions are you allowed to perform as an authenticated user? What are you allowed to access?
	- Prove who you are, have your rights and privileges bestowed upon you
- **Least privilege** - always! Whether for humans or other machines
	- Don't allow them to do more than they need
- Make sure your code always checks whether a user is authorized to perform a certain action
	- Are they logged in? Does their session exist?
	- Applies to different services communicating via API
	- Don't expose all of your API - only what the external service is *authorized* to access
- Keep privilege management separate from app code (the overall idea is separation of concerns) - permissions should be stored someplace else so they're not accessible in the event of compromise
- Fair securely - if a user tries to do something they're not allowed to do, just give a terse error message and don't give up any info (e.g. "this action is not permitted" rather than "only admins, domain admins, service accounts, and privileged users are allowed to do this")
	- Same for machine-to-machine communication - don't expose anything other than what's necessary in your API error messages
- What if the authorization system doesn't respond? 
	- Fall back to a restricted set of users with locally defined permissions
	- Prevent unintentional escalation of privileges
- For web apps, make sure users can't access anything that doesn't belong to their account
- No direct object references or direct access to protected URLs
	- Just throw a sassy 404
	- Restricted resources can't be accessed by just anyone
	- And don't just hide resources (security by obscurity) - actually prevent unauthorized entities from accessing them!
- Number and frequency of operations
	- Not just for password cracking attempts, but also for DoS
	- Limit request rates
	- 1 million requests means 1 million checks - and that's a guaranteed crash
- Retire unused accounts (whether for people or machines)
	- Don't delete (what if there's an important file or evidence of illicit activity) - just disable
- Coding practices:
	- No direct system calls - use API's so that unwanted calls can be filtered out programmatically
	- Avoid race conditions and TOCTOU exploits - use locking mechanism (when a resource is being used, it doesn't allow change from elsewhere, until it's no longer being used)

### System security

- Where does our code run? What OS/server/platform/cloud/phone/etc
- Whatever it is, it has to be kept up to date so that the attack surface is minimal
- Three different attack surfaces:
	- Our app
	- The web server (meaning the service), whether under our control or not
	- The machine on which the web server runs
	- Keep all three in mind
- A web server is just another type of app. Our app will run on that server with the same level of privileges as the server. Bottom line is: least privilege for the app itself, no system-level privileges
	- A web server should **never** have root privileges!
- Remove test versions of accounts, data, and anything else that can bypass regular authentication procedures for the sake of testing
- `robots.txt` for search engines - prevents disclosure of our web app's structure to any search engines that might use crawlers
	- Disallow all sensitive information in this file
	- Don't let Google index what it shouldn't
- Choose HTTP methods that are necessary, disallow all others
	- Don't accept anything below the current TLS version
	- Inspect headers and sanitize them
	- Don't give away any version info

### Database security

- Input validation, output encoding - displaying what's returned from the DB in a safe manner
- **Parameterized queries**, prepared statements (aka **stored procedures**), precompiled statements - all to defend against SQLi
	- Instead of concatenating what the user sends into an SQL statement, treat what the user sends as plain text
	- Use placeholders for parameters that come from the user side
	- This way it's not actually interpreted as SQL
- Least privilege - users shouldn't be able to pull anything from the DB that isn't associated with their accounts
- Don't hardcode DB credentials in the app!
	- Store them securely - [Vault Project](https://www.vaultproject.io/) is a good service
	- Store all secrets securely - API keys, for instance
- No defaults, no demo data, no test data
- Disable any extra DB functions that might be available by default
	- No system calls
	- No executables
- Separate credentials for each app - for better granularity in security policies and isolating vulnerability domains

### Data protection

- Access to data: **least privilege!** :)
- Encryption all day - protects confidentiality and integrity
	- Randomness! Ciphers! Make sure everything you use is on the cutting edge as older ciphers are becoming easier and easier to crack
	- Key management - how are your secrets stored? Who has access to them?
	- Be careful with random number generators - computers are inherently bad at this
- Caches, temp files, especially on the client's side - make sure no sensitive information is in there
	- As a matter of courtesy, your app shouldn't store too much on the client side anyway - don't hog their storage
- Protect the source code, make sure people can't access it on the server
- No storing sensitive data in cleartext
	- Encryption for data at rest and in transit
- Don't allow executable code as input - validation!
	- This also goes for files users upload - validate them (and not just the extension, but the binary representation also), make sure they're not executable
- Don't store user data in the same place as the app and its code
- Data that the app has to access, external libraries or files
	- Use hardcoded references, check them thoroughly
	- Don't just blindly load stuff
	- Check the type of file and its integrity
	- It's more work, but it avoids injection of foreign code
- App files should only ever be read-only for anyone but the developer
	- In case of a successful attack
	- Ensuring the app can't overwrite itself
- Security of the server on which the app runs
	- Malware scans

### Software fingerprinting

- Code signing, especially important for compiled code
	- Making sure that the end user can validate that the app is provided by you and not an impersonator
- Hash the code, encrypt the has with your private key, user gets the public key and uses it to decrypt and validate the hash
	- Nobody but the developer has the private key that encrypts the hash, and the public key only ever matches that private key
	- Makes apps tamper-proof
- Certificates can be attached to code so that the OS can validate the app's origin

### Memory management

- Check how much memory you're allocating for inputs/outputs
- Don't allow any input that's larger than how much memory is allocated for it
	- Preventing buffer and integer overflows
	- For numbers, use stuff like `long int` or make sure numbers larger than their containing variable type are not allowed
	- Truncate inputs on the server side
- Free memory manually - even when the language you're using has garbage collection
	- Always be aware of how much memory your app is using
- Document which functions are safe and which ones aren't 
	- Safe: not allowing buffer overflows, race conditions, other side effects
	- Unsafe: when a language doesn't perform these checks and you have to write code to do it

### Other things to keep in mind

- Logging
	- User behaviour
	- Application behaviour
	- Be able to detect abnormalities and incidents 
	- Account for what's being done in the app
	- This is for performance purposes also
	- Store in a centralized location (syslog)
- Securing data at rest
	- Stuff that's stored on the client side (browsers, phone, OS)
- Integrity checking
	- Data in the hands of the user is the easiest to mess with
	- Validate it before reusing it
- Code analysis: static & dynamic
	- Static: analysing the source code itself
		- Unreachable code
		- Infinite loops
		- App reaching unplanned states
	- Dynamic: analysing app behaviour in production
		- Connections created
		- Files accessed
		- Procedures executed
		- Can users do stuff they're not allowed to do?
- **Ensuring availability**
	- Load and stress tests
	- Designing the infrastructure to be scalable and elastic
- MFA
- [OWASP Proactive Controls](https://owasp.org/www-project-proactive-controls/) is a good read

### Bottom line

- We can't design perfectly secure code
- But we can do a lot to protect our imperfect code
- Humans make mistakes - we help defend against what these mistakes can cause

---

### Exam

Be able to discuss:
- Input validation
- Risk assessment
- WAF's (see 20 and 41)
- Output encoding
- Communication security
- Session management
- Authentication
- Authorization
- Access control
- System security
- Database security
- Data protection
- Software fingerprinting
- Memory management
- Logging
- App testing