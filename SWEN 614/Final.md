
![[Midterm]]

New Content
13. [[Microservices & Serverless Computing]]  
14. [[Databases]]  
15. [[Security and Compliance]]  
16. [[Disaster Recovery & Resiliency]]
17. [[Big Data on the Cloud]]  
18. [[Data Lakes and Analytics]]  
19. [[Observability and Monitoring]]  
20. [[Generative AI on the cloud]]
21. [[Architecting for the Cloud]]

Microservices
- Monolithic architecture: entire application deployed in a single unit
	- inflexible, unreliable, unscalable, and poor development experience
- Microservice architecture: develop loosely coupled services that can be individually developed, deployed, and maintained
	- decouple, componetization, business capability decentralized governance
- good to use rest, async protocol, message queues(AWS SQS)
Serverless Computing
- AWS Fargate: for container orchestration and serverless compute engine for containers
- AWS Lambdas: single function to function of tasks, built-in fault tolerance and scale to support any rate of requests
	- language agnostic
	- pay per execution and very cheap
	- triggers: way to call lambdas
		- common types: message in queue, file in file location, API on the API gateway
	- be mindful of resource limits, execution limits, and memory limits
Databases
- SQL
	- AWS RDS: managed sql database, good scalability, security, and high availability(manages replicas and read replications)
	- Aurora: serverless sql db with auto-scale built-in with high availability with multi-primary, but more expensive and vendor lock-in
	- challenges: complex edit of data, and costly to scale and can't handle unstructured data
- NoSQL
	- approach to database that represents a shift away from traditional relational databases
	- KV DB: edis or caching database
	- Document DB: Mongo, DynamoDB
	- Graph DB: Neo4j, AWS Neptune
	- Column Store DB: Duck DB, ClickHouse
	- DynamoDB: KV and Document base. NoSQL of RDS
Security and Compliance
- Cloud Provider's res: Security **OF** the cloud, prevent DDoS, and robust internet access
- Cloud Customer's res: Security **IN** the cloud, resource access management, encrypt network traffic, configure firewall, manage patches for resources
- Compliances: HIPPA(Health), SOX(fin fraud), GDPR(Data Prot), PCI(Payment), ITAR(Export control)
- IAM: securing access to AWS services and resources
	- User: authenticate people accessing root AWS account
	- Group: a collection of IAM users mass apply permissions
	- Role: entity that defines a set of policies for making service requests associated with IAM user or group
	- Policy: used to define permission for user, group, or role; can be managed by AWS or custom by you
	- Best practice: 2 factor for privileged users, policy conditions for security, follow "Principle of Least Privilege", audit and remove unused credentials, use AWS policy where possible, and group assign permission to IAM users
- Cognito: application security
	- create app user identity and data synchronization between services
	- manage user data across multiple mobile or connected devices
	- User Pools: directory of users in Cognito that manage app's users, handles user registration, authentication, and account recovery; holds user profiles, but delegates authentication to external identity provider
	- Identity Pools: authorization by controlling what resource user can access; creates unique identities for users and assign them temp AWS credentials through IAM roles
- KMS: Key management service for encrypting and managing data for compliance or increased security overall
- CloudTrail: compliance service that enables governance, compliance, operational auditing, and risk auditing of AWS accounts
	- provides event history and logs for security analysis and resource change tracking
Disaster Recovery and Resiliency
- RPO: recovery point objective for maximum acceptable amount of data loss an application can have before causing measurable harm to business
- RTO: recovery time objective for maximum downtime an application can experience before measurable damage
- Pilot Light(active-cold): most critical core resource is configured and running in AWS; rapidly provision to full scale when needed
	- cheap but slower recovery
- Warm standby: more resource version of pilot light with further decrease in recovery time
	- usually set to lowest scale when not in use
- Multi-site(active-active): multi site, region, and runs same resource in parallel; load-balancers are also used to balance usage between sites
	- most costly but near-zero downtime as the secondary site can fully absorb the first one
- Resiliency: how much loss of resource the infra can with stand
- Testing Tools
	- Chaos engineering: specific discipline that allows engineers to test the worst possible case scenarios against distributed systems built from multiple services to find its resiliency
	- Steps: Plan experiment, contain blast radius, scale or squash
	- Chaos Monkey: randomly terminate instances in prod to ensure that engineers implement their services to be resilient
	- Simian Army: suite of failure inducting tools to add more capability to chaos monkey
	- Gremlin: hosted solution for chaos; Failure as a service
