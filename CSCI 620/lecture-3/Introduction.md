

## Why we need column oriented
Reading just one column is slow because its not laid out next to each other.

## Document-oriented
We can have different types of data and are not able to use  a uniform way to store them.

## Graph-oriented
The document databases allows us to represent data as graphs
We are usually interested in paths or more complex queries that involve visiting several, unbound nodes.

## Structure of NoSQL
Database --> Collection --> Document


## Creating a database
```nosql
use hospMng
```

## Creating a collection
```nosql
db.createCollection("bills", [
{option, value, ...}])
```
