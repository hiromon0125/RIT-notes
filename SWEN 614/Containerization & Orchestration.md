2/18/2026
![[SWEN 614/lectures/Containers in the Cloud.pdf|Containers in the Cloud]]

## EC2 instances and auto-scaling

- One drawback is it can be several seconds or minutes to get ready
	- consumes a lot of resources and may not be required if you need to do something small

## Virtual Machines

- EC2 are VMs using hypervisors creates and runs VMs
- within each VM runs one or more unique guest operating systems
- VMs with different operating systems can run on the same physical server
- Each VM has its own binaries, libraries, and applications that it services
- VM could be several gigs in size

## Problem

- What is the likelihood that hardware on the bottom will run the software on the top:![[Screenshot 2026-02-18 at 18.53.09.png]]
- Answer: very difficult

## Containers

- containers sits on top of a physical server and its host OS - like linux or windows
- each container shares the host OS kernel
- shared components are read only
- because they all share a single, common operating system, it is much simpler for things like bug fixes and patches
- should only be megabytes in size use less resources than VMs
- possible to fit multiple containers onto a single server

### Application Management

- Challenge
	- on a typical server applications may be dependent on libraries that are shared with other applications
	- updating one of these dependencies may inadvertently break another application
- Solution
	- Containerizing applications allows each to run in isolation with impacting other containers
	- dependencies can be safely updated without breaking applications in other containers

## Examples of containers

- problem:
	- you have hundreds of applications running on premise and need to move everything to the cloud
- challenge
	- porting everything to the cloud requires recompiling and testing on new servers
	- idle servers can be expensive if they are not fully utilized
- Solution:
	- port to containers and test and validate
	- can move to the cloud with. no recompiling
	- because of this portability they could also be moved easily from one cloud to another and continue to work
	- this avoids __vendor lock-in__

- problem
	- replicate your testing environment for dozens of testers in the cloud
	- the environment which includes a database must be configured the same to avoid any testing discrepancies
- challenges
	- creating multiple and isolate test environments can be a time consuming and error prone process
- solution:
	- take the entire testing env and containerze it
	- multiple copies can be spun up aon demand and quickly torn down after testing to avoid cloud waste
	- This saves time and money and ensures a consistent testing environment

## Container Solutions

- Docker is by far the most popular
	- based on linux kernel
	- 84% of the market share
- Othjers
	- podman
	- ...

## What is docker

open source containerization engine, which automates the packaging shipping and deployment of any software applications that are presented as lightweight portable and self-sufficient ...

### How does it work

- docker client is the major way that provides communication between many dockers users to docker
- sends the commands to the docker daemon, which carries them out
- docker registry is where the docker images are stored
- docker hub is a public registry that anyone can access and configure
	- services provided by docker for finding and sharing container images 
	- 9 mil images available 
	- some are functional but proceed with caution

## Amazon Elastic Container Registry(ECR)

- Fully managed container registry by AWS, enabling secure storage management, and deployment of Docker container images
- provides image encryption at rest
- IAM-based access controls, and vulnerability scanning with Amazon Inspector
- DockerHub vs ECR
	- DockerHub has public repositories, ECR repositories are private by default enhancing security
	- DockerHub is a globally accessible service which ECR operates within AWS regions, ensuring low-latency access
	- DockerHub's requires on a standalone account, ECR uses AWS credentials for authentication

## Containerization Activity

- Going back to the WordPress on Docker activity, what are some of the limitations with this solution
	- single point of failures
	- even if we separate wp to submodules like fe and be
- We can duplicate FE or BE

![[SWEN 614/lectures/Container Orchestration.pdf|Container Orchestration]]
## Orchestration

- automation of all aspects of coordinating and managing containers
- features
	- provisioning and deployment of container
	- redundancy and availability of containers
	- scaling up or down to spread application load evently across host infra
	- external exposure of services running in a container with the outside world
	- load balancing between containers
	- health monitoring containers and servers
- describe configuration using yaml or json file
- configuration file specifies information such as the location of the image networking between container storage mounting, etc
- the container orchestration tool
- automatically schedules the deployment of containers 
- benefits
	- scalability
	- simple and fast deployment 
	- resilience
		- container orchestration tools can automatically restart or scale
	- improved security
