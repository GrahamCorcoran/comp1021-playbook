# Introduction to NoSQL

NoSQL stands for "Not Only SQL". It refers to a broad category of databases that do not use the traditional table-based structure of relational databases. There are several types of NoSQL databases (document, key-value, graph, column-family), but in this course we focus on document databases.

## What is NoSQL?

In a relational (SQL) database, data is organized into tables. Each table has a fixed set of columns, and each row represents one record. If you want to store student information, you define a table with columns like `name`, `program`, and `year`, and every student record must follow that structure.

```
| name     | program | year |
|----------|---------|------|
| Amara    | COMP    | 1    |
| Jin      | BUSN    | 2    |
| Fatima   | COMP    | 3    |
```

In a NoSQL document database, there are no tables or fixed columns. Instead, each record is a **document**, which is a self-contained object that holds its own fields and values. Documents are grouped into **collections**, which are roughly equivalent to tables.

```js
{ name: "Amara", program: "COMP", year: 1 }
{ name: "Jin", program: "BUSN", year: 2, gpa: 3.8 }
{ name: "Fatima", program: "COMP", year: 3 }
```

Notice that the second document has a `gpa` field that the others do not. This is valid. Documents in the same collection do not need to have identical fields.

## Document Databases

A document database stores data as individual documents. Each document is a structured object containing field-value pairs. You can think of a document as a single JavaScript object.

MongoDB is the document database we use in this course. It organizes data into three levels: a **database** contains **collections**, and each collection contains **documents**. A single MongoDB server can host multiple databases.

For example, a `school` database might have a `students` collection and a `courses` collection. Each student would be one document in the `students` collection.

## JSON and BSON

MongoDB documents look like JSON (JavaScript Object Notation), the data format you have likely seen when working with JavaScript objects:

```json
{
    "name": "Tariq",
    "program": "COMP",
    "year": 2,
    "courses": ["COMP1021", "COMP205"]
}
```

Internally, MongoDB stores documents in BSON (Binary JSON). BSON is a binary representation of JSON that supports additional data types like dates, ObjectIds, and binary data. You do not need to work with BSON directly. When you read and write documents using the MongoDB driver or shell, you work with regular JavaScript objects. MongoDB handles the conversion to and from BSON behind the scenes.

One BSON type worth knowing about is `ObjectId`. Every document in MongoDB has a unique `_id` field. If you do not provide one when inserting a document, MongoDB generates an `ObjectId` automatically:

```js
{
    _id: ObjectId("65f1a2b3c4d5e6f7a8b9c0d1"),
    name: "Tariq",
    program: "COMP"
}
```

The `_id` field acts as the primary key for each document.

## When to Use NoSQL vs SQL

The choice depends on what you are building.

SQL databases work well when your data has a consistent structure and you need to enforce relationships between tables, like orders that reference customers. If data integrity and strict validation matter, SQL gives you those guarantees.

NoSQL document databases are a better fit when the shape of your data varies between records, or when you are working with nested data like a blog post with embedded comments. They also let you iterate without defining a rigid schema up front.

Many applications use both. For this course, we focus on MongoDB to learn how document databases work and how they integrate with Node.js.
