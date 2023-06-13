- So you've done the scanning, you've found some vulns, and it's time to fix them
- This is the **remediation phase**
- But it may not always go as planned...

### Legacy systems

- Real old hardware and software, old OS's, anything that's past the EOSL date
- Can't update them - but we have to do something
	- Compensating controls, e.g. a packet-filtering device to further protect the legacy system as long as the org has to keep using it
		- Ideally you want to be able to detect anomalies around legacy systems
	- Air-gap the system - take it offline, disconnect it from internet-facing networks, but allow it to run
	- Best of all, just replace the damn thing, retire it, get something you can actually patch

### Proprietary systems

- Custom software - either developed in-house or purchased for a specific purpose
- In other words, it's not just regular commercial software
- If it was developed in-house, maybe the team that developed it has been dissolved, and no support is possible
- If it was purchased from a different company, that company might have gone out of business or they simply stopped supporting it because it's not worth the time and manpower spending
- Contract ran out or it specifies that support isn't meant to be
- Or it's just no longer supported - abandonware

### Degrading functionality

- Often coupled with updating legacy/proprietary systems
- You've updated the software/firmware package on a critical server, and now it needs a reboot - when do we do it? What if it's the only server? Do we have a rollback plan?
	- It may need to be brought offline
	- What if our high availability/fault tolerance solutions aren't properly implemented or aren't in place at all? 
	- And if they are in place, when they kick in, how will it affect everything?
	- Bringing it back online may cause issues as well
	- All these issues should be covered under Business Process Interruption, with processes and procedures clearly described
		- Have a maintenance window
		- Implement change management
- You've introduced encryption into the workflow of your app or network or for your data at rest
	- What if the server is old and doesn't have the CPU power to process encryption?
- Software dependency issues, library version conflicts
	- What if our stuff only works with certain library versions?
	- If something is poorly designed, it may not work with later versions
- New patches can introduce bugs and crashes
	- Change management, sandboxing to test patches before deploying them across the org - make sure everything can be approved
	- Test env needs to replicate prod env as much as possible

### Organizational governance

- Bureaucracy
- Red tape
- Management can go all bureaucratic: "we have procedures, pipelines, approval requests, decisions, signatures - you can't just go and update our stuff"
- "Why do we need it?"
- Blame management/distribution
- **Exam**: the important idea is to always get written approval from management before making changes to the working environment

### MOUs and SLAs

- Degrading the functionality of your network/systems may not just make colleagues somewhat unhappy, but it may also impact/violate some agreements you have with your partners/clients/users
- Coordinate downtime with whoever may be directly involved
- Don't breach contractual terms!
- There may be clauses in those agreements that prohibit you from making certain remediational changes
- SLA says you can't scan or update - means you can't scan or update
	- Look for someone who can

>**MOU**

**Memorandum of Understanding**

- Somewhat like a gentleman's agreement
- Written in such a way as to clearly describe timelines, who's responsible, what the roles are, what the expectations are
- NOT legally binding
- Can be as simple as an email follow-up after a meeting 

>**SLA**

**Service-Level Agreement**

- Contractually and legally binding
- What the company agrees to provide - what the deliverables are
- How you will measure those deliverables
- SLA with an ISP:
	- ISP says: I commit to providing 24/7 service, 95% uptime, with X mbits/s
	- Uptime and bandwidth are two metrics usually found in such an SLA
- Other examples:
	- Availability for an online service/cloud provider
	- Response time for incidents by a company that provides IR
	- How quickly a component will be replaced by a vendor
	- How quickly a support ticket will be processed
- **Exam**: SLA's have to be **measurable**, and the way they're measured has to be explicitly stated in the SLA documentation
- If provider doesn't comply with what's in the SLA, they can be held legally and financially responsible

### Exam

Be able to discuss risks an org can face while remediating vulnerabilities. Remember the aspects here: legacy systems, performance degradation, proprietary systems, approval requirements, be able to provide examples or answer questions in a scenario. Know what an MOU and an SLA is and what the key differences are between the two.