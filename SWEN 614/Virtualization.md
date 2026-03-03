1/21/2026
![[SWEN 614/lectures/Virtualization.pdf|Virtualization]]

## Motivations of using cloud computing

1. utility-based pricing
	1. this is more on the focus of usage patterns
2. reduction in capital expenditures
	1. CapEx(Capital expenditures)
		1. large up-front investment to be written down as a gamble
	2. OpEx(Operational expenditures)
		1. day to day expense which can much more easily be increased or decreased as business needs dictate
	3. moving from datacenter to cloud is increasing CapEx. 
3. improved resource utilization and allocation
	1. traditional data center vs cloud



## Datacenters vs Cloud

- Operating a large data center which cost 10 to 25 million per year to run
- would moving to the cloud save money
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
- expand servers as needed
- estimated that more than 80% of on-premise enterprise workloads are overprovisioned
- by retaining excess capacity as these workloads are moved to the cloud, companies can actually increase the costs of running them by up to 15%
- Instead of becoming more efficient, they're merely transferring their existing inefficiencies to a new location(and paying more)

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
- provision or migrate any virtual machine to any physical server

before virtualization
- single OS per machine
- software and hardware tightly coupled
- running multiple applications on same machine often creates conflict
- underutilized resources
- inflexible and costly infra

after
- hardware independence of operating system and application
- virtual machines can be provisioned to any system
- can manage OS and application as a single unit by encapsulating them into virtual machines

## Hypervisor

Virtual machine manager(VMM)
Hosted:
- can run multiple OS but doesn't have to access to hardware
- more overhead
- more overhead
- if host crashes you lose access to guest OS

Examples: VMWare Workstation, VMware parallel

Native:
- Installs directly onto a computer
- hypervisor has direct access to all hardware and features
- used for servers because of their security an portability to move from hardware to hardware

## What can be virtualized

anything could be virtualized like 
- storage server
- server virtualization
- desktop virtualization
- application virtualization
- network virtualization
## Advantages of virtualization
- Server consolidation
	- Reduce hardware, power, and maintenance costs
	- simplifies server management and minimizes physical infra footprint
- Infra optimization
	- Dynamically allocates compute, memory, and storage based on workload demands
	- Enables resource scaling without disrupting other running virtual machines
- Flexibility and scalability
	- Provides the ability to create, clone, and deploy new VMs rapidly
	- easily add or remove VMs without the need for additional physical hardware
- More efficient IT operations
	- Streamlines software updates, maintenance, and network management
	- Improves backup and disaster recovery through fast failover to standby VMs

Upgrading servers can be easily be done on the cloud.
Any time the server has to be shutdown causes downtime for traditional methods, but virtualization allows almost no downtime.

## Disadvantages of virtualization

- upfront cost and complexity
	- initial setup can require investment in virtualization infra, including robust hardware, storage, and licensing for hypervisors or management tools
	- Requires planning and technical expertise to configure and optimize virtual environments effectively
- performance overhead
	- Certain high-performance or latency-sensitive workloads may still benefit fro dedicated physical hardware
	- Resource contention between virtual machines can affect performance if not properly managed
- licensing and compliance challenges
	- Licensing models can be complex in virtual env
	- tracking and managing compliance across dynamic virtualized system can be more difficult
- security and isolation risks
	- virtual env introduce new attack surfaces, including hypervisor vulnerabilities and VM sprawl
	- ensuring strong isolation between VMs and consistent patching across hats is critical to maintain security







