
## Dataformat
MongoDB is based on JSON or lightweight data-interchange, but is BSON types which is more types than JSON supports.

## Sample Document
- document has "fields" not "attributes"
- fields can also be another objects or lists
- object keys needs to be unique within that objects

## Insert Doc

```nosql
db.table.insert(
	"_id": ObjectId("..."),
	"address": {
		"key": "value..."
	}
)
```

> Data Lake: data dump and "fish" data out

## Validator(Schema)
After mongo 5.x you can now add validator that forms the table to a restricted type.