- There is no security control that is not vulnerable to the human factor
- We're the biggest vulnerability
- The most important step in establishing an org's cybersecurity is addressing this
- In order to do that, we need some well-defined security policies

### Security frameworks

- These are created for guidance and also to ensure compliance (but again, in a guided way)
- Basically a checklist - just a very big and complicated one, and getting all the checkmarks can take months or sometimes years
- A framework is a blueprint for implementing security - order from chaos
	- It's the highest-level policy
	- Helps us determine where our security posture is at the moment and where it needs to be
	- Based on this, we can prioritize what we should invest in for better security
- Different frameworks for different aspects, some of them are simply to pass a certification/audit and prove that you've done your homework
- **NIST** (National Institute of Standards and Technology)
	- [Risk Management Framework (RMF)](https://csrc.nist.gov/projects/risk-management/about-rmf)
	- [Cybersecurity Framework (CSF)](https://www.nist.gov/cyberframework)
- **PCI DSS**
	- A set of mandatory security controls to ensure secure handling of customers' credit card data and processing payments safely
- **ISO/IEC**
	- International/worldwide
	- 27000: an overview of **Information Security Management Systems (ISMS)**
	- 27001: defines requirements an ISMS must meet
	- 27002: provides a reference of generic information security controls including implementation guidance
	- 27701: PII controllers and processes
	- 31000: risk management
- **TOGAF**
	- Enterprise security architecture modeling
	- How to design policies and procedures
- **SABSA**
	- Information assurance, risk analysis
- **COBIT**
	- IT governance framework focused on security
- **ITIL**
	- Includes best practices for aligning IT and businesses from the security perspective, although it's not entirely focused on security
- **ESA** (Enterprise Security Architecture)
	- Lists of activities and objectives a company can follow in order to reduce or mitigate risk
- **SOC** (System and Organization Controls)
	- An audit of a company's controls that are in place to ensure CIA and privacy 
	- SOC 1: focuses on transaction and processing controls (for revenue software)
	- SOC 2: focuses on security controls that are essential for all service orgs, including CSP's
		- Type I: organization system & controls at a specific point in time, with the focus on security
		- Type II: ditto, over a longer period of time, provides high-level opinions on design and operating effectiveness of controls
		- SOC 2 reports are private - only shared with customers and and prospects under an NDA
	- SOC 3: public, general-use reports on a company's internal controls for ensuring CIA
- So there's a lot of frameworks and there's no exact science in this area
	- A lot of people had a many different ideas about how to govern the process of managing IT and cybersecurity
- Frameworks can be categorised:
	- **Prescriptive**: backed by regulations or compliance requirements
		- COBIT, ITIL, ISO (27001 in particular), PCI DSS
		- Most companies are reactive when it comes to these - these frameworks try to make them more proactive
		- How well these are implemented is measured with levels of maturity:
			- Level 1: we only have risk assessments in place, it's all on paper
			- Level 2: we now have some policies and procedures in place
			- Level 3: we now have continuous monitoring of how these policies and procedures are followed; we can also evaluate and mitigate any potential risks from now on
	- **Risk-based**: address the problem of prescriptive frameworks, which is that some orgs adapt them, check the box, and move on with life; in other words, stagnation is very possible, and attackers love these sorts of scenarios. Risk-based frameworks admit that **there's no universal framework that applies to everyone**, even when it comes to the same industry. Instead, they state: do your homework, do your research, run your risk assessments, determine your exact needs - *and then* we will implement the framework
		- NIST CSF is a great example, it's made of the following components:
			- Core, which addresses five cybersecurity functions with regards to threats: identify, protect, detect, respond, recover
			- Implementation tiers (which can be thought of as maturity levels), describing how the core elements are integrated within the company's processes: partial, risk-informed, repeatable, adaptive (and this is from lowest to highest)
			- Framework profiles: an actual list of statements about what we already have and what we still need
		- These are much more closely representative of the real life

### Framework contents

- In general, frameworks discuss the following aspects
- **AUP (Acceptable Use Policy)**, aka Fair Use Policy
	- How company resources should and should not be used
	- Computers, the internet connection, phones, etc. - everything the company owns
	- Don't use anything that belongs to the company for illegal or unethical purposes
	- Must be enforced - if it's not, and people know about this, it's like it's not there at all
	- Likely requires some technical controls - some people will try to bypass/ignore the policy
- **Code of Conduct** aka Code of Ethics
	- A documented expectation to behave ethically overall, even - and especially - when you're a privileged user
	- Only perform authorized functions
	- Protect the C and the I of the data you own and manage
	- Secure all account credentials, never share them
	- Be aware of compliance requirements, don't shortcut them
	- Respect the privacy of others, even - and especially - when you're a privileged user with access to other people's info
- **Privacy**
	- All employees have the right to expect that their privacy is protected and respected
	- However, there must be a balance - if some policies need to be enforced based on monitoring others, then that's the way it is, as long as it's done ethically
	- Privacy-related documents should be signed and agreed upon in the onboarding process
		- With all existing surveillance documented, and not just in fine print
	- Surveillance can be performed to ensure security, and it may involve monitoring data or physical monitoring
		- We need to keep an eye on the whole operation so we can see right away if there's an incident
- **Ownership**
	- This is about data; if you create the data, then you own the data
	- Data is used in workflows, so it has to be classified by sensitivity level before it can be used; what do we have access to, can we use it, and if so, how can we use it?
	- Another policy that keeps privileged users in check
	- Power -> responsibility
- **Backup policy**
	- Short-term: versioning; we need to be able to revert to a previous version if necessary, also helps recover from accidental file deletion or even ransomware
	- Long-term: for legal requirements
	- Should summarize the destruction process - it should be adequate to the sensitivity of the data we hold
- **Job-related security policies**
	- Separation of duties (high-impact tasks should be performed by more than 1 person)
	- Job rotation (rotating multiple people in the same role, preventing collusion or exclusive knowledge)
	- Mandatory vacation (against overstepping privileges)
	- Dual control (two people to perform a restricted operation; different from separation of duties as the duty the two people are performing is the same, not two different things that complement each other)
	- Least privilege (you know it by now)
- **Sybex: personnel security controls**
	- Separation of duties
	- Succession planning - ensuring continuity of roles, regardless of the reason the person leaves the org. A departing staff member can take critical skills and expertise with them, so not having a succession plan can hinder some critical processes and result in a lack of oversight
	- Background checks
	- Termination (disabling accounts, reviewing access, retrieving org property, etc.)
	- Cross training - teaching employees skill that enable them to take on tasks their coworkers and other staff normally perform, commonly used for **redundancy** purposes, i.e. making sure critical capabilities have backups in place to prevent issues with employee separation when an indispensable employee leaves the company
	- Dual control
	- Mandatory vacation
		- Combined with separation of duties, it's a very effective way to detect employee collusion and malfeasance 

### Corporate policies

- Usually come from the upper management
- Initiated by creating awareness at the management level to get enough support
- Management has to approve all changes
- Should state clearly what the goals and responsibilities are for each department (execs create the policy, HR distributes it and provides necessary training, ops workers are tasked with implementing necessary controls, employees abide)
- Should be on the need-to-know as much as possible so this info doesn't leak to potential attackers

### Procedures

- Quite simply, a procedure is a list of steps, like a recipe
- Examples:
	- Compensating controls: when you can't afford to have the actual control in place, you substitute it with alternatives that achieve the same task for the most part
		- If you can't protect it, you have to be able to rely on people following the right procedures
	- Continuous monitoring
	- Documentation of all updates and changes to systems and security policies
	- Testing controls before implementation and how it's performed
	- Change management - list steps describing how to upgrade/replace devices, how to roll back if something goes wrong; also include exceptions 
	- Patching and updating
	- Retiring technology - what to do with the old stuff
	- Onboarding and offboarding

### Procedure exceptions

- There are situations that may require us to bypass someone up the chain of command
- If it has to be done, then it has to be done (opinions on this differ, though)
- But one thing is for sure: **document everything**
	- Who was involved, what happened, why these decisions were made, risk assessment beforehand to prove that due diligence was done, expected duration, review, steps to roll back to compliance

--- 

### Exam

Know security frameworks and how prescriptive frameworks differ from risk-based ones. Be familiar with framework contents (such as privacy, backup policy, ownership, etc.), give examples of corporate policies and procedures.