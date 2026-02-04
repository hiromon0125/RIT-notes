
## Exists

```json
{field: {
	$exists: <boolean>
}}
```
whether the field exists or not

## Array Containment

```json
{<array_field>: <value>}
```
you can check for any array field with value greater than ...
```json
{<array_field>: {$gt: ...}}
```
if you want to check for array is length n or more:
i.e. length 777 or more
```json
{<array_field>.776: {$exists: true}}
```

## Aggregation pipeline

```js
db.collection.aggregate(
	[<stage1>, <stage2>, ...]
)
```
- mongo is unfortunately a declarative syntax making us the optimizer

## Project operation
```json
{$project {<specification(s)>}}
```
specification could be the following:
- `<field>: <1 or true>, <field>: <0 or false>`
- `_id: <0 or false>`
- `<field>: <expression>`

```js
db.bills.aggregate([ 
	{ $project : { 
		“address.zipcode” : 1, 
		“amount” : 1, 
		“patient” : 1 
	} } ] )
```
would result in 
```js
{ 
	_id : “5099803df3f4948bd2f98391”, 
	“address” : { “zipcode” : “14534” }, 
	“amount” : 756.98, 
	“patient” : “376-97-9845” 
}
```
`_id` is queried by default, so to throw it away it must be set to 0

## Match operation
```json
{$match: {<query>}}
```
filters data
```js
db.bills.aggregate( [ 
	{ $match : { “address.zipcode” : “14534” } } 
] )
```

## Group operation
```json
{$group: {
	_id: <expression>, 
	<field1>: { <accumulator1: <expression1> }, 
	...
}}
```
for example
```json
{$group: { _id: "$cust_id", total: {$sum: "$amount"}}}
```
group by customer id and sum the amount attribute.

