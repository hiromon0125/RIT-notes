1/12/2026 
![[Intro Cloud Computing.pdf]]

key principles of cloud computing
- pooled resources
	- cloud providers are pooled together to service multiple consumers
	- consumers share the same resources which is referred to as "multi-tenancy"
	- Each server is shared by multiple people
	- More cost efficient as users subscribe to resources vs. purchasing(?)
- virtualization
	- vital to cloud as physical servers consume significant space fro power and cooling
	- each server is partitioned into many virtual servers serving multiple applications
	- Single server is used to serve multiple server instances
	- they constitute a large pool of resources available when required
- Elasticity
	- More servers are needed during peak usage but not during normal condition or less requests. 
	- Having all servers on to support peak usage is very wasteful and costly.
	- **Elasticity** is the ability to dynamically grow with respect to however much is needed.
	- Autoscaling number of servers
	- at peak usage we can scale up or increase the number of servers to increase amount of users that could be used online at the same time.
- Automation
	- ability to provision and deploy a new instance of a machine and free or de-provision an instance when finished using or when not needed
	- Cloud-deployed application can provision new instances on an as-needed basis and brought online within minutes
- Metered billing
	- pay for what you use
	- no annual contract and no commitment for a specific level of consumption

***IF YOU ARE NOT USING A RESOURCE, TURN IT ==OFF==***

continue to [[Cloud Deployment Models]]

