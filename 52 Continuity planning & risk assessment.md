- An org doesn't make money by simply being secure
- Security is there as a continuous process to make sure the org can thrive
	- It protects our assets and our business

### Enterprise risk management

- To keep us safe, we need to identify what risks we're facing
- In an enterprise setting, risk is much more difficult to identify
- The process of this identification is what's known as **enterprise risk management**
- The goal is to protect our assets - so where do we start?
- **Prioritize** the assets based on how their potential compromise may affect the business
- **Business continuity**: how do you keep going if a disaster happens?
	- This boils down to redundancy, aka high availability, aka fault tolerance
	- Backups! 
	- Need some well-established policies and test procedures to make sure this redundancy is in place and continuously maintained - otherwise our high availability only exists on paper, and that's no good
- **Legal**: if an incident happens and your assets are compromised, you may be prosecuted (whether criminal or civil) - which always leads to loss of business 
- **Reputation**: even if you don't use your business and don't become legally liable, your reputation can be hurt as a result of an incident - and this again leads to losses, and it's hard to recover from

### Business Continuity Plan (BCP)

- A set of processes that ensure that we can continue doing business after an incident or a disaster
- As with everything else, the best idea is to document it properly and review it on a regular basis - also test it
- Consists of at least three necessary components
- **Disaster Recovery Plan (DRP)**
	- Procedures for recovering from a disaster and maintaining stability and availability of the systems the business relies upon
- **Business Recovery Plan**
	- Describes a list of steps that tell you exactly how to keep the business going and ensure recovery in an orderly fashion. 
- **Contingency plan**
	- A list of actions or procedures we should be following when we have to resume operations by replacing resources, or some of the personnel, or even the whole site

### Calculating risk

- Risk can be described in terms of the probability of a threat being realized and the impact on the business if that threat is realized
- In other words, $R = P \times I$ (R for risk, P for probability, I for impact)
	- The impact depends on the value of assets being impacted and indirect implications of losing those assets
- Even though risk management is a complicated process, calculating each individual risk is really quite simple!


### Quantitative risk assessment

- Assigning numerical values to risk
- There are some variables involved in this process:
	- **Asset Value (AV)** - the cost of replacing an asset
	- **Exposure Factor (EF)** - how much of the asset we expect to lose, expressed as a percentage (for instance, losing 30% of a business site in case of a flood)
	- **Single Loss Expectancy (SLE)** - based on AV and EF, we can calculate how much financial value we can expect to lose from a single bad event 
		- $SLE = AV \times EF$
	- **Annualized Rate of Occurrence (ARO)** - how often might this event happen within a year
	- **Annual Loss Expectancy (ALE)** - how much do we expect to lose in one year
		- $ALE = SLE \times ARO$
- Problem: this is all very subjective and it's pretty much guesswork. While the SLE can be described quite precisely (especially if the EF if 100%, which would apply to quite a lot of things as you can't lose 30% of a server), the ARO is based on assumption, and so the final ALE figure also involves plenty of assumption
- Historical data can be taken into account, but as with many things, it does not guarantee future results - again, it's just an educated guess
- Experience in this field helps may not help with quantifying risk precisely, but at least it'll help with budgeting for disasters and justifying that spending for the management

### Qualitative risk assessment

- This is basically telling a story based on some opinions
- Asking people which risk factors should be taken into consideration and how high a specific risk factor should be (low/medium/high, or some other type of a ranking system)
- As a result, a "traffic lights" report is created, such as this:

![[qualitative-risk-matrix-1.png]]

- Easier to understand, no need to assign precise values - but it's still guesswork!
- **Semi-quantitative** assessments are also possible, they're kind of a middle ground for when you need to guesstimate dollar values: how much is our reputation worth, how much is our employees' morale worth, and so on. 

### Business impact analysis

![[bia-1.png]]

- BIA is the process of estimating the losses for a specific threat scenario
- What's the business impact of an adverse event?
- The business can only make money as long as it's up and running, so the BIA relies on metrics that involve availability - i.e. time is directly involved
- There are some measurable factors here
	- **Maximum Tolerable Downtime (MTD)** - how long can a business tolerate an asset being unusable
		- Also known as the **Maximum Period of Disruption**
		- Depends on how critical the asset is
	- **Mean Time To Repair (MTTR)** - the average amount of time it would take to repair an asset (only applies to those assets that can be repaired)
	- **Mean Time Between Failures (MTBF)** - the average amount of time an asset that cannot be repaired is expected to be operating until it kicks the bucket
		- Should be an important consideration when purchasing such devices, and it's a piece of data a vendor can (and should) provide us with
		- These values are calculated using statistical models, so there's some room for error
	- **Recovery Time Objective (RTO)** - the shortest period of time it should take to fix or replace an asset to prevent a negative impact on the business; in other words, an acceptable amount of downtime, and most importantly it must be less than the MTD
	- **Work Recovery Time (WRT)** - the difference between MTD and the RTO; how much time is left over after we exceed the RTO, until we reach the MTD. This time can be used to reintegrate a device into the network, reconfigure that replaced system, restore data from a backup, etc. - any additional work that needs to be performed on the device itself before normal operations can resume
		- You may be able to get a new server in 2 hours, but you'll probably need quite a few more hours to get everything installed, connected, restored, and configured - this time must be taken into account
	- **Recovery Point Objective (RPO)** - relevant to data; the maximum acceptable amount of data loss measured in time. In other words, it's the age of the files or data in backup storage required to resume normal operations if a computer system or network failure occurs.
		- If your backups are conducted daily, then your RPO is ~24 hours
		- The most recent time in the past from which you can restore a valid backup
		- From a different perspective: how much data you're comfortable losing whenever you're restoring from a previous backup

