
## Closure property


## Remove redudant

```sql
SELECT DISTINCT lastName
FROM Doctor
```

This is a n^2 operation which makes them very un-optimal.

## Arithmetic operation 

```SQL
SELECT 
	lastName, salary * 1.1
FROM Doctor;
```

this will name the column "salary * 1.1"

So rename using AS syntax

```SQL
SELECT 
	lastName, salary * 1.1 AS projected
FROM Doctor;
```

## Star

```SQL
SELECT *
FROM Doctor;
```

if we dont know or care about the column names of the tables we can use * syntax to refer to all fo the m


## Conditions

```sql
SELECT ssn, firstName
FROM Doctor WHERE
	(salary < $ 130K AND lastName = 'Patel')
	OR ...
```

## Sorting


## CRUD



