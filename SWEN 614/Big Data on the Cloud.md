
Concepts:
- Hadoop & Spark Basic


- Cloud compute provides scalable infra and services to store manage and analyze massive datasets
- BD drives(?)

# Hadoop
- open source java based framework for storing and process data
- designed to divide from single servers to 1000 of servers
- each with a computation and storage
- provides massive storage for any kind of data

## Framework
- hadoop common
	- libraries and utility needed by other hadoop mods
- hadoop distributed file system
	- file-system for all machiines
- hadoop yarn(Yet Another Resource Negotiator)
	- a platform responsible for manageing computing resources
- Hadoop MapReduce
	- (?)

## HDFS Commands

- CLI commands to control in hadoop

# What is MapReduce
- classical approach for bigdata
- MapReduce is algo for processing BigData set in parallel
- similar to map and reduce in js and python
- Map job is where a block of data is read and processed to produce key=value pairs as intermediate outputs
- the output of a mapper or map job is input to the reducer
- Syntax
	- very complex
	- Apache Hive
		- relational database built on Hadoop
		- allows SQL queries on over 300 PB
	- Pig
		- simple scripting language for queries and data manipulation
		- will conssume any data that you feed it

# Apache Spark
- distributed data processing engine for batch and streaming modes featuring SQL queries, Graph processing and machine learning
- programmable in java, python, scala, R
- run cluster through Yarn or in standalone mode
- spark is used for real-time data processing and time-consuming data operations
- spark runs 10-100x times faster due to in-memory processing

## Spark Architecture(RDD)
- RDD:
	- resilient
	- distributed
	- dataset
- RDD is readonly and immutable
- think of rdd as the list of data
- each dataset in RDD is divided into logical partitions which may be computed on different (?)

## operations
- Transformations
	- create RDD from each other
	- functions that take an RDD as the input adn produce one of many RDDs as the output
	- lazily evaluation
	- methods
		- Map
			- transformation op
			- applies a function to each element of RDD and returns the result as a new RDD
- Actions
	- returns some result from RDD
	- reduce aggregates using a function
		- function that takes two arguments and reduce to one

## Streaming

- processing data in real time
- as the data comes in you are already analyzing it before saving to disk

### Batch
- Batch data is collected over time
- handles a large batch of data
- process over all or most of the data
- latency is few minutes to hours
- lengthy process is good

![[Screenshot 2026-04-06 at 18.48.08.png|400]]

## Tech
- apache kafka
	- developed at LinkedIn
	- distributed event store and streaming-processing platform
	- core architecture is an immutable log of messages
	- it can deploy on bare metal hardware
- amazon kinesis


## Kafka
- messages stored in topics
- **Producers** send messages to topic from which consumers read it
- **Consumers** read messages from partitions within a topic
- Kafka Consumers are part of a Consumer group
- Partitions are batches of topic messages
- Scalable by adding more consumer to consumer groups
- Even scalable as consumer groups
- Each consumer is similar to a microservice
	- a system centers on an orders service which exposes a REST interface to POST and GET orders
	- posting an order creates an event in Kafka that is recorded in the topic orders

difference between message queue
- kafka lays a strip of topic messages down and the consumers just reads through the strip
### Benefits
- scalable
- low latency
- durable and fault-tolerant
- reliable and flexible performance


## Kinesis
- Similar to kafka but more integrated to aws services
- data producers enter the records into kinesis data streams
- aws offers kinesis producer library for simplifying (?)

Difference betwen kinesis and kafka
- kinesis uses shards
- kafka uses topics
- but they are just the same thing
- Kafka can be run anywhere so its much better in our opinion
- Amazon also has Amazon MSK
	- amazon's managed kafka service
	- but is behind in versions

## Spark Streaming
- extension to spark
- allow ingesions from kafka, kineis
### How does it work?
- Realtime processing
- DStreams(Discretized streams)
	- batches the incoming stream
	- Each stream of data is an RDD
- Still an RDD-based architecture

