![threat-intelligence-cycle-1.png](img/threat-intelligence-cycle-1.png)

- **Planning & requirements**
	- Why are we doing it this? What is our goal?
	- Business-oriented - we don't need to gather info that doesn't concern our org
	- What are legal regulations/restrictions?
	- Sometimes this entire cycle is mandatory - by law
	- High-level: what are some of the most likely threats for our org?
- **Collection & processing**
	- We need some raw information, something to work with
	- Needs to be done in an organized manner
	- Consistent
	- Automated as much as possible - not manual, otherwise it takes all the time in the world
		- SIEM comes into play here
	- Choose good sources - real devices, real endpoints
	- Processing and normalizing
		- Normalization: the act of making disparate data look the same
- **Analysis**
	- The more data, the higher our chance to get something useful out of it
	- But it also can be too much
		- Automation will help us out
		- Scripts (Bash, Python, PowerShell, etc)
		- SIEM - event correlation and automation
		- Some tools involve ML for filtering out the noise
- **Dissemination**
	- Communicating our findings after the analysis
		- This is done internally
	- Choose audience correctly - who are the interested parties?
		- Different levels
		- Technical staff - those who configure our security devices and respond to incidents
		- Upper management
		- C suite if something threatens the entire business
	- Make sure everybody understands
		- Read the room!
		- Tech speak with technical audiences
		- Make it clear for non-technical personnel
		- Your audiences may have different objectives and priorities in mind
	- Types of derived intelligence:
		- **Strategic**
			- Long-term objectives and priorities
		- **Operational**
			- Day-to-day priorities
		- **Tactical**
			- Real-time, shortest-term objectives, things we can do *right now*, probably falls under IR procedures
	- External communication? 
		- Helping out another org is possible
		- OPSEC comes first - don't talk about vulns you've found!
- **Feedback**
	- We have to continuously improve our intelligence process
	- Feeding information back into the TI cycle
	- Lessons learned - what went right? Wrong? Learn from both
	- Did we discover anything new? Any new threats/risks that appeared in the meantime?
	- Come up with a clear list of tasks and a list of people who will be responsible
	- Who wasn't doing what they were supposed to? 
		- Avoid blaming people - encourage improvement kindly, stay constructive, we can all do better

---

### Exam

Understand all phases, be able to explain which activity goes into each phase