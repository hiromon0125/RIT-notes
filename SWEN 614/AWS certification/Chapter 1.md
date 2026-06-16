
Building infrastructure in:
1. AWS servers
	1. Scalability and reliability from the most trusted corporation for managing servers
	2. globally distributed compute resources
	3. with a price that is very comparable to workloads running locally
	4. professionally secured infrastructure ensured by the world leading tech team
	5. infrastructure available on request
	6. only billed for what you used
	7. testing software application can be done minimally and at anytime
	8. low capex, high opex
2. On-premises servers
	1. Massive capex
	2. everything will have to be done by hand and in house
	3. Local infrastructure managed by in house IT team
	4. new infrastructure may take days or even months with budget approval and integration etc.
	5. billed for what you bought and continue to up keep
	6. testing software requires planning of allocating resources and potentially reconfiguring the entire system
	7. high capex, low opex

key important factor is 
```
You manage the content in the cloud while AWS manages the cloud itself.
```
the in house solution will force the team to not only work on security in the cloud but also security of the cloud itself.

## Server Virtualization

- virtualization is done via their **hypervisor** running in their physical servers
	- Speed: 
		- New VMs can be provisioned in few minutes
		- restarting instances could happen in the same exact amount of time
	- Efficiency:
		- utilizes the entire server with densely packed VMs on a single physical machine
		- reduces over allocated resources for VMs thats barely using the resource

## Cloud Platform Models

### IaaS
Infracture as a service that offers infrastructure to their customers and simulates an existence of running your own machine in the cloud.

### PaaS
Platform as a Service offers simplification of managing your own infrastructure by allowing users to go through PaaS's own interface. AWS also has their own PaaS within their list of services such as Elastic Beanstalk, ECS.

### SaaS
Software as a Service offers services meant to be accessed by end users with a direct software product. 

## Scalability

The ability to add new resources to the currently allocated resource

## Elasticity

The ability to quickly provision and tear down services and resources. Often though of as auto-scaling and increasing flexibility.