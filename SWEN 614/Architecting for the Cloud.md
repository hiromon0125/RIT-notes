![[SWEN 614/lectures/Architecting for the Cloud.pdf]]

Things to consider:

|                                                                 |
| --------------------------------------------------------------- |
| 1. Plan for scale and service limits                            |
| 2. Regions based on cost, latency, and service availability     |
| 3. Cost awareness                                               |
| 4. Design for failure, Not Perfection                           |
| 5. Configure Auto-scaling for demand and recovery from failures |
| 6. Automate operation and administrative tasks                  |
| 7. Manage infra using IaC                                       |
| 8. Apply IAM                                                    |
| 9. CI/CD pipelines                                              |
| 10. Monitoring and observability                                |
| 11. Prefer managed services where appropriate                   |
| 12. Regularly evaluate and evolve architecture                  |


## Best Practices

### Plan for scale and service limits early
- have limits
- design scaling
- apps often fail during success; hitting limits causes outages
- avoid surprises:
	- run load tests and capacity and breaking points
	- plan upgrades early based on growth goals
- Examples:
	- ec2 instance limit during rapid auto-scaling
	- s3 request rate limit under heavy uploads
	- RDS Connection limits exceeded during peak traffic

### Select regions based on cost, latency, and availability
- common mistake
- choose a region based on the location of the customer
- no AWS region is created equal
	- pricing
	- feature availability
- if you think there is a certain AWS service in the near future, make sure its available in the certain region of operation
- some configuration could almost double depending on the region of choice
- Even within the US, some EC2 instances in N. Cali will cost 30% more compared to N. Virginia
### Design with cost awareness from beginning
- app costs more than it earns will erode profits
- choose the right instances
	- how many do we need
	- appropriate size
- tracking and calculate costs
- account for all cost factors:
	- data transfer
	- ELB processing
	- instance hours
	- EBS/S3 storage
	- API calls
	- Lambda executions and memory

Look into
- [[Understanding Cloud Costs#How to estimate the cost|Cloud Cost]]

### Design for failure not perfection
- plan for outages
- software issues
- hardware failures
- data center issues
- if physical host hardware fails, all ec2 instances goes down
- outage can occur due to
	- host software issues
	- hardware failures
	- data center issues
- If physical host's hardware fails, all EC2 instances on that host go down

Look into
- [[Disaster Recovery & Resiliency#Disaster Recovery|Disaster recovery & Resiliency]]

### Auto-scaling to handle demand and recover from failures
- launch every EC2 instance in an auto scaling group
- auto-scaling groups monitor instances and replace
- scale based on alarms for metrics 
- define actions when thresholds are met
- common use case:
	- fixed-size groups for resiliency

Look into
- [[Auto-Scaling#Elasticity|Auto-Scaling & Elasticity]]
- [[Microservices & Serverless Computing]]

### Automate operational and administrative tasks

- reduce manual steps
- automate anything repetitive or error-prone
- why matters
	- manual process doesn't scale
	- human error is leading cause of outages and downtime
- examples:
	- automated environment setup scripts
	- scheduled backups and cleanup tasks

Look into 
- Ansible

### Use IaC to manage infra

- define infra through code not manual steps
- manage infra changes like software updates for consistency
- ensures repeatability, version control, and easy rollback
- keeps environments consistent and auditable
- use tools like terraform, CloudFormation for provisioning
- quickly recreate environments for testing or disaster recovery

Look into
- [[Infrastructure as Code|IaC]]
- [[Terraform]]
### Apply strong identity and access management

- grant only the minimum permissions needed for each task
- regularly review and update access policies to maintain security
- avoid overly broad permissions as they pose a serious security risk
- IAM misconfiguration can expose critical resources
- Use separated roles for developers and administrators
- permission often remain unchanged when going live leading to high-profile breaches
- always follow **principle of least privilege**
- Common errors
	- making s3 buckets public
	- s3 are secure by default but loosened during development
	- Permission often remains unchanged when going live, leading to high-profile breaches
- Always follow **Principle of least privilege**
	- grant access only to those who are trained and authorized
	- review and tighten permissions before prod

Look into 
- [[Security and Compliance#Identity and Access management(IAM)|IAM]]

### CI/CD pipelines for consistent deployments

- automate build, test, and deployment processes to reduce manual steps
- speeds up deployments and improves reliability
- minimizes configuration drift and human error
- use tools list GitHub Actions for deploying infrastructure and application code
- Include automated testing before production releases

Look into
- [[DevOps & Pipelines]]

### Monitor and observe application and system behavior

- track metrics, logs, and alerts
- include application-level metrics such as error rates, latency, and transaction success rates
- implement distributed tracing to understand request flow across services
- use centralized logging for visibility into failures and performance bottlenecks
- Examples:
	- CloudWatch alarms for error rate or response time thresholds
	- Custom dashboards for critical workflows
	- Log insights to identify anomalies and trends
- Metrics to consider
	- Customer Experience Metrics
		- response time, load times, error 500s
	- System Metrics
		- CPU, memory, disk I/O, request queue length
	- Business Metrics
		- revenue, costs
	- Observability Tip
		- CloudWatch and AWS X-Ray to monitor metrics, trace requests, and set alerts for critical thresholds

Look into
- [[Observability and Monitoring]]

### Prefer managed services where appropriate

- use AWS managed services instead of self-managed solutions where possible
- focus on business logic, not infra maintenance
- managed services reduce operational overhead
- example
	- RDS for database instead of db servers
	- lambda for compute instead of EC2 instances

Look into
- [[Service models]]

### Regularly evaluate and evolve the architecture as usage grows

- architectures must evolve to keep pace with growth and change
- what works today may fall short tomorrow
- static design often turn into bottleneck overtime
- continuous improvements drive scalability and long-term success
- Examples
	- migration from monolithic to microservices
	- containerization for better portability between clouds
	- replace custom-built components with AWS managed services as needs expansions

Look into
- [[Containerization & Orchestration]]
- [[Auto-Scaling]]
- [[Microservices & Serverless Computing]]

### Bonus: Build vs Buy, making the right choice

- dev team often struggle to accept that buying can sometimes be better than building
- for commodity solutions, evaluate SaaS optiions and compare costs objectively
- buy may involve higher upfront costs, it often reduces long-term support and maintenance burdens
- avoid "Not Invented Here Syndrome" (NIHS)
	- tendency to reject extern solution due to externally developed system
	- mindset can lead to unnecessary complexity and missed opportunities for efficiency



### Last activity

![[Pasted image 20260415191504.png]]

![[Pasted image 20260415191444.png]]


- multi-region
- auto-scaling
- convert ec2 to lambda
- API GW
- RDS db and shard the db for global deployment
- s3/cloudfront hosted frontend