Deployment Strategy
- Blue Green: new production service is spun up(Green) and when tests pass, application traffic from Blue is routed to the Green and green becomes new prod
- Canary: Blue-Green with phased approach. Only subgroups of users are routed to the newly provisioned resource at a time
- Rolling: similar to canary but we slowly update old instances and roll out users onto it, no new instances are required
Big Data
- 4 V's: Volume(size of data), Velocity(speed of incoming data), Variety(kinds of data), Veracity(quality, reliability, and accuracy of data)
- Advantage with Cloud: agility, elasticity, cost efficient, and high data processing
- Hadoop: open source java based framework for storing and processing data and delegate work to 1000s of compute each with storage, hadoop frameworks:
	- Common: libraries and utilities needed by other hadoop mods
	- Distributed file system(HDFS): file-system for all machines
	- Yet another resource negotiator(Yarn): platform for managing computing resource
	- MapReduce(HMR): MapReduce programming model for large scale data processing
- Apache Spark: distributed processing engine for batch and streaming modes featuring SQL queries, graph processing, and ML. Runs in memory processing and is faster
- RDD: resilient, distributed, dataset; immutable list of data that can be partitioned to be computed individually
- Transformation: map function that create RDD from RDD and is lazily evaluated
- Actions: returns a result from RDD process that is eagerly evaluated
- AWS EMR is hadoop on amazon, preloaded with tools like Hive, Pig, and Spark and integrated with amazon services
	- Master(NameNode): main node that manages cluster by running SW comp to coordinate the distribution of data and tasks
	- Core(DataNode): node with SW comp that runs tasks and store data in HDFS on cluster
	- Task: optional node with SW Comp that runs tasks and not store data in HDFS
- Kafka: distributed event store and streaming-processing platform
	- Producers: send messages from partitions within a topic
	- Consumers: read messages from partitions within a topic
	- Partitions are batches of topic messages
	- Scalable by adding more consumer to consumer groups or making more consumer groups
	- each consumer is a microservices
	- scalable, low latency, durable and fault-tolerant
	- AWS has MSK(managed streaming kafka) but behind in versions
- Kinesis: similar to kafka but more integrated to AWS services
	- fully managed service that collect, processes, and analyzes real-time streaming data at any scale, enabling instance insights and actions from clients
- Spark Streaming: extension to spark API that enables scalable, high-throughput, fault-tolerant stream processing of live data streams and ingest from kafka and kinesis.
	- Uses Realtime processing, DStreams(Discretized streams) that batches the incoming stream with same RDD-based architecture
Big Data Storage
- Data Lake: centralized repository for any and all kinds of data
	- serves as a single raw source data and transformed data for reporting, visualization, and analytics without any structure or pre-defined organization
	- All incoming data and processed data will usually land here for further processing later
	- maintain organization to prevent loss of control of data coming in(Data Swamp)
	- S3 is a service for Lakes by AWS with good way to manage permission for sharing data
- Data Warehouse: similar to lakes and used for storing large **structured** data
	- usually these data is already some what processed
	- Redshift is a service managed by AWS
- Hadoop: foundation for Data Lake architecture and provide cheap way to organize data often called HDFS
	- data is ported into a Hadoop platform and business analytics and data mining tools are applied to the data in Hadoop cluster
- Data Lake House: merge of lake and warehouse that stores both unstructured and structured data reducing data duplication when both lake and ware house is needed
- Data Exploration tools: AWS Quicksight, AWS Sagemaker and EMR notebooks, AWS athena(interactive query service using standard structured queries)
- Spark SQL: spark interface for working with structured data like Hive, Parquet, JSON, and JDBC also has simple API like DataFrames and SQL support
Observability & Monitoring
- Monitoring: detects known failure conditions using predefined thresholds and reports them in a log or triggers
	- monitor metrics like CPU usage, error reports, unresponsive resources, etc.
	- can also alert developer when something comes up
- Observability: enables understanding of why failure or degradations occur that is essential for diagnosing complex, distributed failures
	- observe data such as users that are impacted, changes that was made, and bottlenecks
	- 3 pillars of observaibility: 
		- Metrics: foundation for measuring system health and primary source for SLI; used to assess compliance with SLO
		- Logs: provide detailed context once an alert fires
		- Traces: visualize the complete journey of a request and reveal dependencies and bottle necks across distributed services
- SLA(service level agreement): formal agreement to customers often with penalties when broken
- SLI(Service level indicator): measurable signal that reflects user experience like latency, error rate, and availability; low is good
- SLO(Service level objective): target/threshold for an SLI to be
- Monitoring acts as early warning and detection, Observability serves as the diagnostic tool
- CloudWatch: service in AWS for monitoring AWS resources and applications running on AWS
	- mainly for monitoring and can also support observability
	- Alarms can be set to automatically start, stop, or terminate EC2 instances when specific conditions are met
	- Alarm can also trigger Auto-scaling actions or send notifications with AWS SNS
	- limited to short data retention and only monitors AWS services with limited scalability
