
## AWS CLI

- unified tool to manage your AWS services
- support for all OS

### Instance Info

```
aws ec2 describe-instances
```

### Create a new EC2 Instance
```bash
$ aws ec2 run-instances
--image-id ami-087c17d1fe0178315 --count 1 --instance-type t2.micro --key-name [[YOUR KEY NAME]]
```

### Create a new S3 bucket
```bash
$ aws s3 s3://bucket-name
```
ls
```bash
aws s3 ls
```
rm
(?)
cp
(?)

### SDK
Programmatically create instances


## Infrastructure as Code

Benefits
- speed and simplicity
- configuration consistency
- cost efficiency
- elimination of configuration drift
	- configure the same way every single time
- unified deployment

Tools
1. terraform
2. cloudformation(by aws)
3. ansible
4. puppet
5. chef

### Terraform
- IaC tool created by HashiCorp
- wide range of providers
	- aws, Azure, gcp
- no need to write step-by-step instructions
- declarative syntax

#### Imperative vs. Declarative
- Imperative
	- you have to tell in a specific order how to create it
- Declarative
	- You just tell it what you want and it creates it for you

#### Key Features
- provider agnostic
- resource graph
- state management
- modularity
- community and ecosystem

### Steps of Terraform
1. Init
	1. initialize the local terraform environment and execute it once per session
2. plan
	1. Dry run the execution
	2. Compare the terraform state with the as-is store in the cloud, build, and display an execution plan. 
	3. Creates a read-only deployment plan
3. apply
	1. apply the plan
4. destroy
	1. remove the resources that are governed by this specific session

### Core concepts
- providers
- resources
- modules
- data sources
- input variables
- output values

#### Providers
- a plugin that interacts with the various APIs required to create, update, and delete resources
- each provider adds a set of (?)

#### Resources
- most important element
- it describe what you want in the configuration
- describes one of more infrastructure objects, such as storage, virtual networks or compute instances

#### Variables
- used as parameters to input values at run time to customize our deployments
- things that you want to reuse like ids 

#### Data Source
- represents data that is fetched from outside of terraform itself, or data that is computed within terraform but not managed by it
- reads the data from a given provider, and use that data elsewhere in the terraform configuration
- helps to eliminate hard-coding of information
- for example:
	- AMI could change every year and automatically fetch every time.

#### Locals
- values enable the creation of expressions or values that can be easily referenced within the terraform module
- eliminate the duplication of hard coded values (?)

#### Output Values
- information about your infrastructure available on the command line, and can expose information for other terraform configurations to use
- This is because you don't know some variables until you create the instance
- for example: ip addresses of servers

