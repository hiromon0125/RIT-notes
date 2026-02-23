
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
filters data, for example
```js
db.bills.aggregate( [ 
	{ $match : { "address.zipcode" : “14534” } } 
] )
```

## Group operation
```json
{$group: {
	_id: "<expression>", 
	"<field1>": { "<accumulator1>": "<expression1>" }, 
	...
}}
```
for example
```json
{$group: { _id: "$cust_id", total: {$sum: "$amount"}}}
```
group by customer id and sum the amount attribute.



## Lookup fields

```json
{
	$lookup: {
		from: "‹collection to join›",
		localField: "<field from the input documents>",
		foreignField: '‹field from the documents of the "from" collection›',
		as: "‹output array field>"
	}
}
```

- Similar to joins in SQL.
- "from" is collection name
- "localField" is current collection's attribute to search with
- "foreignField" is from_collection's attribute to match with
- "as" field name of the results to put it as. Note that the result is always an array.


## Unwind operation

Expands array field

```json
{$unwind: "<field path"}
```

Say inventory collection:
```json
{
"_id": 1, "item": "ABC1", "sizes": ["S", "M", "L"]
}
```
and aggregate via
```js
db.inventory.addregate( [ { $unwind: "$sizes" } ] )
```

Result in 
```json
{"_id": 1, "item": "ABC1", "sizes": "S"},
{"_id": 1, "item": "ABC1", "sizes": "M"},
{"_id": 1, "item": "ABC1", "sizes": "L"}
```


## Optimization with lookup operations

If you have a pipeline that given 

