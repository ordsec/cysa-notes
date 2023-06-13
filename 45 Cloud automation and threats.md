- Cloud and automation are a match made in heaven
- Cloud environments LOVE programmatic approaches
- API endpoints exist for pretty much everything, and there are many different ways we can talk to them

### Cloud automation methods

- The most basic way of automating anything is **scripting**
	- Bash, PowerShell, Python, Ruby, etc.
- "If it's worth doing once, it's worth automating"
- A script is a collection of commands that can be run without human intervention
- In the cloud, scripts can be used to programmatically create and configure resources, but at the core it's just a set of commands
- Scripts follow some best practices in programming, but errors can happen, bugs can get in, plus scripts by definition are fairly static in that they don't adapt to configuration changes on the underlying service
	- And when this happens, you have to go and fix things manually, which doesn't really count as automation

### Orchestration

- Automation relies on running easily repeatable sets of tasks
- Orchestration takes it a level higher and allows us to automate the very creation of sequences of tasks that will be executed
- It's more than just putting scripts together. Other features include:
	- Implicit validation - most tools have really strict requirements for all parameters that need to be present in the automation workflow, and this helps tremendously with avoiding errors/mistakes/typos by checking whether all valid parameters are in there and have been entered correctly
	- Resource dependency evaluation - resources are deployed in the actual order allowed not just by the cloud environment, but also by what makes sense to us and our infrastructure
	- State knowledge - allows orchestration tools to decide whether some or all steps in the orchestration process need to be executed or not, depending on the current state of the infrastructure. Knowing the state before deciding what needs to be done, if anything!
	- **Idempotency** - related to state knowledge, means that applying a workflow many times over the same set of resources in the same infrastructure should have no effect on any subsequent applications of said workflow. We can reapply the same workflow to validate that it's still compliant with our desired state. 
- Orchestration tools include:
	- Terraform - tool for IaC, designed to deploy and manage resources in public clouds using simple configuration files that describe resources with code; these files are then read, and resources are deployed accordingly
	- Ansible - configuration management, although it can create resources also. Doesn't require an agent - uses SSH instead and runs config scripts written in YAML; these scripts are known as **playbooks**
	- Chef - also config management, uses scripts called **cookbooks** that are written in Ruby (which has to be pre-installed on any instance managed by Chef)
	- Puppet - similar to Chef, also agent server architecture 
	- Docker - developing and shipping applications in containers, which are isolated, self-sufficient instances with all dependencies included that can run on any system that has the Docker engine installed. Some orchestration is included (`docker compose`).
	- Kubernetes - the real master of container orchestration, introduces a whole new level of managing container-based apps and the infrastructure behind them (storage, networking, external access) with lots of flexibility

### Cloud automation methods

##### FaaS and serverless computing
- Function as a Service
- Based on microservices (single-purpose services, can interact with each other)
- Advertised as not requiring any type of infrastructure - but it can't just run in thin air; all that happens is you as a customer have no visibility of the infrastructure your code runs on. No need to manage servers, storage, OS, etc. - we don't even know what tech is being used
- Kind of a dumbed-down PaaS
- No concerns for the infrastructure, just provide your code, which will run in a short-lived container which is instantiated for the function's execution and immediately destroyed upon completion
- Cloud takes care of always having the function available - you only pay for how much you use
- [Netflix and AWS Lambda - a case study](https://aws.amazon.com/solutions/case-studies/netflix-and-aws-lambda/)
- What about security?
	- It's cloud-dependent 
	- Our responsibility is to secure cloud access to your our FaaS functions
	- Make sure the clients that access it don't get compromised
	- We may not have to worry about infrastructure and its availability, but we also may not be given much insight into. If a disaster happens, there's nothing we can do as we can't access the back-end infrastructure. FaaS solutions still run on physical servers that can potentially fail.

### Common cloud security aspects

##### Cloud API's
- Secure all communication - HTTPS only, don't allow HTTP connections, require the latest version of TLS
- All input must be properly validated
- API's can be attacked through DoS - enable rate limiting, use anti-DDoS if possible
- Prevent unintended data exposure - only send back what's requested, make sure unauthorized entities can't access anything sensitive

##### Key management
- API keys (including those for cloud API's) must be required for authentication of every request - otherwise the request should return a very basic and terse error message
- Key exchange should be fully protected with TLS - obviously
- Instruct clients on how to properly store and secure API keys
- **Never** hardcode any secrets in your app or store them in cleartext
- Use least privilege, only allow API users what they need, no "master keys"
- Deactivate unused keys - no one's monitoring them, and they can fall into wrong hands
- Key rotation - issue new keys at regular intervals to make sure that potentially compromised keys don't have any access
- Use secure key storage solutions such as [The Vault Project](https://www.vaultproject.io/)
- Always keep an eye on the security posture of whatever machine/laptop/workstation that is used to access and manage your cloud environment, minimize the attack surface

##### Storage
- Have strict access control policies: who has access to which type of data, least privilege, etc.
- Cloud storage permissions can be pretty complex: access lists, IP access lists, cloud users, machine accounts, regional ACL's
	- Do **not** use default settings, no matter how complex!
- Careful with public write permissions on read-only content and with read permissions on sensitive content
- AWS S3 used to set buckets to public by default - thankfully they've realized [how silly this was](https://www.bitdefender.com/blog/businessinsights/worst-amazon-breaches/)...
	- User misconfigurations can still happen - beware
- Insider threats are a concern in a cloud environment as well - and all "traditional" data threats for that matter (account compromise, data exfiltration, intentional data leakage, etc.)

##### Logging and monitoring
- There's a little less visibility in the cloud compared to an on-prem setting
- Performance management is a bit more difficult - or completely out of your control! Depends on the type of service
- Incidents can go undetected for much longer than on prem
- Good news: CSP's have dedicated solutions for monitoring and logging, with much better integration (sometimes it's literally a checkbox away)
- A good API should also provide monitoring capabilities, including alerts for unfortunate situations such as a DoS attempt
- Metrics are available - if they're not, you made a bad choice of CSP
- But still, you're limited to only what the CSP allows you to monitor and log

---

### Exam

Be able to discuss scripting and orchestration, know the differences and what tools there are for both. Know how orchestration improves upon simple scripting. Know what IaC and FaaS are, discuss different aspects of cloud security and what threats there might be.