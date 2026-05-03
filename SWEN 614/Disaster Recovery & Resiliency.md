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
- loss of revenue and sales
- damaged reputation
	- lengthy downtime can reach across the globe via social media
	- a company's inability to meet expectations and **SLAs** can turn potential customers away and drive stock down
- loss of proprietary and critical data
	- potential loss of data due to a system outage can have significant legal and financial implications

## PPO vs RTO

### RPO: recovery point objective
- how far back the data backup goes
- RPO refers to the maximum acceptable amount of datga loss an application can undergo before causing measurable harm to the business
### RTO: recovery time objective
- RTO states how much downtime an application experiences before there is a measurable damage

## What to do in downtime
- downtime is inevitable
- just because you have a system on the cloud ignoring the likelihood of downtime would be foolish
- Prevention is key
	- planning for Diaster Recovery
	- building Resilient applications
	- having (?)

### Disaster Recovery
- function of restoring normal op after an unplanned event occurs
- How it works
	- Disaster recovery relies upon the replication
	- (?)
- AWS
	- Traditional backup and restore
	- Pilot Light for Quick
	- (?)
- backup and restore
	- Most traditional environment
	- (?)
	- recovery data in a disaster scenario needs to be tested and achieved quickly and reliably
	- EC2 instances can be quickly provisioned from Amazon Machine Images(?)

## Pilot Light
uses the analogy that a small idle flame(pilot light) that
(?)
- start your application EC2 instances from your AMIs
- Resize and or scale any 
- (?)

## Warm Standby
- a solution extends the pilot light elements and preparation 
- further decreases the recovery time
- these servers can be running on a minimum sized
- (?)
- in the case of failure the standby env will be quickly scaled up for production load
- DNS records are changed to point to the other environment

## Multi-site Solution
- A multi-site solution runs in AWS as well as on your exisitng on-site infrastructure
- Commonly (?)
- Traffic is cut over to the AWS infrastructure by updating DNS

## Multi-cloud Disaster Recovery
- More organizations are moving towards a multi-cloud solution for DR to minimize impacts from an outage occurring with a single cloud-provider
- this requires more coordination and testing but may warrant the investment for organizations that cannot afford any unforeseen outages
- (?)

## Resiliency
- about building an architecture that can withstand the loss of a component with minimal or no impact
- while DR focuses on (?)
- example:
	- say wordpress example
	- single instance (EC2)
	- you attack an elastic IP address to it
	- you can also put an entry in AWS mangaed DNS service
	- This works great for one user
	- problem?
		- (?)
	- solution
		- separating the web server from the database will allow these to scale separately
		- Adding Auto-scaling to add an instance if one goes down
		- using a fully managed service for the database will ensure data is not lost
			- AWS takes care of provisioning patching configuration, backup, or recovery
		- Elastic Load Balancers(ELB)
	- problem
		- (?)
	- As more users there are on the web app, the more failover and redundancy issues arise
	- The first(?)

## Deployment Challenges
- the biggest change to software development is the frequency of deployments
- product teams deploy releases to production earlier(and more often)
- moving to a microservices architecture makes this even more challenging\
- Many organization will wait to deploy off hours to push the uipdates tot eh production environments
- CI/CD helps to alleviate some these challenges

## Deployment Strategy

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
- Once the application is singed off for release, only a few users are routed to it to minimize any impact

### Rolling
- rolling deployment is similar to canary but the difference is we update the newer version into a small number of instances
- an application's new version gradually replaces the old one over a period of time
- During this time 

### Challenges
- (?)

## Testing for outage scenarios
- error handling code and failure handling procedures are usually the least well-tested aspect of an application
- so (?)

### Chaos Engineering
- Chaos Engineering is a specific discipline that allows enginners to test the worst possible case scenarios against cloud distributed systems built from multiple services to find its resiliency
- running thoughtful, planned experiments that teach us how our systems behave in the face of failure
- Steps:
	- (?)
- Examples
	- simulating the failure of an entire region or datacenter
	- injecting latency between services for a select percentage of traffic over a predetermined period of time
	- function-basex chaos: randomly causing functions to throw exceptions
	- (?)
- Tools
	- Chaos Monkey(Netflix)
		- randomly terminate instances in production to ensure that engineers implement their services to be resiilent to instance failure
	- Simian Army(Netflix)
		- suite of failure inducting tools designed to add more capability beyong chaos monkey
	- Gremlin
- 