![[SWEN 614/lectures/Resiliency.pdf|Resiliency]]

Recent outages
- MS Azure(2021)
	- microsoft corp was hit by massive cloud outage
	- all microsoft suite was all inaccessible due to the outage
- Google(2019)
	- a configuration change intended for a small group of servers in one region was wrongly applied
- AWS(2017)
	- An amazon web service engineer trying to debug on an S3 storage system in virginia data center typed a wrong command taking down the entire internet

## Downtime is really expensive

## business impact on downtime

- business disruption
	- varies to application and the business
- loss of revenue and sales(avg $300k in loss)
- damaged reputation
	- lengthy downtime can reach across the globe via social media
	- a company's inability to meet expectations and **SLAs** can turn potential customers away and drive stock down
- loss of proprietary and critical data
	- potential loss of data due to a system outage can have significant legal and financial implications

## RPO vs RTO

![[Pasted image 20260503153211.png|400]]
### RPO: recovery point objective
- when disaster struck how far back the data backup goes
- how often the backup data is taken
- RPO refers to the maximum acceptable amount of data loss an application can undergo before causing measurable harm to the business
### RTO: recovery time objective
- How much downtime an application can afford to experience
- RTO states how much downtime an application experiences before there is a measurable damage

## What to do in downtime
- downtime is inevitable
- just because you have a system on the cloud ignoring the likelihood of downtime would be foolish
- **Prevention** is key
	- planning for Disaster Recovery
	- building Resilient applications
	- having deployment strategy that minimizes downtime
	- thorough testing for outage scenarios

### Disaster Recovery
- function of restoring normal op after an unplanned event occurs
- How it works
	- Disaster recovery relies upon the replication
	- when servers go down due to 
		- natural disaster, 
		- equipment failure 
		- or cyber attack
	- Application will needs to recover lost data from a second location where the data is backed up
	- Ideally the org should transfer its compute to that safe remote location in order to continue operation
