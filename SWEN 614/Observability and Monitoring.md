![[Cloud Observability.pdf]]
## Why observability matters
- What happens after you deploy your application successfully to the cloud
- deployment is only the start
- real success depends on reliability performance and user experience over time
- without observability, teams lack the context to understand why systems fail, making root cause analysis slow and reactive
- comprehensive data from logs, metrics and traces provides actionable insight into system behavior, enabling faster troubleshooting and informed decisions
- proactive detection of anomalies and dependencies help prevent cascading failures and ensures resilience at scale
## Terms
- Monitoring
	- detects known failure conditions using predefined thresholds
	- commonly tied to basic availability or error-rate metrics
	- answers questions like:
		- Is CPU running too high
		- Is something throwing an error
		- Is something not responding
		- Is anything Broken
	- alerting developers when something does go wrong
- Observability
	- enables understanding of why failures or degradations occur
	- essential for diagnosing complex, distributed failures
	- Answers questions like:
		- which users are impacted
		- what changed
		- where is the bottleneck
	- helps you understand why it went wrong
- SLA
	- service level agreement
	- formal promise to customers often with penalties if unmet
- SLI
	- service level indicator
	- measurable signal that reflects user experience
	- metrics like latency, error rate, availability
	- you want this low or quiet
- SLO
	- Service level objective
	- target/threshold for an SLI

## Monitoring and observability working together

- Monitoring acts as a early warning system
- observability serves as the diagnostic toolkit
- Most real-world incidents begin with a monitoring alert and end with observability-driven root cause analysis(RCA)

### Flow of incident:
1. monitoring alarm fires
2. Use observability tools to investigate
	1. provide deep insights into system behavior to uncover the root cause
3. insights feedback improve monitoring thresholds and refine system design for greater resilience

## Why cloud system require observability
- Cloud applications are distributed, dynamic and failure-prone by design
- failure are often partial and affect only subsets of users
- SLIs can degrade gradually before outages occur
- no observability means:
	- teams miss early warning signs that SLOs are at risk
	- troubleshooting becomes slow, reactive and guess-driven
	- SLA breaches lead to costly consequences and damaged trust

## 3 Pillars of Observability
1. Metric
	1. foundation for measuring system health
	2. Primary source for service level indicators(SLI)
	3. Used to assess compliance with Service Level Objectives(SLOs)
2. Logs
	1. Provide detailed context once an alert fires
3. Traces
	1. Visualize the complete journey of a request
	2. Reveal dependencies and bottlenecks across distributed services

## CloudWatch
- A service for monitoring AWS resources and the applications you run in AWS
- It allows devs to monitor their application in the cloud
- It is automatically configured to provide metrics on request counts, latency, and CPU usage
- CloudWatch is a central metrics repo
- Mainly monitoring service but can also support observability
- AWS services publish metrics to CloudWatch and you can retrieve stats from these metrics
- Metrics can be used to calculate statistics and visualize data in the CloudWatch console
- you can set alarms to automatically stop, start or terminate EC2 instances when specific conditions are met
- alarm can also trigger Auto-scaling actions or send notifications via Amazon SNS

## Advantages
- unified dashboard
- cost optimization
- detailed insights
- performance optimization

## Limitations
- short data retention - stored for 2 weeks
- aws only monitoring
- limited to basic visualization
- low frequency in basic monitoring
- higher cost for detailed and high frequency monitoring
- scalability concerns

## Cloud Monitoring Solutions
- CloudWatch provides core monitoring for application health and performance within AWS
- For broader visibility many organizations integrate thirdparty applications
	- Datadog, new Relic, Prometheus
	- combines CloudWatch data with other sources
	- Deliver holistic reporting across multi-cloud and on-prem environments
	- offer advanced analytics, visualization, and alerting capabilities

## Advanced CloudWatch Usage
- builds on CloudWatch metrics and alarms
- shifts the focus from "is it broken" to "why did it break"
- core components
	- CloudWatch logs
	- AWS x-ray
	- correlation
- Key point:
	- Observability transform alerts into actionable understanding

## AWS X-Ray
- distributed tracing on AWS
- X-Ray traces units of work as they flow through distributed applications, not limited to HTTP API calls
- traces units of work across services
- Metrics show that something is running slow, X-Ray shows where and why

## OpenTelemetry
- OpenTelemetry(OTel) is a vendor-neutral framework for collecting key observability signals
	- Metrics
	- Logs
	- Traces
- defines how telemetry data is generated, structures, and propagated
- eliminates vendor lock-in and ensures portability across platforms
- OTel standardize observability signals and platforms like AWS consume them

### OpenTelemetry + AWS End-to-End Flow
- application emit telemetry using OpenTelemetry SDKs
- AWS services consume this data:
	- Metrics --> CloudWatch for monitoring SLO 
	- traces --> AWS X-Ray for distributed request analysis
- Operational Flow:
	- OTel instruments application code
	- Metrics feed CloudWatch alarms(Monitoring)
	- Alarms signal SLO risk
	- Logs and X-Ray traces explain root cause
	- Insights improve future monitoring and system design
- this setup enables consistent observability across AWS and non-AWS systems


