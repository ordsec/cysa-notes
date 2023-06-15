- What do cybersecurity analysts do? They keep bad guys out.
- Can't do it with our fists
- Can do it by setting up defenses and improving the org's security posture
- Most of these defenses are provided via security controls

### Reactive security

- React when a threat becomes real enough
- Enemy's at the gate - do we even have a proper gate?
- This is far from ideal. Being reactive often means not thinking straight and not acting according to a good plan.
- If we keep being reactive under attack, it might be too late and we'll take big losses.
- We have to be **proactive**. Don't wait for bad things to happen.

### Proactive security

- Ok, but there are so many threats and possible attacks out there, where do we even get started?
- Start by looking at the org itself
	1. **Asset inventory and classification**
		- What exactly do we want to protect?
		- Evaluate the assets - with actual numbers
		- Are they worth protecting? Every security control is an investment.
	2. **Risk management**
		- Evaluate all the potential risks that can affect you
		- How likely are these risks to materialize?
	3. **Security controls**
		- What hardware/software solutions can we implement to improve our security posture now that we know exactly what needs protecting?
		- A security control is something you can install/implement/apply to protect one or more aspects of the CIA triad in the org. Same goes for non-repudiation, though it's more environment-specific.
		- No silver bullet, lots of ways to approach designing controls
		- The swiss cheese analogy: one slice has many holes, but when you stack slices, there are fewer holes! *Now give Edgar some 1's :)*
		- Controls complement each other

# Security Controls

### NIST categories - SP 800-53

- **Exam**: remember this publication!
- Once again, the idea is to install controls in such a way as to make them complement each other and work together in a strong bond
- Defense in depth - install security controls from different categories

- **Technical** aka **Logical**
	- Any hardware/software we install in order to help keep bad guys out
	- Specialized systems: WAFs (e.g. ModSecurity, WebKnight for IIS, or paid solutions such as FortiWeb), database FWs (such as FortiDB)
- **Operational** aka **Administrative**
	- The day-to-day
	- Things that can be implemented by using real people as opposed to systems
	- User awareness training, security guards, etc. 
	- Contigency planning for disaster recovery
- **Managerial**
	- Refer to a high-level overview of the org's security system
	- The big picture
	- Regulations, procedures, frameworks to help us choose one control over another
	- What kind of documents do we have to review when choosing which firewall to use?
	- These also cover risk assessments and help us determine what's critical for our infrastructure and where to focus our efforts
	- Planning is a managerial control
		- Yet contingency planning is an operational control
		- Confusing? Sometimes

### Categories based on goals <<< big exam topic!!

- **Preventative** - *before the attack takes place*
	- Reduces/eliminates risk
	- Enforces some policy that keeps bad guys out
	- Examples:
		- Firewall rules/policies
		- Antivirus signatures on workstations
		- Fences, locks, biometrics, mantraps, alarm systems
		- Job rotation, data classification, pentesting
		- Access control methods
- **Detective** - *during or after*
	- Notify you of an attack or an attempt
	- Alerts, logs to review
	- Examples:
		- CCTV, IDS, security guards
		- Locks, fences, security badges, mantraps
		- Trespass/intrusion systems (physical)
		- Separation of duties, awareness training
		- Encryption
		- Auditing
		- Firewalls
- **Corrective** - *after the attack*
	- A late fix - it's time to restore our business operations
	- Bring it back to compliance as well
	- Restore from backups
	- Prevent it from happening **again**
	- Examples:
		- Backups
		- Patching
		- IPS, antivirus
		- Alarms, mantraps
		- BCP, security policies
- **Physical**
	- Deals with physical access - prevent direct contact with systems
	- Overlaps with preventive
	- Examples:
		- Guards, fences, motion detectors, guard dogs
		- **Bollards**, how could we forget...
		- Locked doors, sealed windows
		- Lights, cable protections
		- Laptop locks
		- Swipe cards
		- CCTV, mantraps, alarms
- **Deterrent**
	- Also overlaps with preventative, but in a manner such as it **discourages** an attack from happening from the very beginning
	- It doesn't actually stop an attack, but it's a definite buzzkill
	- Examples:
		- Locks, fences, security badges
		- Security guards, guard dogs
		- Mantraps, CCTV
		- Trespass/intrusion alarms
		- Separation of duties!
		- Awareness training
		- Encryption
		- Auditing
		- Firewalls
- **Compensating**
	- If we can't afford to protect it the *right* way, what can we do instead that will still do some of the job?
	- A feasible substitution in case the main control can't be implemented
	- Not perfect, but a good workaround
	- The concept is introduced by PCI DSS - you can deploy a control that ensures compliance, but uses a slightly different method (it can be one or multiple, but it has to be just as good)
	- Has to be thoroughly documented and formally approved - **remember for the exam**
	- Can overlap with deterrent, but you have to really think about it!
	- Examples:
		- Warning signs, such as "beware of dog"
		- Fake CCTV cameras (should look real)
		- Security policies
		- Personnel supervision
		- Monitoring
		- Work task procedures

### Evaluating security controls

- You have to make sure controls satisfy our needs and perform the task
- **Quality control**
	- Designed to ensure a device or solution is free from defects, software is free of bugs, etc.
	- Should be done by the manufacturer, but also by user
	- Part of the QA process
- **Verification** and **validation**
	- Verification is a compliance testing process
		- Does this piece of hardware/software meet the requirements of our design/framework?
	- Validation: does it do what we want it to, once installed/enabled?
		- Is it the right fit?
- **Assessments** - *objective*
	- Subjecting a product/solution to a checklist of requirements
		- Is the FW properly configured? Does the AV have the latest signatures?
- **Evaluations** - *subjective*
	- Humans evaluate the control's usefulness - how fit is it for our environment? Does it do the job in an efficient manner?
	- Continuous monitoring of performance
	- Takes place throughout a longer period of time
- Assessments and evaluations are much more useful when they take into consideration the current state of the environment
	- Identify problem areas and what needs to be fixed
	- Where should investments go?

### Security audits

- Audits are similar to assessments, but they're much more formal and more rigid
- Employs a predefined baseline, looks for faults and noncompliance using this baseline
- It's a compliance/regulation check

### Continuous monitoring

- Aka CSM: Continuous Security Monitoring
- Basically a continuous risk assessment
- Track anything that can be involved in a security incident
	- Network traffic, hosts, endpoints, DLP, business operations - all of these have to be watched and tracked
- Use fine-tuned algorithms/systems, detect and log issues
- **Turn reactive processes into proactive ones!**
- Costly, time-consuming, requires well-paid and motivated personnel
- The reward is that it keeps the bad guys at bay as for them it becomes a lot more time- and resource-consuming to do any "meaningful" amount of harm to your org
- There are tools that can automate this process!

---

### Exam

Understand what a security control is, be able to discuss NIST categories of controls and the 6 control purposes, be able to put examples of controls into the right bucket(s). Be able to talk about audits and assessments.