- tools
	- k8
	- openshift
	- docker swarm
	- google k8 engine
	- amazon elastic container services (ECS)
- K8s
	- open source container orchestration platform for automating deplyment, scaling and management of containerized application 
	- abstracts infrastructure management and ensures scalability
	- features
		- service discovery and load balancing
			- dns, ip pods and balances
		- automated rollouts and rollbacks
			- deploy new pods and configurations
		- secret and config management
		- storage orchestration
		- automatic bin packing
		- self-healing
	- core concepts
		- **Cluster** is a group of machines(nodes) running k8s
		- A **worker node** is a single machine in the cluster responsible for running workloads
		- **Pods** smallest deployable unit in k8s, containing one or more containers
		- **Namespace** is a logical partitions within a cluster isolating resources
![[Pasted image 20260223185534.png]]

- Master(control plane) a set of components that collectively manage the state of the k8 cluster
	- API server: central management interface. validates requests processes configuration data etc.
	- ETCD distributed key-value store that serves as k8s backing store
	- Controller Manger ensures the desired state of the cluster by running controllers that handle tasks like node monitoring replication, and maintaining service endpoints
	- scheduler assigns pods to nodes based on resource availability, constraints, and scheduling policies, ensuring efficient placement and workload balance in the cluster
- **Kubelet** serves as the cluster's "node agent" supervise node.
- **Kube-proxy** is set up on every node and enables services in a cluster connection


### Autoscaling

Horizontal pod autoscaling(HPA)
- adjusts the number based on metrics like CPU, Memory, or app metrics
Vertical pod autoscaling(VPA)
- dynamically adjust resources
Cluster Autoscaler
- adds or removes nodes in the cluster based on pending or underutilized pods

## Deploying an Application
- yaml file is used to define the desired state 
- describes kubernetes objects like pods deployments, services, and ConfigMaps
- Key comps
	- apiVersion specifies the K8s API version
	- kind specifies the type of k8s resource 
	- metadata specifies resource name, label and 
	- spec describe the desired state and configuration of the setup
- k8s sercrets securely stoire sensitive information likek passwords API keys or certificates separating configuration data

## Amazon Elastic Kubernetes Services(EKS)

- fully managed K8s service that simplifies the deployment
- integrates seamlessly with AWS
- 100% compatible with k8s 
- uses same tooling and APIs as self-managed kubernetes
- **Fargate** serverless compute 
	- works with ECS and EKS where the management of the underlying VM is performed by AWS and the user needs to manage the development and deployment of the application
	- Fargate is good for 
		- microservices with variable workloads
		- Dev/Test environments
		- Teams without infrastructure expertise
		- Applications requiring strict workload isolation
		- Batched processing jobs
	- EC2 is good for
		- High-performance applications
		- workloads requiring specific instance types
		- Stateful applications
		- Cost-sensitive production workloads
		- Applications requiring full host access
	- ![[Screenshot 2026-02-23 at 19.12.42.png]]
- Using terraform with EKS
	- setting up an Amazon EKS cluster involves creating and managing numerous interconnected resources. Terraform can streamline this complex process by defining your entire EKS environment in a single reusable configuration file
	- Other benefits
		- EKS requires resources like VPCs, IAM roles, security groups, node groups, and add-ons. Terraform allows you. to manage these resources cohesively,  avoiding manual steps across the AWS Management Console.
		- Automate the creation of over 60 resources required for a fully functional EKS setup, ensuring consistent configurations across environments
		- terraform automatically resolves dependencies between AWS resources, ensuring that components like VPCs and subnets are available before node groups or services are provisioned
		- With terraform modules, you can reuse configurations and easily extend your setup to include additional resources, such as monitoring or auto-scaling policies with minimal effort

## Amazon Elastic Container Services(ECS)

- managed container orchestration services to enable users to run it
- Cluster management
	- supports cluster of EC2 or Fargate
- Task definition
	- Defines the containers to run as a part of a task, including image, CPU, memory, and network configuration
- Services
	- ensures a specified number of tasks are continuously running and can be automatically scaled and managed
- Auto-Scaling
	- ECS supports Auto-scaling for both the containerized applications and the underlying EC2 instances, ensuring that applications can dynamically adjust to varying workloads

## EKS vs ECS

![[Screenshot 2026-02-23 at 19.15.45.png]]



