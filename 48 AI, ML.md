- Artificial Intelligence, Machine Learning
- We don't have true AI yet - and hopefully we won't anytime soon - but pretty strong models already exist
- AI is the science of creating machines that imitate human thought process
- Because of its limitations, referring to it as AI is mildly incorrect - most of today's AI is machine learning

### ML

- Takes an input, returns a result, but also slightly adapts, improves, and refines the decision process with every decision taken
- The decision making algorithm is called a **model**
- In order to improve, needs human intervention in the process called **training**, which involves providing feedback to the model

##### Deep learning
- A subset of ML
- Tries to address the limitation of human intervention in ML
- Tries to abstract the logical network known as the **neural network** as a hierarchy of multiple layers, from complex to very simple classes of knowledge

### ML and deep learning uses in cybersecurity

- Smart SIEM: figuring out that malware is present in the network simply by analyzing hundreds of data points from network devices without any explicit prior training
- Data enrichment: a single alert that hasn't yet been marked as a security incident gets enriched with data from an ML system by correlating events that happened at the same time on different systems (user activity, active connections, etc.)
- Signature creation: learning the behaviour of malware and creating signatures based on observed current and historical data; refining existing signatures
- SOAR: incident response and threat hunting, mitigating the TMI problem (too many alerts, not enough eyes to look at them)
	- Storing security baselines and threat intel in a database
	- Reacting as soon as an incident is detected
	- Data is fine-tuned using ML, data enrichment is provided for better understanding of the nature of an incident and how response is conducted
	- See 42 for more on SOAR

---

### Exam

Know what AI and ML are and what the differences are, what some aspects of ML such as models and traning are, how deep learning improves on conventional ML, how ML is used in cybersecurity (SOAR in particular).