![[SWEN 614/lectures/Infrastructure as Code.pdf|Infrastructure as Code]]
![[SWEN 614/lectures/Terraform Advanced.pdf|Terraform Advanced]]

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
- each provider adds a set of resource types and data sources that terraform can mange

#### Resources
- most important element in the language
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
- eliminate the duplication of hard coded values that are used multiple times throughout the project

#### Output Values
- information about your infrastructure available on the command line, and can expose information for other terraform configurations to use
- This is because you don't know some variables until you create the instance
- for example: ip addresses of servers

## Challenges
1. reusability of configurations
2. manage and separate infra config for dev, test, staging, prod
3. manage shared state using remote backends and avoid team conflicts
4. securely handle sensitive values like credentials, keys, and secrets
5. manage variables consistently across code, scripts, and pipelines

## Terraform Modules
- **modules** are containers for multiple resources that are used together 
- consists of .tf and/or .tf.json files kept together in a directory
- terraform module allows you to create logical abstraction on the top of some resource set
- allow you to group resources together and reuse it somewhere else
- every project has at least one module that acts as the root module. 

## Terraform workspaces
- workspaces allow you to manage multiple environments in one configuration(dev, staging, etc.)
- each workspace has its own instance of state, 
- meaning we can use the same configuration code across environments without creating conflicting state files
- other benefits
	- environment isolation
	- configuration reusability
	- simplified workflow
- `terraform workspace new dev`
- `terraform workspace select dev`
- `terraform workspace show`
	- creates things like 
		- `vars_dev.tfvars`
		- `vars_test.tfvars`
		- ...
- `terraform apply -var-file=vars_dev.tfvars`

### Modules vs workspaces
- modules
	- designed for organizing and reusing code
	- encapsulation of resources, variables, and outputs
	- designed to be reusable across multiple projects
	- you can call the same module with different variables to deploy similar resources across different environments or accounts
- workspaces
	- doesn't add functionality to the infrastructure
	- allow reuse of the same modules without changing code
	- use `terraform workspace` command

## Terraform State Management
- terraform uses a state file `.tfstate` to keep track of infrastructure resources it manages
- the state file stores metadata that maps real-world resources to configuration enabling terraform to detect changes and maintain dependencies
- State Storage
	- local state
		- stored on the user's local machine for single-user environments but can be risky if lost or corrupted
	- remote state
		- stored in a shared backend, such as s3, enabling collaboration and providing features
- State Locking
	- prevents multiple users from simultaneously modifying the state file, which can lead to conflicts, corruption, or resource duplication
	- DynamoDB was commonly used, but this was recently switched to S3
	- note that this will prevent multiple devs from working at the same time

### why is this important
- resources tracking
	- allows terraform to understand the current state of managed resources
- dependency mangement
	- manages dependencies between resources, making updates
- team collaboration
	- remote state storage allows users to share and manage the infra together

### Protecting sensitive input variables in terraform

- terraform configurations often include sensitive information, such as passwords 
- teraform supports marking variables as sensitive, ensuring they are not displayed in CLI output or logs
- Sensitive variables help prevent accidental exposure of confidential data during plan and apply stages
- use the `sensitive=true` attribute in variable definition to keep values hidden

![[Pasted image 20260204190944.png]]
![[Pasted image 20260204190955.png]]
![[Pasted image 20260204191024.png]]

## Terraform and Variables
- **templatefile** is a terraform function that reads a tempalte file, fills in placeholders with variable values, and produces a formatted string output
- used for generating dynamic content
- create a file with `.tpl`
- you can then place `${app_environment}` that will then gets replaced during execution
- in .tf file you can use 
- ```
  template("script_path"), {
	app_environment: "halo"
  }
  ```


### Misc
terraform fmt
- formats your files

terraform validate
- validates your files and checks for syntax and identify internal consistencies




