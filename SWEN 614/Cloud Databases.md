
## SQL vs NoSQL


### Amazon RDS Service(DB Service)
- Amazon RDS is a managed database service
- Fully managed DB does a lot of the handling of maintaining DB and keeping it up.
- managed Database admin tasks
- scalability
- security
- high availability
	- manages replicas
	- read replication

### Aurora(SQL)
- serverless
- auto-scale built-in
- High availability
	- Multi-primary
- Downsides
	- more expensive and vendor lock-in

### Amazon RDS (managed) vs Self-hosting

- considerable amount of time and staff know-how to build a comparable relational database environmental based on virtual machines
- Takes best employee to mange ourselves which is more expensive

### Challenges with SQL
- with SQL db there is overhead for complex select update and deletes
- can be costly to scale
- sql database don't do well with __unstructured data__

## NoSQL

- NoSQL is an approach to database that represents a shift away from traditional relational databases
- KV DB
	- redis or caching databases
- Document DB
- Graph DB
	- Neo4j, Amazon Neptune
- Column store DB

### DynamoDB
- KV DB and DocumentDB
- managed db, multi-region, multi-master, durable databse
- security builtin 
- backup and restore
- Data types:
	- single valued
	- multi-valued: string, number set, binary set
	- Document: list and maps
- (?)

### Challenges
- don't conform to ACID 
- can not rely on "eventual consistency"


## when to pick which
SQL:
- transactions and data consistency
- storring relationships
NoSQL:
- handling a large number of read/write operations
- running data analytics

## Providers
![[Pasted image 20260318192420.png]]



