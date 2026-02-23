
- Measures
	- central tendency: how aggregated the dp are
	- variation: how spread out the dp are from mean
- Arithmetic mean
	- $\bar{x}=\frac{1}{N}\sum_{i=1}^{N} x_{i}$
	- Regular mean formula
- Geometric mean
	- $\sqrt[n]{ x_{1}x_{2}\dots x_{n} }$
	- ![[Screenshot 2026-02-18 at 11.12.28.png|100]]
- Mode
	- N data points which takes value $y_i$, the mode is the most frequent value
	- can be multiple
- Median
	- uk what it is
- Quartile
	- also duh
- Quantile
	- if we have $y_{1}, \dots,y_{n}$, the p-th quantile can be computed as
	- $k = \left\lfloor  \frac{n*p}{100}  \right\rfloor$
	- 

## SQL

### Window function

```sql
SELECT value,
ROW_NUMBER() over 
(ORDER BY value) AS position
FROM Relation
```

```sql
SELECT value,
COUNT(*) OVER() AS total_count
FROM Relation
```

## MONGO

