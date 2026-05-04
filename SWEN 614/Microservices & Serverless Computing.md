![[Microservice and Serverless.pdf]]
## Monolithic
- Architecture of software such that entire application is deployed all at once in a single unit.
- designed to be self-contained where components of the program are interconnected and interdependent
- tightly coupled architecture, each component and its associated components must be present in order for code to be executed or compiled
- Challenges
	- inflexible
	- unreliable
	- unscalable
	- blocks continuous deployment
	- slow development
	- not fit for complex and large applications

### When to move to the cloud
- life and shift
	- simply moving to the cloud may be more costly and likely would have other limitations
- for starters, you would decompose the problem into smaller services, which would allow more teams to work on this
- although you would prefer to use a single programming language, this is not a hard fast requirement
- as this will all be new, you need to be able to respond very quickly to fixes and/or modifications and release quickly
- this needs to be scalable but also cost effective

## Microservices
- Alternative to Monolithic where each sections of application is a sub unit as a service.
- modeled around a business domain
- each of these services is responsible for discrete task and can communicate with other services through simple APIs
- Microservices architecture enables developers to build loosely coupled services which can be developed, deployed, and smaintained independently
- Features
	- decoupling
	- componetization
	- business capabilities
	- Continuous Delivery
	- Decentralized Governance

### Monolithic to Microservices
- Split the monolith to build a microservices architecture
- Create a bunch of Synchronous Restful application
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
- microservices can reduce the team management complexity but no dimish the need for team comms
- can choose different tech stack for different components and lead to problem of non-uniform application design and architecture and possibly increase maintenance costs
- inter-service communications needs to be secured to avoid security breach
- testing of such applications can be more complex in comparison to monolith applications

