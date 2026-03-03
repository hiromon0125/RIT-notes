
- Some limitations with current setup on wordpress assignment example
	- everything resides on a single server
- if anything crashes app is unavailable
- if anything needs to be updated, the app needs to be taken offline
- No load balancing if there is problem accessing a region or single server that might be unavailable
- No external storage to expand over time
- no ability to expand as user demand increase
- cost to run it per month is $8.50/month
## How to estimate the cost

- there are many factors to consider when determining an estimate
	- how many requests are coming in per month
	- estimated amount of data to transfer in/out per month
	- how much storage is needed
	- region that stuff is running in 
	- type of support model
- AWS has a solution called AWS pricing calculator

## Cloud waste
- estimates based on costs associated with IaaS where the cloud resources are wasted
- Idle resources
	- server is running with no jobs
- over provisioned resources
	- about 40% of instances are sized at least one size larger than needed for their workloads
	- cost can be cut in half by reducing an instance by one size while downsizing by two sizes saves 75%
- Orphaned volumes and Snapshots
	- these are resources that have been detached from the infrastructure they were created to support such as a volume detached from an instance or a snapshot with no volume attachment

## Another Cost Factor - Support

- AWS Support Plans
- IMAGE


## Things to consider

- computing power
- infrastructure location
- storage
- bandwidth allocation
- service level agreements/uptime requirements


## Group Activity

![[Screenshot 2026-02-11 at 19.31.05.png]]