### So what do we do with all this risk?!

- A few things, actually - it depends on what our risk appetite is and what our resources are
- **Risk avoidance**: terminating the cause of the risk completely. Simply no longer doing whatever it was that created the risk in the first place
	- Likely assumes getting rid of some functionality - can we afford to do that?
- **Risk transfer or sharing**: your risk is now someone else problem - likely the insurance company's, but situations such as signing up for a cloud provider instead of using on-prem systems also fall under this. 
	- Downsides: it costs. Also, even though you've transferred the risk and protected your financials, if you experience a data breach, it's still a loss of that data and likely a loss of reputation - something that'll probably cost more than what you get back from the insurance claim
- **Risk mitigation or remediation**: do something to reduce the risk (either its impact or its likelihood), then accept the **residual risk**. Usually involves setting up some countermeasures or controls.
- **Risk acceptance**: we're cool with it 😎 Sometimes the effort or the cost is just too high or simply not worth it.
	- Back to residual risk: we've patched everything we know about, we've set up our controls, but the unknown unknowns are still out there, and that's something we have to accept. Risk is never zero unless we perform avoidance. 
	- Everybody involved must understand the magnitude of such decisions!
- And so risk assessment is a process that we perform in order to bring all risk in our enterprise down to an acceptable level - not to zero. 

### Incident impact

- What we stand to lose in terms of money, availability, reputation, etc.
- First, we have to identify the incident and know exactly what we're dealing with
- High-level identification of an incident:
	- Category: phishing, DoS, worm outbreak, etc.
	- Vector: people and systems affected, how long it lasted, is it still happening
	- Severity (but it's subjective)
- Scope-based classification of an incident:
	- Organizational vs local: how much was affected. Organizational likely affects mission-critical assets, local means it's a department, or a group of users, or just one user
		- Can be tricky to draw the line here. It may be just one server, but what if the resulting fallout affects the overall function of the business? 
	- Was anything critical affected?
	- Number of systems affected - but it may not accurately reflect the impact. You may have 100 machines affected by a worm, but that would only result in a temporary loss of performance
- How difficult is recovery?
	- Consider the complexity of all the steps required to recover; business may not function at all until you do recover
	- What is the financial and time expenditure to get back up?
- Immediate vs total impact:
	- Immediate impact is the direct cost of the incident: damage, downtime, costs, fees, fines if applicable. Anything we can calculate
	- Total impact: costs that arise after the incident: reputation, loss of clients, loss of partners

### Risk assessments

- The purpose is to determine the probability of a threat
- Should be conducted whenever a new system is installed or if we're planning on purchasing and integrating one
- Conduct periodically in general due to other factors that can change in our environment
- Addresses some important points:
	- **System characteristics**: having a thorough understanding of the system and the risks that apply to it and the way it's used: hardware, software, users, connections, and so on. Pretend you're an auditor
	- **Threat identification**: human, technical, anything. Motivation for those threats as well. If we're storing a bunch of sensitive data, that gives bad actors motivation to attack us
	- **Vulnerability identification**: what exactly can be exploited? Vuln scans and pentests are part of the overall risk assessment
	- **Control analysis**: what controls do we already have and how well are they performing? 
	- **Likelihood**: how often can we expect a certain adverse event to happen?
	- **Impact analysis**: expected impact of an incident on our CIA
	- **Control recommendations**: once everything is evaluated, it's time to think about what controls should be set in place in addition to or instead of existing controls. How can we improve? Think about whether every suggested new control is worth the investment
	- **Documentation**: write everything down, in detail. Otherwise all these improvements will be extremely difficult to implement and maintain
- **Objectives**: asset security: human and technical (people first!)
	- Minimize the risk, minimize the loss, 
	- Make sure it doesn't happen again
	- Make sure the recommendations coming from the risk assessment increase the stability of all our systems
	- Avoid being liable for any damages related to a potential incident
- **Planning**
	- Have an IR team, likely representatives from each department that will help identify critical systems and assets in their respective departments
		- Also a group of highly skilled individuals who can take care of all systems
	- Conduct a BIA, determine losses in case of an incident
	- Create and communicate procedures, assign responsibilities, make sure everybody knows who to call. **Make sure your entire team is responsive not reactive**, make sure there's enough awareness, conduct presentations and workshops
	- Test procedures: tabletop exercises, walk-throughs, simulations. Be open to ideas from everybody - what if you've missed something?

---

### Exam

Understand all aspects of continuity planning and risk assessment: risk calculation, BIA, etc. Know risk policies (avoidance, transference, mitigation, acceptance). Know the details of how the impact of an incident can be analyzed and described.