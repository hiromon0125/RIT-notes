
## Attempt 1
result: 17/25

Questions I got wrong:

3: How does AWS ensure that no single customer consumes an unsustainable proportion of available resources?
- A: AWS imposes default limits on the use of its service resources but allows customers to request higher limits.
- Other options:
	- AWS allows customers to consume as much as they're willing to pay for, regardless of general availability
	- AWS imposes hard default limits on the use of its service resources
	- AWS imposes default limits on the use of its services by Basic account holders; Premium account holders face no limits.(what I chose)
- Chapter 2
- AWS applies usage limits on most features of its services. However, in many cases, you can apply for a limit to be lifted

4: The AWS Free Tier is designed to give new account holders the opportunity to get to know how their services work without necessarily costing any money. How does it work?
- A: You get free lightweight access to many core AWS services for a full 12 months.
- Other options:
	- You get service credits that can be used to provision and launch a few typical workloads.(What I chose)
	- You get full free access to a few core AWS services for one month.
	- You get low‐cost access to many core AWS services for three months.
- Chapter 2
- The Free Tier offers you free lightweight access to many core AWS services for a full 12 months.

5: AWS customers receive “production system down” support within one hour when they subscribe to which support plan(s)?
- A: Business and Enterprise
- Other options:
	- Enterprise
	- Developer and Basic
	- All plans get this level of support
- Chapter 3
- “Production system down” support within one hour is available only to subscribers to the Business or Enterprise support plans.

6: AWS customers get access to the AWS Trusted Advisor best practice checks when they subscribe to which support plan(s)?
- A: Developer, Business, and Enterprise
- Other options:
	- All plans get this support
	- Basic and Business
	- Business and Enterprise(What I chose)
- Chapter 3
- All support plans come with full access to Trusted Advisor except for the (free) Basic plan

10: Which of the following describes a methodology that protects your organization’s data when it’s on‐site locally, in transit to AWS, and stored on AWS?
- A: Client-side encryption
- Other options:
	- Server-side encryption
	- Cryptographic transformation
	- encryption at rest
- Chapter 5
- End‐to‐end encryption that protects data at every step of its life cycle is called client‐side encryption.

12: Which of these is the primary benefit from using resource tags with your AWS assets?
- A: Tags make it easier to identify and administrate running resources in a busy AWS account
- other options: 
	- Tags enable the use of remote administration operations via the AWS CLI
	- Tags enhance data security throughout your account(what I chose)
	- Some AWS services won't work without the use of resource tags
- Chapter 6
- Resource tags—especially when applied with consistent naming patterns—can make it easier to visualize and administrate resources on busy accounts.

16: Which of the following AWS storage services can make the most practical sense for petabyte‐sized archives that currently exist in your local data center?
- A: Saving to an AWS Snowball device
- Other options:
	- Saving to a Glacier Vault(what I chose)
	- Saving to S3 bucket
	- Saving to Elastic Block Store(EBS) Volume
- Chapter 8
- You can transfer large data stores to the AWS cloud (to S3 buckets) by having Amazon send you a Snowball device to which you copy your data and which you then ship back to Amazon.

18: What's the best and simplest way to increase reliability of an RDS database instance?
- A: Enable Multi-AZ
- Other options:
	- Increase the available IOPS
	- Choose the Aurora database engine when you configure your instance
	- Duplicate the database in a second AWS Region
- Chapter 9
- Multi‐AZ will automatically replicate your database in a second Availability Zone for greater reliability. It will, of course, also double your costs.

21: What is Amazon's Git-compliant version control service for integrating your source code with AWS resources?
- A: CodeCommit(got this right but guessed)
- Other options:
	- Code Build
	- Code Deploy
	- Cloud9
- Chapter 11
- CodeCommit is a Git‐compliant version control service for integrating your source code with AWS resources.

23: What is Amazon Athena?
- A: A service that permits queries against data stored in Amazon S3
- Other options:
	- A service that permists processign and analyzing of real-time video and data streams
	- A NoSQL database engine(what i chose)
	- A Greece-based Amazon Direct Connect service partner
- Chapter 13
- Amazon Athena is a managed service that permits queries against S3‐stored data.