- Third-party solutions for broader visibility and integrate with other applications
- AWS X-Ray: traces distributed systems on AWS and traces units of work as they flow through distributed applications and not limited to HTTP API calls; looks like chrome dev tools
- OpenTelemetry: standard for telemetry data like metrics, logs, and traces that defines how data is generated, structured, and propagated
	- eliminates vendor lock-in and ensures portability across platforms
Gen AI
- training and inference with AI models at cloud scale
- cloud providers have pre-trained models hosted via APIs, removing complexity and overhead
- devs focus on integration, prompt design and workflows, manage security, cost, and governance
- Foundation models: large pre-trained models that serve as a base for many tasks; adaptable for text, image, and more through fine-tuning or APIs
- LLMs: specialized foundation models for text generation; commonly used for chatbots, assistants, and content creation
- Protect against Hallucinations
	- misinfo, reputational harm, and compliance failures
	- operation safeguards
		- guardrails and content filter: block unsafe or misleading outputs before delivery
		- logging and monitoring: tracking prompts and responses to detect patterns and prevent recurrent
		- prompt constraints: enforce structured input to guide models toward accurate results
- AWS Bedrock: fully managed service for deploying and integrating generative AI models that provide API access to multiple foundation models without need to managing the infra and supports customization and fine-tuning
	- Titan model: family of AWS-built foundation model designed to run cheaply on AWS infra(model types: text premier, image gen, text embedding)
Architecture decisions
1. Plan for scale and service limits
	1. design scaling into the architecture
	2. have limits but avoid surprises by running load tests and capacity and break points, plan upgrades early based on growth goals
	3. example: 
		1. ec2 instance limit for rapid auto-scaling
		2. s3 request rate limit under heavy uploads
		3. RDS connection limits exceeded during peak usage
2. Select Regions based on cost, latency, and service availability
	1. choose region based on location of the customer but also 
	2. keep costs in mind where some regions costs more to host the same resources
	3. some regions also have limited feature availability
3. Cost awareness
	1. balance revenue and costs as thinner margins will be riskier for the business
	2. choose the right instances and allocate appropriate size
	3. track and calculate costs for all factors
		1. data transfer
		2. ELB processing
		3. instance hours
		4. EBS/S3 storage
		5. API calls
		6. lambda executions and memory
4. Design for failure, not perfection
	1. Plan for outages like software and hardware issues
	2. and data center issues
	3. plan for disaster recovery and setup proper resilient systems
5. Configure Auto-scaling for demand and recovery from failures
	1. launch ec2 instances in an auto-scaling group and monitor instances with health checks and replace if unresponsive
	2. scale based on monitoring metrics and define actions when thresholds are met
	3. use systems like microservices and serverless architecture
	4. auto-scale with loadbalancer for infinite scaling
6. Automate operation and administrative tasks
	1. use CI/CD pipelines to automate any and all manual steps
	2. automate any repetitive and error-prone areas
	3. matters because manual process doesn't scale and human error is leading cause of outage and downtime
	4. automated environments and setup scripts or scheduled backups and cleanup tasks
7. Manage infra using IaC
	1. define infra through code and not manual steps that is harder to track
	2. manage infra changes like software updates for consistency and ensure repeatability, version control, and easy rollback
	3. keep environments consistent and auditable
	4. recreate environments for testing and disaster recovery plans
8. Apply proper IAM 
	1. grant only the minimum permissions needed for each task and avoid overly broad permissions as they pose a serious security risk
	2. regularly reviewing and update access policies to maintain security
	3. use separated roles for developers and administrators
	4. always follow principle of least privilege
9. CI/CD pipelines
	1. automate build, test, and deployment processes to reduce manual steps speeds up deployments and improve reliability
	2. minimize config drift and human error
	3. use tools like GitHub Actions
10. Monitoring and observability
	1. Monitor metrics, logs, and alerts
	2. like CPU usage, error rates, latency metrics, and transaction success rates
	3. implement distributed tracing to understand request flow across services
	4. use centralized logging for visibility into failures and performance bottlenecks
	5. types of metrics: customer experience metrics, System metrics, business metrics
	6. Observe using CloudWatch and AWS X-Ray to monitor metrics, trace requests and set alerts for crit thresholds
11. Prefer managed services where appropriate
	1. use AWS managed services instead of self-managed solutions where possible
	2. focus on business logic and not infrastructure maintenance
	3. managed services reduce operational overhead for example RDS, Bedrock, lambdas instead of manually configured EC2 instances
12. Regularly evaluate and evolve architecture
	1. architectures must evolve to keep pace with growth and change
	2. what works today may not work in the close future
	3. static solution will turn into bottleneck and fail overtime
	4. continuous improvements drive scalability and long-term success
	5. think about migration from monolithic to microservices, containerize for better portability between clouds, and replace custom-built components with AWS managed services as needs expansions