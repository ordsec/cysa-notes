- Perimeter security is great - until your users bring in a bunch of smartphones...
- There are some significant difficulties and threats mobile devices pose

### BYOD

- Bring Your Own Disaster
- The trend we need to be the most concerned about
- Employees using their personal mobile devices to connect to the network at their workplace
- Fundamental idea: allowing employees to work the way they're used to, with devices they feel the most comfortable with
- The problem: we're bringing devices into the network that may not be secure at all
	- To make it worse, these devices are used **a lot**
- BYOD policies therefore need to be very strict yet coherent - creating security without too much frustration
- BYOD-related concerns:
	- Lost or stolen devices with sensitive data exposed
	- Personal data and how much control the company has over one's personal device - if it's 100% then it's no longer a personal device. Where do you draw the line?
	- Company control and managing a multitude of devices with different OS's, configurations, etc. We may have an attack surface of a completely unknown size
	- What happens if a personal device is involved in a security incident on our turf? We would need to mitigate threats on such devices, and we may not even be able to investigate anything since the devices don't belong to us. Again, tricky to draw the line between what's personal and what's business-related
	- The sheer number of devices on our network if personal devices are suddenly allowed - can we handle it?

### Mobile platforms

- We're pretty much down to just two major ones: Android and iOS

##### Android
- Open-source, so there's a lot of flexibility for implementing customized versions and solutions, including those for mobile device management
- More relaxed in terms of app stores, SDK's are easily available
- Samsung Knox is a good solution for managing the security of Samsung phones within an org
- SEAndroid is similar to SELinux - it's an implementation of Mandatory Access Control for mobile
- Downside:
	- More open environment, so updates, including security updates, are the responsibility of vendors who provide model-specific implementations of Android. This means that updates main not be rolled out in a timely fashion. 

##### iOS
- Closed environment, no 3rd-party app stores, everything is controlled solely by Apple
- Much less customization, iOS is iOS on every iPhone
- Fewer opportunities for attackers to try and compromise it
- Updates are rolled out promptly

### Centralized management

![[uem-schematic-1.png]]

- There are lots of solutions and related acronyms, which can be confusing since a lot of them are not tied to specific technologies 
- A lot of these things are concepts or sets of policies (such as MDM, which encompasses a lot of different ideas of methodologies for how to manage mobile devices in a corporate environment)
	- Therefore a lot of them are marketing terms
- **Exam**: be familiar with
	- **MDM**: device security (passwords, biometrics), provisioning (bringing the device into the company environment securely), device grouping (associating related devices into groups and applying group-wide policies), location tracking (danger zone - don't get into legal trouble with this one), device health monitoring (aka security posture evaluation, completely disallowing jailbroken/rooted devices)
	- **EMM** (Enterprise Mobility Management): a higher level of abstraction that encompasses a few concepts:
		- Mobile Application Management (MAM): what applications are employees allowed to install and run? What's the source? Companies can restrict app stores from which apps can be installed or even create their own app stores and only allow apps from there
		- Mobile Content Management (MCM): what kind of content is allowed on the device in an enterprise environment? Is sharing allowed? This is where we containerize business-related data and hopefully segment it away completely from personal data if that's allowed.
		- Mobile Information Management (MIM): device tracking, keeping track of OS versions and update history, general accounting
		- BYOD
	- **UEM** (Unified Endpoint Management): attempts to address all endpoints, from desktops to laptops, tablets, wearables
- MDM solution examples:
	- VMware Workspace ONE (formerly AirWatch)
	- mobileiron UEM
	- [There are too many](https://www.pcmag.com/picks/the-best-mobile-device-management-mdm-solutions?test_uuid=001OQhoHLBxsrrrMgWU3gQF&test_variant=b)

---

### Exam

Be able to discuss BYOD: what it is, what challenges it presents, and how those challenges can be dealt with. Compare and contrast Android and iOS, know what MDM and EMM are and how they're related to each other.