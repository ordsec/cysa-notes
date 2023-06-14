- Protecting what we own starts with knowing exactly what it is that we own
- The next step is prioritizing what's more and less important
	- This is fairly company-specific, but the general gist is that anything business-critical is an important asset
- Within the IT security context, assets are usually routers, switches, servers, endpoints, mobile devices, the huge screen in the lobby, and so on

### Asset tagging

- Lots of devices to keep track of, plus what's leased from other vendors - it must stay organized, or it's a total mess
- Assigning a unique identifier (number, QR code, barcode, RFID tag), ideally reflected in a physical tag that's attached to the device 
- If physical security is a concern, make sure tags are tamper-proof
- Use dedicated software (not Excel) to keep a record of everything
- We need this for
	- Inventory and security - what devices are we exposing to the internet or to the physical world when they're taken home by an employee?
	- Discouraging theft
	- Keeping track of who the device belongs to if other than our org
	- Warranty and service - who you gonna call?
	- Daily operations
- Software licenses should all be on record as well - they're paid assets
- Generate management/inventory reports

### Change management

- Every company goes through changes, whether they're small upgrades/updates or fundamental changes such as network structure
- Ideally, for every asset, there should be a record for what its initial state was when purchased and a record of every change made to this asset (installations, upgrades, replacements, etc) - this is the process of change management
- Change can bring in risk, and one of the goals of change management if minimizing this risk
	- Performing changes without adversely impacting operations
- Proactive or reactive
- In cybersecurity, change management has to include an assessment of the security impact of a change
- **RFC (Request for Change)** - standard tool used in change management
	- There should be a designated person writing these whenever a change needs to be made. Includes a description, explanation for why the change is needed, what is affected, and a risk assessment. If risks are identified, a rollback plan is necessary in case things go wrong. In other words, always have a way back to the last working configuration
	- Has to go through an approval pipeline before it's implemented, particularly for major changes
		- May not be necessary for small updates, but never assume that you can just go and do it - there's always a chance it impacts someone else
		- CAB (Change Advisory Board) may be in charge
		- Automation is great to have for any repetitive or regularly occurring change processes 

### Risks of change

- Some things to keep in mind when making changes to assets
- **Downtime**
	- Making your systems/services/applications unavailable for any amount of time
	- If any errors happen in the update/reboot process, downtime gets extended
	- Have a snapshot/backup to return to a previous working state if anything goes awry
	- Have a maintenance window that budgets enough time for this stuff
- **Degraded functionality**
	- Partially or completely affecting the functionality of some systems
	- Updating software but keeping the same hardware may not necessarily improve performance - the opposite can happen
- **Increased attack surface**
	- An update can reset previously configured security settings, including resetting to default
	- Exposing more of the application/service than previously intended
	- Replacing a network device with a new one requires bring it up to speed security wise, make sure all firewall rules are up to speed
	- Make sure all assets are still covered by general security policies that apply everywhere else
- **Planning vs reactive change**
	- Proactive: plan for bad things to happen; prepare for war if you want peace. Try and predict what can go wrong, reduce the "surprise surface"
		- A documented plan with specific procedures is the best way, for any scenario that isn't the best case
	- Reactive: oh s***, everything is broken, what do we do?
- General best practices for dealing with these risks
	- Have well-documented **rollback procedures**
	- Use a test environment or a sandbox whenever major changes are involved, before deploying for real. Analyze in detail how the update behaves and what side effects there are

### Configuration management

- Really only concerns software
- Keeping track of all configs
	- OS settings
	- Device configs
	- System settings for servers
	- AD settings for endpoints
	- List of software that is supposed to run
- Have a baseline so you know exactly what to expect in a "normal" state of things
	- Otherwise we can't catch anomalies/misconfigurations
	- Update the baseline when changes go through successfully
	- Compare baselines to see all differences after the update
		- Maybe we have new services and our attack surface has grown?
- Versioning - keep track of it for software and firmware
- Use dedicated software for this

### Exam

Understand the importance of asset management and tracking, be able to talk about change management, its purpose and risks, remember configuration management's purpose and benefits.