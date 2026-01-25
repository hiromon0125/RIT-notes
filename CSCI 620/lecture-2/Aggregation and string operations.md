
## Aggregation functions

- AVG
- MIN
- MAX
- SUM
- COUNT


## Basic aggregation queries

```sql
SELECT MIN(salary) 
FROM Doctor 
WHERE dob >= ‘1986-01-01’;
```

Minimum salary of people where dob is after 1981


## Grouping
```sql
SELECT lastName, AVG(salary) 
FROM Doctor 
WHERE salary < $170K 
GROUP BY lastName;
```


## Having

```sql
SELECT lastName, AVG(salary) 
FROM Doctor 
GROUP BY lastName 
HAVING AVG(salary) > $140K
```

Post condition after grouping
i.e. grabs groups of AVG salary greater than 140k



## String operation

```sql
SELECT ssn 
FROM Doctor 
WHERE lastName 
LIKE ‘Smit%’;
```

regex over string. 


## Indexes
Default is index for primary key

if you try to index everything it wont work because it will lead to poor performance.

### b+-tree
B-tree is used to remember the structure, but if the table gets edits the tree will need to be updated making it slowdown.

- =, >, <, >=, <= and BETWEEN are allowed. 
- <> is NOT allowed. 
- LIKE queries of the form ‘ZZZ%’ and ‘ZZ%ZZ%’ are also allowed. 
- LIKE queries of the form ‘%ZZZ%’ are NOT allowed. 
- Queries of the form “lastName LIKE firstName” are NOT allowed (the second value is not a constant).

### Hashing
Hashing is also used to group by column's hash. Remember that hashing could cause collisions, so the more unique the column is the better.

```sql
CREATE INDEX PersonBirthDate 
ON Person(birthDate) [USING {BTREE | HASH}];
```

note that the db may just use one method because its limited for example our laptops may just use btree only


- =, <> are allowed.
- the rest is NOT allowed
