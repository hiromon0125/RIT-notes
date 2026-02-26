
## API protocols and Data Exchange

- REST using json
- SOAP using xml

### RESTful guideline/compliance practice
- uniform interface
	- stick to finite set of operations of the application protocol
	- CRUD
- client-sever
	- client and server should be able to evolve separately
- stateless
	- server will not store anything about the latest request
- cacheable
	- caching provides performance improvement as the same resource
- layered system
	- client cannot ordinarily tell whether it is connected directly to the end server

## OPEN API Specification
- previously called "swagger specification"
- language agnostic
- why important?
	- declarative resource spec
	- clients can understand and consume services without knowledge
	- applications implemented based on OpenAPI

## API Gateway

- API gateway is a server that is the single entry pioint into the system 
- it sits between the clients and services and acts as a reverse proxy
- routes requedsts from client to the service
- it encapsulates the internal system architecture and provides an API that is tailored to the client
- features
	- authentication
	- input validation
		- ensures the request contains necessary info to complete requests
	- metrics collection
		- ideal place to collect analytics such as measure how many requests a user can make
		- rate limiting
	- response transformation
		- mobile devices might need less data than desktop devices
		- gateway can be used to account for this effectively a unique api to each client type


## API Gateway Policy
- Rule that enforces when processing incoming and outgoing requests
- some of the features previously mentioned may be enforced via policies
- policies are typically used in four specific areas: authentication authorization, security, and traffic management
- Inbound Policy
	- incoming request rule
- Outbound Policy
	- outgoing request rule


## Load balancers
- api gateway would route traffic to a load balancer
- load balancer distribute requests
- balancing methods
	- Round robin
		- circle around in order and does not care about server status
		- drawback: assumes every workload is the same
	- Least outstanding requests
		- selects target with the least amount of pending request

