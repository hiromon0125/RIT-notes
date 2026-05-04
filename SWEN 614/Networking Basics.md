![[Cloud Networking.pdf]]
## Virtual Private Cloud(VPC)

- A VPC defines the network boundary
- establishes which AWS can communicate
- VPCs allow isolation of workloads and environments
- Each VPC exists within a single AWS region
- a VPC serves as the foundational networking layer for AWS workloads
- AWS services operate within the context of a VPC

Purpose
- it defines a network boundary for applications in AWS
- it establishes where and how AWS resources can communicate with eachother
- multiple VPC can exist in a single AWS account for different use cases
- all subsequent networking constructs build upon the VPC foundation

## Subnet
- A logical subdivision of a VPC
- used to organize resources
- subnets define where aws resources are placed within VPC
- each subnet represents a portion of the VPC's IP address space
- instance like EC2 and load balancers are launched inside the subnets
- subnets provide the structural foundation for separating application components

### Public Subnet
- intended for resource that is reachable from the outside of the VPC
- They can commonly host entry-point components such as load balancers

### Private Subnet
- intended for resources that should not be directly accessible from the internet
- for example databases should not be accessible from the outside

### How to configure VPC and Subnet

- when you create the vpc or subnet you must specify an IPv4 CIDR block for the VPC
	- 10.0.0.0/24 -> 2^32 - 2^24 = 2^8 = 256 different ip addresses
		- 10.0.0.0 - 10.0.0.255
	- 10.0.0.0/20 -> 2^32 - 2^20 = 4096 different ip addresses 
		- 10.0.0.0 - 10.0.15.255
- rule with subnet is that the ip address range for the subnet must be within the ip address range of VPC that the subnet is inside of.
	- VPC: 10.0.0.0/24 is ip range of 10.0.0.0 to 10.0.0.255
	- Subnet #1: 10.0.0.0/16 is ip range of 10.0.0.0 to 10.0.0.63
	- Subnet #2: 10.0.0.64/16 is ip range of 10.0.0.64 to 10.0.0.127

### Route Table

- defines how network traffic is directed within a VPC
- the destination specifies the IP address range for the traffic
- The target specifies where that traffic should be sent
- To allow outbound internet access, a common route is 
	- Destination: 0.0.0.0/0 (all addresses)
	- Target: Internet Gateway attached to the VPC
- Route tables are what ultimately determine whether a subnet is public or private

rules and internet access
- each route consists of a destination and a target
- destination specifies the IP address range for the traffic
- target specifies where the traffic should be sent
- for example:
	- destination: 0.0.0.0/0, target igw-xxxxxx
	- destination here means all ipv4 addresses 
	- target is the internet gateway attached to the VPC

## Internet Gateway
- enables communication between a VPC and the public internet
- provides a connection point that allows resources in a VPC to send and receive internet access
- IGW is attached to the VPC level, not the individual subnets or resources
- each VPC can have only one internet gateway attached at a time
- an internet gateway must be used in conjuction with route tables to allow internet access

### Ingress
- traffic going into your system
### Egress
- traffic going out of your system
![[Pasted image 20260128192420.png|500]]

## NAT Gateway

- allow resources in a private subnet to initiate outbound connections to the internet or AWS services
- enables egress-only internet access for private resources
- internet traffic cannot initiate inbound connections to resources behind a NAT gateway
- NAT gateway is commonly used for private application servers that need external access
- NAT Gateway improve security by keeping private resources unreachable from the internet
- **NAT Gateway must be deployed in a *public subnet***
- An **Elastic IP address** is required and associated with the NAT Gateway
- private submnets route outbound internet traffic to the NAT Gateway via route tables
- NAT Gateways are managed highly available AWS services

## VPC Endpoint

- enables private connectivity between a VPC and supported AWS services
- traffic between the VPC and AWS services does not traverse the public internet
- VPC endpoints allow access to services such as S3

### Summary

![[Pasted image 20260128193112.png|200]]
