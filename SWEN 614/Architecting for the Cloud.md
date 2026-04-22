
Things to consider:

|                                                                    |
| ------------------------------------------------------------------ |
| 1. Plan for scale and service limits early                         |
| 2. Select regions based on cost, latency, and service availability |
| 3. Design with cost awareness from the beginning                   |
| 4. Design For Failure, Not Perfection                              |
| 5. Use Auto-scaling to handle demand and recover from failures     |
| 6. Automate operational and administrative tasks                   |
| 7. Manage infrastructure using Infrastructure as Code (laC)        |
| 8. Apply strong identity and access management                     |
| 9. Establish CI/CD pipelines for consistent deployments            |
| 10. Monitor and observe application and system behavior            |
| 11. Prefer managed services where appropriate                      |
| 12. Regularly evaluate and evolve the architecture as usage grows  |


## Best Practices

### Plan for scale and service limits early
- have limits
- design scaling
- apps often fail during success; hitting limits causes outages
- avoid surprises:
	- run load tests and capacity and breaking points
	- plan upgrades early based on growth goals
- Examples:
	- ec2 instance limit
	- s3 request rate limit
	- (?)

### Select regions based on cost, latency, and availability
- common mistake
- choose a region based on the location of the customer
- no aws region is created equal
- (?)

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

### Design for failure not perfection

- plan for outages
- software issues
- hardware failures
- data center issues
- if physical host hardware fails, all ec2 instances goes down

### Auto-scaling to handle demand and recover from failures

- launch every EC2 instance in an auto scaling group
- auto-scaling groups monitor instances and replace
- scale based on alarms for metrics 
- define actions when thresholds are met
- common(?)

### Automate operational and administrative tasks

- reduce manual steps
- automate anything repetitive or error-prone
- why matters
	- manual process doens't scale
	- human error is leading cause of outages and downtime
- examples:
	- automated environment setup scripts
	- scheduled backups and cleanup tasks
- Look into Ansible


### Use IaC and manage infra

- define infra through code
- manage infra changes like software updates for consistency
- ensures repeatability, version control, and easy rollback
- keeps environments consistent and auditable
- use tools like terraform, cloudformation
- quickly recreate environments for testing or disaster recovery

### Apply strong identity and access management

- grant only the minimum permissions needed for each task
- regularly review and update access policies to maintain security
- avoid overly broad permissions as they pose a serious security risk
- permission often remain unchanged when going live leading to high-profile breaches
- always follow **principle of least privilege**

### CI/CD pipelines for consistent deployments

- automate build, test, and deployment processes to reduce manual steps
- speeds up deployments and improves reliability
- minimizes configuration drift and human error
- use tools list GitHub Actions for deploying infrastructure and application code
- Include automated testing before production releases

### Monitor and observe application and system behavior

- track metrics, logs, and alerts
- include application-level metrics such as error rates, latency, and transaction success rates
- implement distributed tracing to understand request flow across services
- use centralized logging for visibility into failures and performance bottlenecks

### Prefer managed services where appropriate

- aws managed services instead of self-managed solutions where possible
- focus on business logic, not infra maintenance
- managed services reduce operational overhead
- example
	- RDS for databse instead of db servers
	- lambda for compute instead of EC2 instances

### Regularly evaluate and evolve the architecture as usage grows

- architectures must evolve to keep pace with growth and change
- what works today may fall short tmr
- static design often turn into bottleneck overtime
- (?)

### Bonus: Build vs Buy, making the right choice

- dev team often struggle to accept that buying can sometimes be better than building
- for commodity solutions, evaluate SaaS optiions and compare costs objectively
- buy may involve higher upfront costs, it often reduces long-term support and maintenance burdens
- avoid "Not Invented Here Syndrome" (NIHS)
	- tendency to reject extern solution
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