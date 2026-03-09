# Introduction to MongoDB

MongoDB is the database we will use for the rest of this course. This chapter covers getting it running, understanding its core terminology, and using the Mongo shell to interact with a database directly.

## Installing and Running MongoDB

There are two ways to run MongoDB: locally on your machine, or through a cloud service called MongoDB Atlas. In this course we run it locally.

### Installing MongoDB Community Edition

Follow the official installation guide for your operating system at [https://www.mongodb.com/docs/manual/installation/](https://www.mongodb.com/docs/manual/installation/). Install the **Community Edition**.

The installation includes:
- `mongod`: the MongoDB server process (the database itself)
- `mongosh`: the MongoDB shell (a command-line tool for interacting with the database)

### Starting the Server

On most systems, MongoDB runs as a background service after installation. You can verify it is running by opening a terminal and typing:

```bash
mongosh
```

If it connects successfully, MongoDB is running. If you get a connection error, you may need to start the server manually:

```bash
mongod
```

Keep this terminal open while you work. The server needs to be running for anything to connect to it.

### Connection String

By default, MongoDB listens on `localhost` at port `27017`. The connection string for a local instance is:

```
mongodb://localhost:27017
```

You will use this string when connecting from the shell or from Node.js.

## Core Terminology

MongoDB uses different terminology than relational databases, but the concepts map closely:

| Relational (SQL) | MongoDB |
|-------------------|---------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |

A **database** holds one or more collections. A **collection** holds documents. A **document** is a single record made up of fields and values, stored as a JSON-like object.

MongoDB creates databases and collections automatically when you first write data to them. You do not need to create them in advance.

## The Mongo Shell

`mongosh` is an interactive command-line tool for working with MongoDB. It lets you run queries, insert data, and manage databases without writing a full program.

### Selecting a Database

To switch to a database (or create one by using it for the first time):

```
use myDatabase
```

### Viewing Databases and Collections

```
show dbs
show collections
```

`show dbs` only lists databases that contain data. An empty database will not appear.

## Inserting Documents

To add a document to a collection, use `insertOne`:

```js
db.students.insertOne({
    name: "Linh",
    program: "COMP",
    year: 1
})
```

If the `students` collection does not exist yet, MongoDB creates it automatically. The result includes the generated `_id` for the new document.

To insert multiple documents at once:

```js
db.students.insertMany([
    { name: "Ravi", program: "BUSN", year: 2 },
    { name: "Sophie", program: "COMP", year: 1 },
    { name: "Marcus", program: "COMP", year: 3 }
])
```

## Querying Documents

To retrieve documents from a collection, use `find` or `findOne`.

`findOne` returns a single document matching the query:

```js
db.students.findOne({ name: "Linh" })
```

`find` returns all matching documents:

```js
db.students.find({ program: "COMP" })
```

Calling `find` with no arguments returns every document in the collection:

```js
db.students.find()
```

These are the basics for getting data in and out of MongoDB from the shell. The following chapters cover CRUD operations in more detail, and then how to do all of this from Node.js.
