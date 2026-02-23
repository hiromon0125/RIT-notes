
### Netflix
- Netflix spends around $28 million per month on AWS 
- using more than 100k server instances
- not clear on the type of servers 
- Key point here is to manage costs effectively and pay only for what you need and nothing more
![[Screenshot 2026-02-16 at 18.52.00.png]]

## Elasticity

- **Elasticity** is key principle of cloud computing
- defines the ability to scale infra up or down based on demand
- provide rules to programming(?)

## Auto-Scaling Adavntages

- Fault tolerance
	- Auto-Scaling can detect when an instance is unhealthy terminate it
	- then launch an instance to replace it
- improved availability
	- helps ensure that your applcition always has the right amount of cap
- Cost management
	- when server loads are low, auto-scaling allows comanies(?)


## Auto-Scaling Usage Patterns

### Start Small, grow fast

- common scenario for statup or early growth companies
- don't init with a lot of capacity
- cost is minimized until the demand grows
- as usage increases, the infra(?)

### Predictable Burst

- Usually triggered by an event that may anticipated
- example: a new releast of a product or tickets
- over time, and interest may wane and return to normal levels

### Unpredictable Burst

- Similar to a predicable burst, but not tied to an event that can be predicted
- example: could be a major events such as a terrorist attack

### Periodic Processing

- tied to an application that may be used heavily during a specific time of year and then forgotten completely
- a tax applciation or a system processing annual reviews

### Sizing for each scenario

- sizing for the anticipated peak and not adjusting can be expensive and is a common example of "cloud waste"(over provisioning)
- manually monitor and adjust the infra up or down
	- too much work
- thats where auto-scaling comes in

### Auto-Scaling Overview

- Auto-scaling enables resouce group to dynamically increase or decrease capacity in response to realtime requests

### Launch Template
- contains AMI, instance type, etc.
- Auto-scaling group
	- collection of EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management
- Auto-Scale plan
	- defines "when and how" to scale the Auto-Scaling group
	- Health check time
	- desired number(starting number), min/max number
- Load Balancers
	- are the solution around this

## Auto-scaling Plan

- Auto-scaling strategies:
	- always maintain current instance level
		- configure and maintain a specified number of running instances at all time
	- scale based on schedule
	- scale based on demand
		- CPU, memory, etc.

## scaling Policies

- Scaling out - launching new instances
- Scaling in - temrinating instaces

### Horizontal vs Vertical Scaling

- Vertical Scaling(Scale up)
	- Incerase power of exisiting server by adding more resources
	- Pros - Quick and easy fix to add capacity
	- Cons - costly and introduces single point of failure. Server might reach a limit where it cannot be further upgraded
- Horizontal Scaling(Scale up)
	- increase power of existing system by adding more servers
	- pros - less costly, fault tolerant
	- cons - requires load balancing, can be more complex to manage

![[Screen Recording 2026-02-16 at 19.10.19.mov]]

## Load Balancing

- load balancing involves distributing network traffic
- with load balancing alone you need to know ahead of time how much capacity you need so you can keep addiional instances running and registered(?)

> [!NOTE]
> You need both load balancer and auto-scaling to make it work

## Challenges

- there is no one-size fits all, so it takes time to determine the optimal plan
- an instance takes time to start up so lowering the threshold of when to add more instances is a good idea
- measuring a CPU utilization is just one metric and you may consider other factors
	- memory or Network traffic
	- messaging accumulating on queues
- AWS offers other tooling to help with this

Auto-Scaling with Terraform
![[Pasted image 20260216192300.png]]

### Auto-Scaling & Netflix

 - Auto-Scaling plan and policy does Netflix use?
 - "Scryer"
	 - tool that predicts what the needs will be prior to the time of need and provisions the instances based on those predictions
	 - differs from AWS Auto-Scaling, which is more reactive
 - AWS Auto-Scaling address their needs?
	 - Rapid Spike in demand - Instances take 10-15 min on startup 
	 - Outages - A sudden drop in request
- Hybrid approach - Best of both worlds
	- Scryer alone does not replace AWS Auto-Scaling
	- Netflix has indicated that by combining Scryer + AWS
- Netflix
	- (?)

