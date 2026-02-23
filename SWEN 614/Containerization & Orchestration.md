2/18/2026

## EC2 instances and auto-scaling

- One drawback is it can be several seconds or minutes to get ready
	- consumes a lot of resources and may not be reuired if you need to do something small

## Virtual Machines

- EC2 are VMs using hypervisors creates and runs VMs
- within each VM runs one or more unique guest operating systems
- (?)

## Problem

- What is the likelihood that hardware on the bottom will run the software on the top:![[Screenshot 2026-02-18 at 18.53.09.png]]
- Answer: very difficult

## COntainers

- containers sits on top of a physical server and its host OS - like linux or windows
- each container shares the host OS kernel
- shared components are read only
- because they all sheare a single (?)

### Application Management

- on a typical server applications may be dependent on libraries that are shared with other applications
- updating one of these dependencies may inadvertently break another application
- Contianerizing applications allows each to run in isolation with impacting other containers
- dependencies can be safely (?)

## Examples of containers

- problem:
	- you have hundreds of applications runnning on premise and need to move everything to the cloud
- challenge
	- porting everything to the cloud requires recompiling and testing on new servers
	- idle servers can be expensive if they are not fully utilized
- Solution:
	- port to contianers and tst and validate
	- can move to the cloud with. no recompiling
	- becasue of this portability they could also be moved easily from one clooud to anotherand continue to work
	- this avoids __vendor lock-in__

- problem
	- replicate your testing environment for dozens of testers in the cloud
	- the environment which includes a database must be configured the same to avoid any testing discrepancies
- challenges
	- creating multiple and isolate test environments can be a time consuming and error prone process
- solution:
	- take the entire testing env and containerze it
	- multiple copies can be spun up aon demand and quickly torn down after testing to avoid cloud waste
	- (?)

## Container Solutions

- Docker is by far the most popular
	- based on linux kernel
	- 84% of the market share
- Othjers
	- podman
	- ...

## What is docker

open source containerization engine, which automates the packaging shipping and deployment of any software applications that are presented as lightweight portable and self-sufficient ...

### How does it work?

- docker client is the mahjor way that provides communication between many dockers users to docker
- sends the commands to the docker daemon, which carries them out
- docker registry is where the docker images are stored
- docker hub is a public registry that anyone can access and configure
	- services provided by docker for finding and sharing container images 
	- 9 mil images available 
	- some are functional but proceed with caution

## Amazon Elastic Container Registry(ECR)

- Fully managed container registry by AWS, enabling secure storage management, and deployment of Docker container images
- provides image encryption at rest
- (?)

## Containerization Activity

- Going back to the WordPress on Docker activity, what are some of the limitations with this solution
	- single point of failures
	- even if we separate wp to submodules like fe and be
- We can duplicate FE or BE