- AWS
	- Traditional backup and restore
	- [Pilot Light](<#Pilot Light>) for Quick recovery(active - cold backup)
	- [Warm standby](<#Warm Standby>)
	- [Multi-site Solution](<#Multi-site Solution>) (Active - Active)
		- Runs two or more full instances in parallel.
		- Most costly, but zero downtime as one can just fully absorb the requests from the other when needed
- backup and restore
	- Most traditional environment data is backed up to tape and sent off-site regularly
	- S3 is an ideal destination for backup data as it provide 99.999999999% reliability
	- for systems running on AWS customers also backup into S3
	- recovery data in a disaster scenario needs to be tested and achieved quickly and reliably
	- EC2 instances can be quickly provisioned from Amazon Machine Images
	- AMI is a pre-configured with the proper OS and applications

## Pilot Light
- Active-cold backup config
- uses the analogy that a small idle flame(pilot light) that is always on and can quickly ignite the entire furnace to heat up
- ensure that most critical core elements of the system already configured and running in AWS
- when the time comes for recovery a rapidly provision of the full scale production environment around critical core
- start your application EC2 instances from your AMIs
- Resize and or scale any db or data store instance where necessary
- change DNS to point to the backup servers
- one site is actively handling request
- second backup site is "off" or minimally running and ready to provision when needed
- when disaster strikes the backup site is provisioned to complete state.
- cheap to maintain but slower recovery due to the need of provision step

## Warm Standby
- a solution extends the pilot light elements and preparation 
- further decreases the recovery time
- these servers can be running on a minimum sized fleet of ec2 on the smallest scaled size possible
- in the case of failure the standby env will be quickly scaled up for production load
- DNS records are changed to point to the other environment
- more costly than pilot light but less downtime

## Multi-site Solution
- Active-active config
- A multi-site solution runs in AWS as well as on your exisitng on-site infrastructure
- Traffic is cut to problematic AWS infrastructure by updating DNS
- Load-balancers are also used to balance usage between sites
- Most costly solution as double or more full production scale resource is provisioned at the same time

## Multi-cloud Disaster Recovery
- Organizations are moving towards a multi-cloud solution for DR to minimize impacts from an outage occurring with a single cloud-provider
- this requires more coordination and testing but may warrant the investment for organizations that cannot afford any unforeseen outages

## Resiliency
- An architecture that can withstand loss of a component with minimal or no impact
- different focus or goals
	- DR focuses on recovering from disaster, 
	- IT resilience focuses on the proactive measures 
		- that business can put in place to keep running for worst case scenario
- example:
	- say wordpress example
	- single instance (EC2)
	- you attack an elastic IP address to it
	- you can also put an entry in AWS mangaed DNS service
	- This works great for one user
	- problem
		- scalability may be a problem due to overwhelmed servers
		- no recovery methods
	- solution
		- separating the web server from the database will allow these to scale separately
		- Adding Auto-scaling to add an instance if one goes down
		- using a fully managed service for the database will ensure data is not lost
			- AWS takes care of provisioning patching configuration, backup, or recovery
		- Elastic Load Balancers(ELB)
	- problem
		- scalability could be better
	- As more users there are on the web app, the more failover and redundancy issues arise

## Deployment Challenges
- the biggest change to software development is the frequency of deployments
- product teams deploy releases to production earlier(and more often)
- moving to a microservices architecture makes this even more challenging\
- Many organization will wait to deploy off hours to push the uipdates tot eh production environments
- CI/CD helps to alleviate some these challenges

## Deployment Strategy

How a new verison of the infrastructure or version of the software is deployed

### Blue Green
- two identical production environments work in parallel
- one is the currently-running production environment receiving all user traffic(Blue)
- the other is a clone of it but idle(Green)
- Once the testing results are successful, application traffic is routed from blue to green 
- green then becomes the new production

### Canary
- canary deployment is like Blue-Green
	- except its more risk-averse
- Instead of switching from Blue to Green in one step you use a phased approach
- Once the application is signed off for release, only a few users are routed to it to minimize any impact

### Rolling
- rolling deployment is similar to canary but the difference is we update the newer version into a small number of instances
- an application's new version gradually replaces the old one over a period of time
- During this time 

### Challenges
- which deployment strategy to use
	1. Blue-green
		1. rollback is easy as we have complete copy of the production environments
	2. Canary
		1. no downtime while updating or releasing the new feature
		2. cheaper in terms of infra
	3. Rolling
		1. rollback process is easy as deploying instance by instance
		2. cost-effective as utilization is reduced

## Testing for outage scenarios
- error handling code and failure handling procedures are usually the least well-tested aspect of an application
- how do you know you have built a reliable system
- if you know things will fail, you can build mechanisms to ensure system persist regardless of what happens

### Chaos Engineering
- Chaos Engineering is a specific discipline that allows enginners to test the worst possible case scenarios against cloud distributed systems built from multiple services to find its resiliency
- running thoughtful, planned experiments that teach us how our systems behave in the face of failure
- Steps:
	1. Plan experiment
	2. contain blast radius
	3. scale or squash
- Examples
	- simulating the failure of an entire region or datacenter
	- injecting latency between services for a select percentage of traffic over a predetermined period of time
	- function-base chaos: randomly causing functions to throw exceptions
	- Code insertion: adding instructions to the target program and allowing fault injection to occur prior to certain instructions
	- Time travel: Forcing system clocks out of sync
	- Error handling: Executing a routine in driver code emulating I/O errors
	- Max resource: Mazing out CPU cores on a clusters
- Tools
	- Chaos Monkey(Netflix)
		- randomly terminate instances in production to ensure that engineers implement their services to be resiilent to instance failure
	- Simian Army(Netflix)
		- suite of failure inducting tools designed to add more capability beyong chaos monkey
	- Gremlin
		- Hosted solution for chaos
		- Failure as a service
