# Module 2: Working with Databases

Most web applications need to store data somewhere. Whether it is user accounts or product listings, that data lives in a database. In this module, we cover how to work with databases using MongoDB, a NoSQL document database that stores data in a format that maps naturally to JavaScript objects.

We start with what NoSQL means and how it compares to relational databases, then move into MongoDB itself: installing it, working with the shell, performing CRUD operations, and connecting to it from Node.js.

## SQL vs NoSQL

There are two broad categories of databases you will encounter: SQL (relational) and NoSQL (non-relational).

**SQL databases** like MySQL and PostgreSQL store data in tables with fixed columns and rows. You define a schema up front that determines exactly what fields each record must have. Data is queried using SQL (Structured Query Language), and relationships between tables are handled through joins.

**NoSQL databases** take a different approach. Instead of rigid tables and schemas, they use flexible structures like documents, key-value pairs, or graphs. MongoDB is a document database, meaning each record is stored as a document (a JSON-like object) inside a collection. Documents in the same collection do not need to have the same fields.

SQL databases enforce structure and are well-suited for data with clear relationships between tables. NoSQL databases are more flexible. If your data does not fit neatly into rows and columns, or if the shape of your data changes over time, a document database can be easier to work with.

## Why MongoDB?

MongoDB pairs well with JavaScript and Node.js. Documents in MongoDB are stored in a format called BSON (Binary JSON), which maps directly to JavaScript objects. This means the data you read from the database looks like the objects you are already working with in your code.

MongoDB is also widely used in industry and has a mature Node.js driver, making it a practical choice for learning how databases fit into web applications.

## What We Will Cover

By the end of this module, you should be able to design a simple document schema, perform CRUD operations (Create, Read, Update, Delete), and connect to MongoDB from a Node.js application.
