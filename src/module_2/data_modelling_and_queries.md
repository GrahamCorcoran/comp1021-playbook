# Data Modelling and Queries

Up to this point, we have worked with simple documents and basic filters. Real applications have more complex data and need more flexible ways to query it. This chapter covers how to design your document structure and how to write queries that go beyond exact matches.

## Designing Document Schemas

MongoDB does not enforce a schema, but that does not mean you should store data without any structure in mind. How you organize your documents affects how easy they are to query and how well your application performs.

The main decision when designing a schema is whether to **embed** related data inside a document or **reference** it from a separate collection.

### Embedding vs Referencing

**Embedding** means storing related data directly inside a document:

```js
{
    name: "Nadia",
    email: "nadia@example.com",
    address: {
        street: "42 Oak Avenue",
        city: "Toronto",
        province: "ON"
    }
}
```

The address is part of the person document. You get everything in a single read.

**Referencing** means storing related data in a separate collection and linking to it by `_id`:

```js
// In the "orders" collection
{
    item: "Laptop",
    quantity: 1,
    customerId: ObjectId("65f1a2b3c4d5e6f7a8b9c0d1")
}

// In the "customers" collection
{
    _id: ObjectId("65f1a2b3c4d5e6f7a8b9c0d1"),
    name: "Kofi",
    email: "kofi@example.com"
}
```

The order does not contain the customer's details. It stores a reference to the customer document.

Embedding works well when the related data belongs to the parent and you almost always need it together. An address on a person document is a good example. You would rarely look up an address without also wanting the person.

Referencing makes more sense when the related data is shared. A product might be referenced by hundreds of orders, and you would not want a copy of the full product details embedded in every order document. Referencing is also better when the related data grows over time, like comments on a post.

The right choice depends on how your application reads and writes data. When in doubt, start with embedding and pull things into separate collections if it becomes a problem.

## Query Operators

Basic queries match exact values. Query operators let you match based on comparisons, ranges, and patterns.

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `$eq` | Equal to (same as a plain value match) |
| `$ne` | Not equal to |
| `$gt` | Greater than |
| `$gte` | Greater than or equal to |
| `$lt` | Less than |
| `$lte` | Less than or equal to |
| `$in` | Matches any value in an array |
| `$nin` | Matches none of the values in an array |

```js
// Find employees earning more than 70000
db.employees.find({ salary: { $gt: 70000 } })

// Find students in year 1 or 2
db.students.find({ year: { $in: [1, 2] } })
```

You can combine multiple operators on the same field. This finds products priced between 10 and 50:

```js
db.products.find({ price: { $gte: 10, $lte: 50 } })
```

### Logical Operators

You can combine conditions using logical operators:

| Operator | Meaning |
|----------|---------|
| `$and` | All conditions must match |
| `$or` | At least one condition must match |

```js
// Find Engineering employees earning over 70000
db.employees.find({
    $and: [
        { department: "Engineering" },
        { salary: { $gt: 70000 } }
    ]
})
```

```js
// Find employees in Engineering or Marketing
db.employees.find({
    $or: [
        { department: "Engineering" },
        { department: "Marketing" }
    ]
})
```

```admonish note title="Implicit $and"
When you include multiple fields in a query filter, MongoDB treats it as an implicit `$and`. These two queries are equivalent:

    db.employees.find({ department: "Engineering", salary: { $gt: 70000 } })

    db.employees.find({ $and: [{ department: "Engineering" }, { salary: { $gt: 70000 } }] })

You only need explicit `$and` when you have multiple conditions on the same field.
```

## Sorting and Limiting

When querying, you can control the order and number of results.

### sort

`sort` orders the results by a field. Use `1` for ascending and `-1` for descending:

```js
// Sort by salary, highest first
db.employees.find().sort({ salary: -1 })
```

```js
// Sort by name alphabetically
db.employees.find().sort({ name: 1 })
```

### limit

`limit` restricts the number of documents returned:

```js
// Get the top 3 highest-paid employees
db.employees.find().sort({ salary: -1 }).limit(3)
```

You can chain `sort` and `limit` together. The sort is applied first, then the limit.

### skip

`skip` skips a number of documents before returning results. Combined with `limit`, it enables pagination:

```js
// Skip the first 10 results and return the next 5
db.employees.find().skip(10).limit(5)
```

## Projection

By default, queries return every field in the matching documents. Projection lets you specify which fields to include or exclude.

Pass a second argument to `find` or `findOne` with `1` to include a field or `0` to exclude it:

```js
// Return only name and salary (and _id, which is included by default)
db.employees.find({}, { name: 1, salary: 1 })

// Return everything except the salary field
db.employees.find({}, { salary: 0 })
```

You cannot mix inclusion and exclusion in the same projection, with one exception: you can always explicitly exclude `_id` alongside included fields (e.g., `{ name: 1, _id: 0 }`).

Projection is useful when documents are large and you only need a few fields.
