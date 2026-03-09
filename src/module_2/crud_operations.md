# CRUD Operations

CRUD stands for Create, Read, Update, Delete. These are the four fundamental operations for working with data in any database. MongoDB provides methods for each one, and they all follow a similar pattern: you call a method on a collection and pass in a document or query.

The examples in this chapter use the Mongo shell (`mongosh`), but the method names and syntax are the same when using the Node.js driver.

## Create

### insertOne

`insertOne` adds a single document to a collection:

```js
db.employees.insertOne({
    name: "Kenji",
    department: "Engineering",
    salary: 72000
})
```

The result object includes an `insertedId` field containing the `_id` that MongoDB assigned to the new document.

### insertMany

`insertMany` adds multiple documents at once. Pass in an array of documents:

```js
db.employees.insertMany([
    { name: "Dana", department: "Marketing", salary: 65000 },
    { name: "Olga", department: "Engineering", salary: 78000 },
    { name: "Carlos", department: "Marketing", salary: 61000 }
])
```

The result includes an `insertedIds` object mapping the index of each document to its generated `_id`.

## Read

### findOne

`findOne` returns the first document that matches a query:

```js
db.employees.findOne({ name: "Kenji" })
```

If no document matches, `findOne` returns `null`.

### find

`find` returns all documents that match a query:

```js
db.employees.find({ department: "Engineering" })
```

With no arguments, `find` returns every document in the collection:

```js
db.employees.find()
```

### Query Filters

The object you pass to `find` or `findOne` is a query filter. It specifies which documents to match. A filter with multiple fields requires all of them to match:

```js
db.employees.find({ department: "Engineering", salary: 72000 })
```

This returns only documents where the department is "Engineering" **and** the salary is 72000.

MongoDB also supports comparison operators for more flexible queries. These are covered in the Data Modelling and Queries chapter.

## Update

### updateOne

`updateOne` takes two arguments: a filter (which document to find) and an update (what to change):

```js
db.employees.updateOne(
    { name: "Kenji" },
    { $set: { salary: 75000 } }
)
```

The result includes `matchedCount` (how many documents matched the filter) and `modifiedCount` (how many were actually changed).

### updateMany

`updateMany` modifies all documents that match a filter:

```js
db.employees.updateMany(
    { department: "Marketing" },
    { $set: { department: "Growth" } }
)
```

This renames the department from "Marketing" to "Growth" for every matching employee.

### Update Operators

The `$set` operator replaces the value of a field, or adds the field if it does not exist. MongoDB has several other update operators:

| Operator | Description |
|----------|-------------|
| `$set` | Sets the value of a field |
| `$unset` | Removes a field from the document |
| `$inc` | Increments a numeric field by a specified amount |
| `$push` | Adds an element to an array field |
| `$pull` | Removes an element from an array field |

```js
// Give Kenji a raise of 5000
db.employees.updateOne(
    { name: "Kenji" },
    { $inc: { salary: 5000 } }
)
```

```js
// Add a skill to an employee's skills array
db.employees.updateOne(
    { name: "Olga" },
    { $push: { skills: "Python" } }
)
```

```admonish warning title="Always Use Update Operators"
When updating, always use an operator like `$set`. If you pass a plain object without an operator, MongoDB will replace the entire document with that object, removing all other fields. This is almost never what you want.
```

## Delete

### deleteOne

`deleteOne` removes the first document that matches a filter:

```js
db.employees.deleteOne({ name: "Carlos" })
```

The result includes `deletedCount`, which tells you how many documents were removed (either 0 or 1).

### deleteMany

`deleteMany` removes all documents that match a filter:

```js
db.employees.deleteMany({ department: "Growth" })
```

To delete every document in a collection, pass an empty filter:

```js
db.employees.deleteMany({})
```

```admonish warning title="Be Careful with deleteMany"
`deleteMany({})` removes every document in the collection. There is no undo. Always double-check your filter before running a delete operation.
```
