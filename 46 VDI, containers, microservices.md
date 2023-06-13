- All the cool new tech!
- We still need to secure it.

### Virtualization

- This should look familiar...

![[virtualization-1.png]]

- **Type 2 hypervisor**: a dedicated piece of software that runs on top of an OS
- **Type 1 hypervisor**: a dedicated piece of software that runs on bare metal, without an underlying OS
- The hypervisor provides a lot of visibility behind the scenes: how VM's are behaving, what the resource consumption is
	- It is, however, just a piece of software, and theoretically it can have vulnerabilities that can be attacked and potentially exploited. Unauthorized access to VM management interfaces is no bueno

### VDI: Virtual Desktop Infrastructure

- Using virtual machines instead of physical machines
- Your VMs run in a data center elsewhere, no dedicated desktop or laptop computers on premises
- **Thin client**: a minimal computer used for remote connections to these VMs
	- Basically just a screen, a keyboard, and a mouse hooked up to a minified solution whose sole purpose is to connect you to remote VMs via a remote desktop connection
- Pros:
	- Extremely easy to deploy, a total luxury to manage/repair/apply security policies to
	- If a VM gets infected, you simply destroy it and restore it from a base image - literally with one click
	- Users get the same environment, resources, and experience regardless of their location or what device they use
- Cons:
	- If your backups are lacking, you're dead in the water
	- All workers depend on one single infrastructure - and it better not be a SPOF (Single Point of Failure)
	- A DoS in this context doesn't even require a successful attack - it can be a flood in the data center, or a misconfigured policy that locks everybody out, or the data center losing network connectivity
	- CYA!
