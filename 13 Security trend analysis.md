- Just when you think you're in control, you get a curveball you totally don't expect
- We don't want surprises in cybersecurity
- Details are important, but so is the bigger picture

### Trend analysis

- Look at historic data, learn from it
- Try to use it to predict what can happen in the future
- The idea is being **proactive**, rather than **reactive**, which is more in line with incident response
- In addition to helping with future predictions, it can help understand past events better and build a more comprehensive bigger picture
- Don't forget about people 
	- Here's a trend: how well are your employees trained? 
	- This is fluid - people come and go, threat landscape changes
	- Train and test periodically, for instance running a phishing campaign to see how the personnel handles it
		- Run a few of these over a longer period of time, see what the trend is and how training affects it
- First, we need to collect some metrics

### What to look for

- Pretty much on the same subject as baselining
- **Frequency-based** metrics
	- How often do login failures happen in our network?
	- How often does network traffic spike? What part of the day do these spikes usually happen?
	- How many errors are logged per day/hour/minute by a specific service/application?
	- Use dedicated tools to gather all this, calculate the mean, look for outliers
	- Points of deviation are what we're after
- **Volume-based** metrics
	- How much of a specific resource is consumed?
	- How many logs are generated during one day?
	- How many events (access, system, etc.) are logged per day?
	- What's the normal volume of traffic in a given timeframe?
	- If logs grow in volume faster than usual, there may be an anomaly
- Collect information, then normalize it, group events together, correlate - SIEM!
- Then look for suspicious events and **statistical deviations**

### Monitoring

- Trend monitoring can help us get more information out of our alerts
- There's usually too much information generated, and it requires more manpower than orgs usually have to work through it
- Two things happen in this situation
	- Tuning down sensitivity to avoid getting overwhelmed with so many alerts
	- Ignoring it altogether - when you get 50000 alerts a day, you tune them out
- **Alerts and security incidents are not the same thing!**
	- Use trend analysis to determine how good you are at your own cybersecurity
- Trend analysis helps tremendously with preparation compliance audits
- If you periodically scan and monitor your network, you'll likely have everything ready for a security audit at any point in time
- A good practice is comparing your trends with some industry standards in the outside world
	- Are we within similar ranges?
	- Is anything else happening out there that applies to us?
- Use resources for trends and the overall state of affairs
	- [SANS](https://www.sans.org/security-resources/) provides a whole bunch
	- [DarkReading](https://www.darkreading.com/) is a great source of news
	- [Microsoft Security Intelligence](https://www.microsoft.com/en-us/wdsi) 
	- [Alienvault](https://otx.alienvault.com/)
	- [Symantec](https://symantec-enterprise-blogs.security.com/blogs/threat-intelligence)
	- [Secureworks](https://www.secureworks.com/services/counter-threat-unit)
	- [Cisco Talos](https://talosintelligence.com/vulnerability_info)
- Most vendors know about largely the same threats out there - a good thing because validating info via various sources makes it more reliable
	- But keep in mind that most of them publish that info to promote their security products
	- Try to filter out marketing information, go for what's relevant

### Drawbacks of trend analysis

- Very time-consuming, requires a lot of resources and dedicated effort
- There are tools that can automate it - SIEM
	- Aggregating, normalizing, correlating info

---

### Exam

Understand trend analysis and why it's useful, what types of metrics you would normally look for, remember that SIEM solutions are there to help you out.