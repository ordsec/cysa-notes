- Cybersecurity perspective
- Testing software to catch bugs that can potentially become vulnerabilities
- Nipping those data breaches at the bud
- Keeping customers happy
- Bad news: this process is very difficult
- It's pretty much impossible to catch every single issue in a fairly complex app
	- But we must try and minimize the number of potential problems
- We can only fix things that we know about
	- And that's why bug bounties exist - companies admitting to themselves and to everybody else that they don't know what other issues there may be. And it's totally okay!

# Testing and improving code

### Static code analysis

- Literally just looking at the code
- Analyzing it for potential weaknesses that can lead to vulnerabilities
- **This is done before the code is run or compiled**
- Manual code review, peer review
	- Another human being next to you, working through the same process
	- Could be a programmer, but could be a security analyst!
	- But unfortunately security is usually an afterthought - and this needs to change
	- Focus on communication, accept the limitations of your brain, admit when you're wrong
	- You're looking for flaws in someone else's work - they may take it personally (although they shouldn't)
	- Managers and team leads should train developers from early on about basic security concepts so that many issues can be avoided
- Automated review - tools may have better insight, intelligence, and attention to detail
	- [List of source code security analyzers by NIST](https://www.nist.gov/itl/ssd/software-quality-group/source-code-security-analyzers)
		- Most are focused on specific programming languages
		- Some are free, some are paid, some haven't been updated for a while
		- Linters belong here

### Formal method

- Completely automated
- A type of software verification that relies on mathematical inputs to track code execution
- Which code paths are being used, which ones are unreachable? Potential infinite loops perhaps?
- Mostly used for complex projects where there can be many branches and execution paths - too many to keep in mind for human testers
- Static analysis requires automated tools to understand the programming language the code is written in
- Formal method requires that tools understand the application itself and how it's supposed to be used
- Need a well-defined formal model
	- How does execution flow?
	- What are some end states?
	- This isn't an easy task, sometimes it can't be accomplished

### UAT (User Acceptance Testing)

- Focuses on users!
- Beta versions
- Users become testers - knowingly or not...
- Have a select few future customers volunteer
	- Or push out a pre-release version (but make it abundantly clear it's not final)
- Provide guidelines/documentation for testers
	- They may or may not follow them, but either way is fine - we're testing the app and looking for points of failure
- This is about user experience as well. You may have a vision for how your app is gonna be used, but will it end up being used in such a way? 
- Does the app/feature set meet user requirements? Do users accept the app in its current state?

### Regression testing

- Performed by the dev team
- Just like any functionality testing you'd normally perform, but it happens every time a feature is added or updated
- The idea is that any code change may break something that was previously working
	- Or have unintended side effects
	- Introducing new vulns with a new patch is an undesired possibility
- Here's a cool new feature! It totally works - we've tested it!
	- Ok, but we have to test everything else we've done before it
	- Make sure merging in the new feature doesn't break other things
- Patches are supposed to fix problems, not create new ones

### Reverse engineering

- Deconstructing compiled code into source code
	- Or at least down to a certain state where we can figure out what the code is supposed to do: all possible ways of execution flow and end states
- Use this mainly to figure out what the code is supposed to do
- Not always fully possible
- Three levels of code:
	- Machine code (compied): contents of the executable file. Impossible to understand with naked eye, hard to understand while using tools, toughest to deconstruct and analyze
	- Assembly code: native processor instructions, presented in a human-readable language
		- Pretty obscure, but we can directly program instructions for the CPU, which gives us a lot of control
		- Most instructions refer to moving data around and performing mathematical operations
		- Working directly with memory
		- A **disassembler** is used to generate assembly code from a compiled program - again, not always possible, and not easy to read and understand
	- High-level code: written in all the languages we write code in: C/C++, Java, Python, Go, and so on
		- A **decompiler** is used to try and deconstruct compiled code into one of these high-level languages - not always possible, but there are languages that are better suited for this than others
- Most popular tool for reverse engineering is [IDA (Interactive DisAssembler)](https://hex-rays.com/ida-pro/)
	- Very powerful, shows a breakdown of functions code performs, graphic representations of execution branches, etc.

##### Reverse-engineering malware

- Use a **sandbox** (isolated environment where malware can't "detonate" and do any harm)
- Examine behaviour as we let malware do its thing
- But bad guys won't just let you take a look at their code and figure out how it achieves its task
	- Code obfuscation
	- Code encryption
	- Anti-reversing techniques

### Dynamic code analysis

- Taking her for a spin!
- How does the application run? How does it interact with the system it's run on? How does it behave?
- Run the code - stop staring at it
- **Debugger**: a tool for examining code execution as it runs
	- Pause execution
	- See which execution branch you're in
	- Look at the data flow (examine variables and the execution stack)
	- Step through execution manually, line by line 
- **Stress testing**, aka load testing
	- Closer to the end of the development process, when all other problems are solved
	- Stretching the app to its limits by throwing a bunch of input at it
	- When does the performance begin to decrease? Can we crash it?
	- How well can it withstand a DoS attack? Does it fail in a secure manner?
	- If the app is in the cloud, we can stress-test it to determine at which point scaling will be needed
- **Fuzzing**
	- Throwing all different types of input at the app
	- Focus is on diversity rather than quantity
	- Great for making sure input is properly validated and exploring edge cases
	- Use obfuscation techniques to trick the app
	- Types:
		- Application UI (forms, text fields, etc.)
		- Protocol - how does app handle traffic that's not normal for the protocol it uses? Unexpected values in headers? Many attacks can be detected at the protocol level (this is one of the things IDPS solutions look for)
		- File format for uploads - can we trick the app into executing a payload with an `.jpeg` extension? Can it handle unexpected formats? How does a resume parser behave when given a Word file with macros in it?
	- Design your test cases carefully, don't automate them too much, be creative, be crafty
	- Devise testing mechanisms that kick in after fuzzing is completed to determine whether the app processed inputs properly or not. Has it crashed? Has it reached an undesireable state?
	- Fuzzing for web apps: Burp Suite (Intruder mode for automation)
	- Users shouldn't be able to do this stuff at all - once app is deployed, it needs to have limited request rates on the server side, captcha checks, etc.

### To summarize vuln assessment in testing...

- A few areas to keep in mind
- **Authentication** - will most likely be the first one under attack
	- Any situations in which authentication can be bypassed?
	- Are there resources that allow access without any authentication?
	- Validate user identities securely, don't allow overriding
	- Don't transmit creds in cleartext
- **Injection flaws** - also has to do with input validation
	- Sanitize input, don't allow any type of code to run
	- Perform sanitization on the server side, don't trust apps users run
- **Authorization**
	- Check permissions
	- Make sure every privileged operation can only be run if the user is authorized
	- Users shouldn't be able to impersonate one another
	- Make sure sessions are secure and have expiration
- **Error handling**
	- Provide just enough info to users to show them there's an error - nothing on top of that, don't disclose internal processes
	- This includes all interaction with users
	- Don't list the types of characters allowed/disallowed for passwords
- **Encryption**
	- At rest - including storing on users' devices
	- In transit - whenever transferring data between user/app or between app/3rd party app
- **Auditing/logging**
	- Keep good records of user and app activity
	- Keep logs secure - users shouldn't be able to access them
- **Session flaws**
	- Protecting the identity of the user while they're interacting with the app
	- Make sure there are no impersonation vectors
	- Session IDs shouldn't be guessable
- **Insecure configurations**
	- Default credits? 
	- Test data? Test accounts?
	- Functionality that bypasses authentication?
	- Provide users with a secure environment by default - not all of them are tech-savvy

### SAST and DAST

>**SAST**

**Static Application Security Testing**

>**DAST**

**Dynamic Application Security Testing**

![[sast-dast-explained.png]]

### Exam

Just know all of the above, pretty much. Be able to explain the ideas behind static/dynamic testing, fuzzing, formal method, UAT, regression testing, reverse engineering, vuln assessment. SAST and DAST are on Sec+, may be on this one also.