- VDI solution examples:
	- [Citrix x Google](https://www.citrix.com/global-partners/google/)
	- [VMware x Microsoft](https://www.vmware.com/products/horizon-cloud/azure.html)
	- [Azure Virtual Desktops](https://azure.microsoft.com/en-us/products/virtual-desktop/)

### SOA: Service-Oriented Architecture

- Back then: monolithic, all-in-one apps
	- You had to choose the right devices/boxes and place them appropriately
- Now: we can be much more flexible in terms of placing those boxes, which allows for business-focused design
	- Think about the business first, design virtualized app components in a way that perfectly matches business requirements
	- And whenever those reqs change, we simply reconfigure
- SOA relates to the concept of designing virtual services in a way that's closely mapped to the business workflow
- To accomplish this, each service...
	- Needs well-defined inputs and outputs (a black box with a simple interface)
	- Has to be composed of smaller services
	- Must not rely on external state
	- Needs to be independently upgradeable 
	- Needs to be code-independent - using any programming language as long as it's still compatible with other services and can talk to each other
	- Needs simplified interoperability between the services
	- Have to be able to communicate with one another seamlessly
		- Using an abstract communication method known as a **mesh** or **ESB (Enterprise Service Bus)**

### Containerisation

- Aka user space virtualization

![[containers-1.png]]

- No need for a hypervisor, no need to spin up entire OS's as a VM
- Isolated environments dedicated to running a single application, built on top of a single operating system
- The container engine is obviously required, and that's what manages all the containers
- Every container is a fully fledged environment for the app that's running within, with dependencies and everything else necessary for the app to run properly
- When the OS overhead is removed, containers can be extremely fast, both for creation and destruction
- Example solutions:
	- Docker, the supreme leader
	- You don't need anything else
	- All hail Docker
- Use cases and features:
	- Development: you can instantly create and destroy a test environment to test minor changes, and you can easily use different environments to test compatibility
	- Packaging and distribution: you can make your app easily available and ship it very quickly - end user doesn't have to worry about dependencies. Batteries are included!
	- Describing infrastructure as code (IaC) through the use of Dockerfiles
		- These describe the build process executed to put a container image together
	- Also Docker-compose, which is a file that describes an entire environment: containers, networking that connects them, storage that attaches to containers, etc., all in a single file with code
- Security:
	- Less secure out of the box than a VM - quite permissive
	- Some additional precautions need to be taken to keep containers secure
	- Rememnber that the container engine runs on top of a Windows/Linux OS - which can itself have a lot of weaknesses, especially if poorly managed
		- An OS is definitely less secure than a piece of hypervisor software

### Microservices

- SOA: mapping IT architecture to business processes
- Microservices offer a similar approach
- They're all about splitting a larger functionality into small individual sub-services
- Reason: it's easier to combine them. We don't need to know how they work, we just need to know what they do - and each one does one specific job
- We can consider them black boxes - they're self-contained, rely on very clear inputs, and produce very clear and predictable outputs
- Just like with SOA, we can replace and upgrade components individually, without affecting the whole application, and this works for different vendors as well
- We do have to make sure that our microservices can communicate with each other properly
- In SOA, sub-services are allowed, but microservices use the "atomic" approach - they can't be broken down further
- Closely related conceptually to agile development and to the Unix philosophy: each piece of software should do **one** thing, and it should do that one thing **really well**
- Cloud environments are very friendly with this approach. Regardless of where they're implemented, containers are the #1 solution for an architecture that's purely based on microservices

### SOAP: Simple Object Access Protocol

- Microservices have to be able to communicate with each other regardless of what language they're written in
- SOAP ensured this compatibility for a long time
- Total dinosaur
- Manages sending and receiving data in web apps using XML
- Complex and very high-overhead protocol
- Has security extensions as a built-in feature
	- Authentication
	- Encryption
	- Asynchronous communication
	- Advanced error handling
- SOA and SOAP create new communication pathways between services, creating some serious risks:
	- Probing - similar to a scan or a brute force attempt, allows an attacker to discover various requests accepted by a web service or even view all its functions
		- Basically scanning web services
		- Malformed inputs can lead to data disclosure, privesc, or crashes
		- Input validation!
	- Coercive parsing - relies on the fact that the web service will attempt to process any XML request sent to it
		- Excessively large payloads (XML bombs) can cause a DoS
		- Also input validation!
	- External referencing - again facilitated by improper XML validation, might lead to DoS or data disclosure
	- Malware, the tried and true approach - BOF, code injection, privesc, the works
	- SQLi - if the attacker knows that an XML input is used by the service to run SQL queries

### REST API's

- **REpresentational Stage Transfer**
- The cool "new" approach
- API's are *the* way to communicate with services programmatically
	- Meaning scripts and orchestration, including programmatically creating and managing resources in cloud environments (hello IaC)
- Machine-to-machine communication, web services communicating with each other
- SOAP with XML was the standard initially, but REST API's rely on HTTP (preferably HTTPS)
- A much simpler implementation and specification compared to SOAP
- API's do create a very tangible attack surface! They are a gateway into our applications and can be a doorway into our infrastructure if not properly secured
- API keys are used to interact with the services - they're secret keys used for authentication, so they must be secured
	- Store them securely 
	- Use HTTPS
	- Come on now
- REST dictates that each resource is denoted by a distinct, clearly readable URL:
	- `mycoolstore.com/electronics`
	- `mycoolstore.com/electronics/computers`
	- `mycoolstore.com/electronics/computers/{id}`
	- `electronics` and `computers` are collections - many items from one category on one page
	- The `id` is unique for each individual item and can be generated in many ways - ideally in a secure way so that they can't be guessed (the same system can be used for a forum or a social network - you don't want people to view stuff they're not allowed to view)
	- Resources are accessible via GET or POST, no more XML documents
	- Responses can be in any format depending on the configuration, but most of the time it's JSON (XML and CSV also possible)
- A few REST principles to live by:
	- **Uniformity**: all requests should look the same, use unique ID's for separate items so each one has a unique URL, otherwise it's a collection or just a page
	- **Decoupling** between the client and the server - the REST API should be the only means of communication between these two entities; no back channels
	- **Statelessness**: no storing state information, each request must encapsulate all info required for processing, including any kind of existing state that might be relevant
		- No server-side sessions, at least as part of the REST API design
		- This helps with performance and scalability on server and client sides
	- **Cacheability** of information when possible, to improve performance

---

### Exam

Be able to explain, in detail, the concepts of virtualization and containerisation, know what VDI is all about, be able to discuss everything that has to do with microservices, SOA and SOAP, and REST API's.