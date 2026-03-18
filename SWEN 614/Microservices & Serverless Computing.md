
## Monolithic
Architecture of software such that entire application is deployed all at once in a single unit.

### CON
(?)


## Microservices

- Alternative to Monolithic where each sections of application is a sub unit as a service.

### Monolithic to Microservices

Synchronous Restful application
- Create restful endpoint between all small services

Asynchronous protocol
- Use message queues 
- Has a queue for managing request between each services
- AWS SQS(Simple Queue Service)

How to run this quickly

- EC2 is not great because you are still dealing with managing workloads and infrastructures


## Serverless Computing

### AWS Fargate
- Container orchestration 
- serverless compute engine for containers

## AWS Lambdas
- function to function of tasks
- built-in fault tolerance
- scale to support the rate of requests without configuration
- pay per execution(number of calls and duration)
- VERY CHEAP!
- Triggers: way to call a lambda
- Common triggers:
	- message in a queue
	- file in a file location
	- API on the API gateway is called
- language agnostic
	- python, go, java, etc

### Pros
- no worry about hardware
- scale automatically
- cost-efficient

### Cons
- Resource Limits
- execution time limits
- memory limits
- testing can be more challenging
- AWS services can lead to vendor lock-in

## Considerations
- microservice can reduce the team management complexity but no dimish the need for team comms
- can choose different tech stack for different components and lead to problem of non-uniform application design and architecture and possibly increase maintenance costs
- inter-servie comms needs to be secured to avoid security breach
- testing of such applications can be more complex in comparison to monolith applications

