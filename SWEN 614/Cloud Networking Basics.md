
## Virtual Private Cloud(VPC)

- A VPC defines the network boundary
- establishes which AWS can communicate
- VPCs allow isolation of workloads and environments
- (?)

## Subnet
- A logical subdivision of a VPC
- used to organize resources
- subnets define where aws resources are palced within VPC
- (?)

### Public Subnet
- intended for resource that is reachable from the outside of the VPC
- They can commonly host entry-point components such as load balancers

### Private Subnet
- intended for resources that should not be directly accessible from the internet
- for example databases should not be accessible from the outside

### How to configure VPC and Subnet

- when you create the vpc or subnet you must specify an IPv4 CIDR block for the VPC
	- 10.0.0.0/24 -> 2^32 - 2^24 = 2^8 = 256 different ip addresses for the VPC
- rule with subnet is that the ip address range for the subnet must be within the ip address range of VPC that the subnet is inside of.
	- VPC: 10.0.0.0/24 
	- Subnet #1: 10.0.0.0/16
	- Subnet #2: 10.0.0.64/16

### Route Table

- A sroute table defines how network traffic is directed within a VPC
- the destination specifies the IP address range for the traffic
- The target specifies where that traffic should be sent
- To allow outbound internet access, a common route is 
	- Destination: 0.0.0.0/0 (all addresses)
	- Target: Internet Gateway attached to the VPC
- Route tables are what ultimately determine whether a subnet is public or private

## Internet Gateway
- enables communication between a VPC and the public internet
- provides a connection point that allows resources in a VPC to send and receive internet access
- (?)


### Ingress
- traffic going into your system
### Egress
- traffic going out of your system
![[Pasted image 20260128192420.png|500]]

## NAT Gateway

- allow resources in a private subnet to initiate outbound connections to the internet or AWS services
- enables egress-only internet access for private resources
- (?)
- **NAT Gateway must be deployed in a *public subnet***
- An **Elastic IP address** is required and associated with the NAT Gateway

## VPC Endpoint

- enables private connectivity between a VPC and supported AWS services
- traffic between the VPC and AWS services does not traverse the public internet
- VPC endpoints allow access to services such as S3

### Summary

![[Pasted image 20260128193112.png|200]]
