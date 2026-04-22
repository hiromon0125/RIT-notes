
Big Dat Analystics
- searching for meaningful patterns in an ocean of data
- why?
	- cost reduction
	- faster smarter decision
	- innovation and personalization
- Data lake is centralized repository that can store structured or not data at any scale
- typically serves as a single source of data
- (?)


## How do they work?

1. incoming flow represents multiple raw data archives ranging from emails spreadsheets social media content, etc.
2. the reservoir of water is dataset
3. the outflow of water is the data being analyzed 
4. (?)

## Challenges

- raw data is stored with no oversight of the content
- for a data lake to make data usable it needs to be clearly defined
- without thees elements data cannot be found, or trusted resulting in a "Data Swamp"

# Data Warehouse

- similar to lakes and used for storing big data but they are not interchangeable terms
- data warehouse is a repository for structured, filtered data that has already been processed for a specific purpose
- Data lake is a vast pool of raw data
- Redshift is amazon's service of data warehouse
- Hadoop is data lake

## Hadoop
- provide the foundation fro Datalake architectures as it provides cheap way to organize data
- (?)

## Data Lakes in the cloud on AWS
- data lakes built on Amazon s3
- there are several native AWS services to run big data analytics, artificial intelligence, machine learning to gain insights from your unstructured data sets
- another benefit of using s3 is that it can be shared via permissions

## Data Lake House
- merge of Data Warehouse and Data Lake
- all of this is living in one place
- this reduces data duplication
- allows both structured and unstructured data

## Data Visualization
- Data Visualization is the practice of presenting data in pictorial or graphical formats such as charts, graphs, etc
- (?)

## Data Exploration & visualization tools on AWS
- Amazon Quicksight
- Amazon Sagemaker and EMR notebooks
- Amazon athena
	- interactive query service that's capable of seamlessly using standard structured queries

## Spark SQL
- spark's interface for working with structured data
- (?)
- Features
	- Spark SQL queries are integrated with Spark programs.
	- both DataFrames and SQL support a common way to access a variety of data sources, like Hive, Parquet, (?)
- Dataframe
	- a distributed collection of data organized into named columns, similar to a table in a relation database
	- (?)


## Main Idea
- Data Lake
	- Unstructured bucket for any kind of data regardless of structured or not
	- People can get lazy, so make sure this is always well maintained
- Data Warehouse
	- **Structured** database for structured data that can be used to query and used with a good performance
	- Typically people would bring data from the Lake and clean the data and store it into the warehouse and do work here.
- Data Lakehouse
	- Both structured and unstructured data can be stored here, much more preferable because a structured data can directly reference data in unstructured data
	- Look into snowflake for more information on this.