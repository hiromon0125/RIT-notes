

## Motivations of using cloud computing

1. utility-based pricing
	1. this is more on the focus of usage patterns
2. reduction in capital expenditures
	1. CapEx(Capital expenditures)
		1. large up-front invenstment to be written down as a gamble
	2. OpEx(Operational expenditures)
		1. pay as you go(?)
	3. moving from datacenter to cloud is increasing CapEx. 
3. improved resource utilization and allocation
	1. traditional data center vs cloud

## Datacenters vs Cloud

- Operating a large data center which cost 10 to 25 million per year to run
- would moving to the cloud save money?
	- probably not
	- but depends on your server utilization

## traditional data center vs cloud

traditional dc
- must purchase hardware upfront for anticipated peak performance
- must plan for disaster recovery separately at the department or project level
- stand-by server would be needed to act as a backup
- each system must be individually patched to keep up to date with security and performance patches
cloud
- ability to manage servers much easier
- expand servers as needed?

## Virtualization 

partitioning
- run multiple OS on one physical machine
isolation
- each virtual machine is isolated from its host physical system and other virtualized machine
- if one virtual instance crashes, it doesn't affect the other virtual machines
encapsulation
- save the entire state of a virtual machine to files
- move and copy virtual machines as easily as moving and copying files
hardware independence
- provision or migrate any (?)

before virtualization
- single OS per machine
- software and hardware tightly coupled
- running multiple applications on same machine often creates conflict
- underutilized resources
- inflexible and costly infra

after
- hardware independence of operating system and applciation
- virtual machines can be provisioned to any system
- can manage OS and application as a ??

## Hypervisor

Hosted:
- can run multiple OS but doesn't have to access to hardware
- more overhead
- mroe overhead
- if host crashes you lose access to guest OS

Examples: VMWare Workstation, VMware parallel

Native:
- Installs directly onto a computer
- hypervisor has direct access to all hardware and features
- used for servers because of their security an portability to move frrom hardware to hardware

## What can be virtualized

anything could be virtualized like storage server, ??
## Advantages of virtualization
- server consolidation
- infra optimization
- (?)
- (?)
- (?)

Upgrading servers can be easily be done on the cloud.
Any time the server has to be shutdown causes downtime.

## Disadvantages of virtualization

- upfront cost and complexity
- performance overhead
- licensing and compliance challenges
- security and isolation risks







