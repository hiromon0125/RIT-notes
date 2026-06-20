
## free tier

You get certain amount of duration to run instances
such as ec2(t2.micro) for 750 hours(31 days) per month with no charge

if you go over you only get charged for the amount of duration you went over.

## Online Pricing calculators

- Simple Monthly Calculator
	- calculator that you can enter infra configuration and receive a cost breakdown on all services used
- Total cost of ownership calculator
	- calculator that you can enter the current on-premise server configuration and compare them on the AWS services equivalent.

## Service Limits

pretty arbitrary limit set by amazon on how many instance you can run at the same time.
Most limits are soft limits and can be requested for increased amounts but hard limits are set for exact limit and can not be expanded.

## AWS Billing Dashboard

The main Billing & Cost Management dashboard shows helpful spending summary for current month so far along with some forecast of the cost at the end of the month

## AWS Budgets

tool for tracking a specified set of events so that when a preset threshold is approached or passed an alert will be sent.

## Monitoring costs

- Cost Explorer
	- lets you build graphs to visualize account's infra costs. Even lets you customize view and lets you filter account cost events. 
- Cost and Usage Reports
	- you can configure reports from billing dashboards and export into data files like CSV format and sent to S3 buckets. From S3 you can set up quicksights and access and process the data as its generated.
- Cost Allocation Tags
	- tags are meta data identification elements representing a resource and its actions
	- tags can be used to organize and track resources allowing to visualize and better understand resources being used. 
	- Different kinds of tags
		- Resource tags -- often used in busy accounts to help administrators quickly identify the purpose and owner of a particular running resource.
		- Cost allocation tags -- only relevant to billing tools and wont show up in the context of any other AWS resource or process. Similarly to resource tags will lets you identify resources that are automatically generated when resources are created and user-defined cost allocation tags.

## AWS organizations

formerly known as Consolidated Billing, AWS Organizations allows to centralize the administration of multiple AWS accounts owned or controlled by a single company. 