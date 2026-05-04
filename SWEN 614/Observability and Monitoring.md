
## Why observability matters
- What happens after you deploy your application successfully to the cloud?
- deployment is only the start
- real success depends on reliability performance and user experience over time
- without observability, teams lack the context to understand why systems fail, making root cause analysis slow and reactive
- comprehensive data from logs, metrics and traces provides actionable insight into system behavior, enabling faster troubleshooting and informed decisions
- proactive detection of anomalies and dependencies help prevent cascading failures and ensures resilience at scale

## Terms
- Monitoring
	- detects known failure conditions using predefined thresholds
	- commonly tied to basic availability or error-rate mtrics
	- things like CPU is too hot
- Observability
	- enables understanding of why failures or degradations occur
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

Flow of incident:
1. monitoring alarm fires
2. observability tools investigate and provide deep insights into system behavior to uncover the root cause
3. insights feedback improve monitoring thresholds and refine system design for greater resilience

## Why cloud system require observability
- Cloud applications are distributed, dynamic and failure-prone by design
- failure are often partial and affect only subsets of users
- SLIs can degrade gradually before outages occur
- no observability means:
	- teams miss early warning signs that SLOs are at risk
	- troubleshooting becomes slow, reactive and guess-driven
	- SLA breaches lead to costly consequences and damaged trust

## Monitoring and metrics in the cloud
- Cloudwatch is a service for monitoring AWS resources and the applications you run in AWS
- It allows devs to monitor their application in the cloud
- (?)
- CloudWatchis. a central metrics repo
- AWS services publish metrics to cloudwatch and you can retrieve stats from these metrics
- Metrics can be used to calculate statistics and visualize data in the CloudWatch console
- you can also set alarms to automatically stop, start or terminate EC2 instances when specific conditions are met

## Advantages
- unified dashboard
- cost optimization
- detailed insights
- performance optimization

## Limitations
- short data retention - stored for 2 weeks
- aws only
- limited visualization
- low frequency in monitoring
- higher cost for detailed monitoring
- scalability concerns

## Cloud Monitoring Solutions
- CloudWatch provides core monitoring for application health and performance within AWS
- For broader visibility many organizations integrate thirdparty applications


## advanced cloudwatch usage
- builds on cloudwatch metrics and alarms
- core compontns
	- cloudwatch logs
	- aws x-ray
	- correlation
- (?)

## AWS X-Ray
- distributed tracing on AWS
- X-Ray traces units of work as they flow through distributed applications, not limited to HTTP API calls
- traces units of work across services
- (?)

## OpenTelemetry
- OpenTelemetry(OTel) is a vendor-neutral framework for collecting key observability signals
	- Metrics
	- Logs
	- Traces
- defines how telemetry data is generated, structures, and propagated
- eliminates vendor lock-in and ensures portability across platforms

## OpenTelemetry + AWS End-to-End Flow
- application emit telemetry using OpenTelemetry SDKs
- AWS services consume this data:
	- Metrics --> cloudwatch for monitoring SLO 
	- traces 
- (?)


