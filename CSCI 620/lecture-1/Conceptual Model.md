
## Relational model

Entities
- actual object in the database
- Singular or plural
	- Each entity should be singular

Entity set attribute
- property or description of entity
- issues with attributes
	- make sure things like SSN or HIPPA regulated data is private or securely stored

relationships
- description between two entities
- "in" or "has" is the only relationship is the ones we will use in this class
- Binary relationship sets
	- relation with two entities
- Non-binary relationship sets
	- relationships that involves more than two entities
	- no going to be used in this class

roles
- another description of entity that specifies each entities from another group of roles

cardinalities
1. 1 to 1 - I have an id card
2. 1 to N - doctor have many patients
3. M to N - bills may be broken up into many payments

inheritance
- doctor and patient could inherit from person
- doctor could have an extra salary attribute for example
	- in which doctor would have all the attribute of person and the extra attribute


Types of Keys
1. super
2. candidate
3. primary

What to do if there are no identifiable keys?
- create an artificial id
	- note that depending on the data size or frequency of new items it could be difficult to create unique ids
	- in which case we should describe using other items or attributes

