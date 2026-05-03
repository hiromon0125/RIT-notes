![[SWEN 614/lectures/Security.pdf|Security]]

- Security is top concern with any organization moving to cloud

## Cloud data breaches
- Top concerns
	1. security misconfig
	2. Lack of adequate visibility into access settings and activities
	3. identity and access management
	4. permission errors

## Whos at fault?

### Capital One breach
- 100 mil customers have their data compromised by hacker after a cloud misconfig
- hack was able to access to credit apps, SSN, and bank account numbers
- Amazon: for its part, pointed to the admission of misconfig in the court document and the capital one statements. 
- Spokesman told news outlet that Capital One's data was not accessed through a vulnerability in AWS systems
- This example is a proof that companies have to learn how to deploy secure systems in the cloud

## Cloud provider's responsibility
- protecting the network through automated monitoring systems and robust internet access, to prevent DDoS attacks
- performing background checks on employees who have access to sensitive areas
- decommissioning storage devices by physically destroying them after end of life
- ensuring the physical and environmental security of data centers 

## Cloud customers' responsibility
- implement proper access management that restrict access to AWS resources like S3 and EC2 to minimum
- encrypt network traffic to prevent attackers from reading or manipulating data
- configuring a firewall for your virtual network that controls incoming and outgoing traffic with security groups and access control lists(ACLs)
- Manage patches for the OS and additional software on virtual machines
	- AWS will not update EC2 instance for you

## Shared Responsibility Model

### Infra
- AWS is responsible for security ==**of**== the cloud
- Customer is responsible for security ==**in**== the cloud
![[Pasted image 20260503165111.png]]
### Service Models
![[Pasted image 20260503165328.png]]

## Compliance
- HIPPA
	- series of regulatory standards that outline the lawful use and disclosure of protected health info
- SOX
	- Sabanes Oxley Act
	- est. rules to protect the public from fradulent or erroneous practices by corps
- GDPR
	- General Data Protection Regulation
	- requires businesses to protect the personal data and privacy of EU citizen for transactions that occur within EU member states
- PCI Compliance
	- Payment Card industry
	- credit card related payment systems
- ITAR
	- International Traffic in Arms Regulations
	- set of US department of state regulations that control the export of defense and military tech to safeguard national security and further its foreign policy objectives
- Questions that needs to be answered for compliance
	- where in the world is the operation
		- during the audit, you need to prove the location of your data
		- and measures in place to protect it
	- how to enforce access controls
		- orgs must be able to demonstrate the level of access that each user has
		- how those levels are maintained
		- crucial for a cloud provider to have sound access control in place and implement them properly
	- how are you protecting the data
		- what type of encryption does a cloud provider use
		- how and when its applied
		- companies are responsible for the protection of data in motion and data at rest using proper encryption techniques

## Security on AWS
- securing access to AWS services and resources
	- identity and access management via IAM
- securing applicatinos
	- Cognito
- protecting data through encryption
	- key management service(KMS)
- Compliance
	- CloudTrail

## Access Controls
- when signing up for AWS account you are the root user
- the root has unrestricted access to all resources on AWS
- permission are not restricted in any way
- As a best practice, you should lock the root user access so no one can access it
- create users/groups that have more restrictive access

## Identity and Access management(IAM)
- a service that helps administrate control access to AWS resources
- IAM administrators control who can be authenticated and authorized to use certain AWS resources
- IAM provides
	- AWS IAM users and their access
	- Manage IAM roels and permissions
	- Manage federated users and their permissions
- IAM user
	- used to authenticate people accessing your AWS account
- IAM Group
	- a collection of IAM users
- IAM Role
	- An IAM entity that defines a set of policies for making AWS Service requests
	- They are not associated with a specific IAM user or group
- IAM Policy
	- used to define the permission for a user, group, or role

### IAM Policy
- IAM Policy is entity that when attached to identity or resource, defines their permissions
- Policies are stored in AWS as JSON docs
- Policies can be either [managed](<#Managed Policies>) or [inline](<#Inline Policies>)

### Managed Policies
- standalone identity-based policies that you can attach to users, groups, and roles in AWS account
- there are two types of managed policies
	1. AWS Managed Policies
		1. AWS's created and managed policies
	2. Customer Managed Policies
		1. Custom policies that is created by me in the account

### Inline Policies
- attached directly to a specific user, group, or role
- it has a 1 to 1 relationship where the policy applies only to that identity
- if the identity is deleted, the inline policy is deleted automatically with it

### IAM Role
- an IAM identity that you can create in your account that has specific permission
- roles can be created to act as a proxy to allow users or services to access resources

### Best practices
- 2 factor for privileged users
- Use policy conditions for extra security
- remove unnecessary credentials(Principle of Least Privilege)
	- regular audit user credentials 
	- remove credentials if they are not in use
- Use AWS-Defined policies to assign permissions
	- major benefit of using policies is the auto-update functionality as new or updated policies are introduced
- Use Groups to assign permissions to IAM users
	- Managing permissions is not only easier but more secure and manageable

## AWS Cognito
- user identify and data synchronization service
- Manage user data across multiple mobile or connected devices
	- user data can be app pref, game states, etc
- Provide a secure user directory that scales to hundreds of million users
- two main components of Cognito are user pools and identity pools

### User pools
- user directory in Cognito that stores and manage app's users
- handles user registration, authentication, and account recovery
- users can sign in directly through the user pool or federate through external identity providers
- Cognito user pool stores user profiles, but delegates authentication to the external identity provider

### Identity Pools
- authorization by controlling what AWS resources a user can access
- identity pools create unique identities for users and assign them temporary AWS credentials through IAM roles
- After authentication, user can directly access other AWS services based on their permissions by identity pool's IAM roles

## KMS
### Protecting Data with Encryption
- to encrypt data message, you need a key to start an encryption and you need a key to decrypt the message
- if someone is listening and hijacks the data, they can't read it

### Encryption on AWS
- encrypting data at rest is vital for regulatory compliance to ensure that sensitive data saved on disks is not readable by any user or application without a valid key
- some compliance regulations such as PCI and HIPAA require that data at rest be encrypted throughout the data lifecycle
- AWS KMS is a fully managed service taht makes it easy to create and control encryption keys on AWS can then be utilized to encrypt and decrypt data in safe manner

#### Securing Linux encrypted File System with KMS
1. the admin uses KMS to encrypt a secret password
2. The encrypted password file is stored in S3 bucket
3. During instance startup, the EC2 instance copies the file from S3 to local disk
4. The instance then uses KMS to decrypt file and retrieve the secret password
	1. The password is used to configure a linux encrypted file system
	2. all data written to that file system is encrypted on disk using AES-256

### Integration
- several AWS services like S3, EBS, RDS, and Dynamo integrate with KMS
- combine this with IAM policies to provide layers of control

## Cloud Trail
- service that enables governance, compliance, operational auditing, and risk auditing of AWS account
- Provides event history of AWS account activity, including actions taken through the AWS Management Console, SDKs, CLIs, and AWS services
- provide logs of all keys usage may require regulatory and compliance needs
- event history simplifies security analysis, resource change tracking, and troubleshooting to even detect unusual activity in AWS accounts

## How secure are you?
- AWS trusted advisor is a tool that provides you real time guidance to help you provision your resources following AWS best practices
- For Security, trusted advisor scans for the following:
	- Checks security groups for rules that allow unrestricted access to specific ports
	- Checks security groups for rules that access to resource
	- Checks buckets in S3 that have open access permissions

